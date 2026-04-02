---
title: 四期路线图
order: 25
nav:
  order: 1
  title: admin
description: mss-boot-admin 四期演进规划与优先级排序
keywords: [admin roadmap phase4 planning enhancement]
---

## 概述

三期路线图已全部完成，产品主线（治理 + 运营 + AI 注解协同）已清晰。四期重点在于：

1. **运营能力深化**：补齐与竞品差距，提升运维效率
2. **安全能力增强**：补齐存储安全、审计追溯能力
3. **扩展能力完善**：WebSocket 集群、API 文档自动化
4. **国际化完善**：Accept-Language 处理、语言代码校验

## 四期优先级排序

| 优先级 | 能力线 | 说明 | 预计工作量 |
|--------|--------|------|------------|
| P1 | 运行时日志查看 | 运维排障基础能力 | 高 |
| P2 | 监控数据可视化 | 用户体验提升 | 中 |
| P3 | 存储安全增强 | 文件校验、权限控制 | 中 |
| P4 | 告警规则配置 | 主动发现问题 | 中 |
| P5 | WebSocket 集群支持 | 分布式部署 | 高 |
| P6 | API 文档自动化 | Swagger 完善 | 低 |
| P7 | 国际化完善 | Accept-Language、语言代码校验 | 低 |

---

## P1. 运行时日志查看

### 背景

当前系统缺少运行时日志查看能力，运维排障需要登录服务器查看日志文件。

### 目标

提供 Web 界面查看运行时日志，支持过滤、搜索、导出。

### 功能范围

| 功能 | 说明 |
|------|------|
| 日志列表 | 按时间倒序显示最近 N 条日志 |
| 级别过滤 | info / warn / error / debug |
| 关键词搜索 | 支持正则表达式 |
| 时间范围 | 按时间区间筛选 |
| 日志导出 | 导出为文本或 JSON |

### 实现方案

**后端：**
```go
// models/log.go
type LogEntry struct {
    Timestamp time.Time `json:"timestamp"`
    Level     string    `json:"level"`     // info, warn, error, debug
    Message   string    `json:"message"`
    Source    string    `json:"source"`    // 来源模块
    TraceID   string    `json:"traceId"`   // 追踪 ID
}

// apis/log.go
// GET /admin/api/logs - 查询日志
// GET /admin/api/logs/export - 导出日志
```

**日志收集方式：**
- 方案 A：读取本地日志文件（简单，适合单机部署）
- 方案 B：集成日志库（如 logrus/zap hook）
- 方案 C：对接日志系统（如 ELK、Loki）

**推荐：** 方案 A（读取本地文件），后续可扩展为方案 C。

### 前端界面

```
日志管理
├── 过滤栏：级别下拉、时间范围、关键词输入
├── 日志列表：时间、级别、来源、消息
└── 操作：刷新、导出
```

### 验收标准

- [ ] 可查看最近 1000 条日志
- [ ] 可按级别过滤
- [ ] 可按关键词搜索
- [ ] 可按时间范围筛选
- [ ] 可导出日志

---

## P2. 监控数据可视化

### 背景

当前监控数据仅 API 返回 JSON，无前端图表展示。

### 目标

提供可视化监控仪表盘，实时展示系统状态。

### 功能范围

| 功能 | 说明 |
|------|------|
| CPU 趋势图 | 使用率历史曲线 |
| 内存趋势图 | 使用量历史曲线 |
| 磁盘使用图 | 各分区使用率 |
| 网络流量图 | 入站/出站流量 |
| 运行时信息 | Goroutine、GC、堆内存 |
| 实时刷新 | 可配置刷新间隔 |

### 实现方案

**前端图表库选择：**
- 方案 A：ECharts（功能全，生态好）
- 方案 B：Ant Design Charts（与现有技术栈一致）
- 方案 C：Recharts（轻量，React 原生）

**推荐：** 方案 B（Ant Design Charts），与 Ant Design Pro 集成更好。

**后端扩展：**
```go
// apis/monitor.go 新增历史数据接口
// GET /admin/api/monitor/history?duration=1h
```

### 前端界面

```
监控仪表盘
├── 概览卡片：CPU、内存、磁盘、网络概要
├── 趋势图表：CPU/内存历史曲线
├── 磁盘图表：各分区使用率饼图
└── 运行时：Goroutine、GC 次数、堆内存
```

### 验收标准

- [ ] CPU 使用率趋势图
- [ ] 内存使用趋势图
- [ ] 磁盘使用图表
- [ ] 网络流量图表
- [ ] 自动刷新间隔可配置

---

## P3. 存储安全增强

### 背景

当前存储能力缺少安全校验，存在恶意文件上传风险。

### 目标

增加文件类型、大小校验，细粒度权限控制。

### 功能范围

| 功能 | 说明 |
|------|------|
| 文件类型白名单 | 只允许指定类型上传 |
| 文件大小限制 | 单文件最大限制 |
| MIME 类型校验 | 检测真实文件类型 |
| 存储权限控制 | Casbin 权限集成 |
| 上传审计日志 | 记录上传操作 |

### 实现方案

**后端配置：**
```yaml
storage:
  maxSize: 10485760  # 10MB
  allowedTypes:
    - image/jpeg
    - image/png
    - image/gif
    - application/pdf
    - text/plain
  checkMime: true
```

**代码实现：**
```go
// service/storage.go
func (s *Storage) validateFile(file *multipart.FileHeader) error {
    // 大小校验
    if file.Size > maxSize {
        return errors.New("file too large")
    }
    // 类型校验
    if !isAllowedType(file.Header.Get("Content-Type")) {
        return errors.New("file type not allowed")
    }
    // MIME 校验
    if checkMime {
        actualType := detectMIME(file)
        if !isAllowedType(actualType) {
            return errors.New("MIME type mismatch")
        }
    }
    return nil
}
```

### 验收标准

- [ ] 文件大小超限时拒绝上传
- [ ] 非白名单类型拒绝上传
- [ ] MIME 类型校验生效
- [ ] 上传操作记录审计日志

---

## P4. 告警规则配置

### 背景

当前系统无告警能力，问题依赖人工发现。

### 目标

支持配置告警规则，触发后通过多种渠道通知。

### 功能范围

| 功能 | 说明 |
|------|------|
| 告警规则 | 指标、阈值、触发条件 |
| 通知渠道 | 系统通知、邮件、钉钉、企微 |
| 告警历史 | 历史告警记录 |
| 告警静默 | 临时屏蔽告警 |

### 实现方案

**数据模型：**
```go
type AlertRule struct {
    ID          string      `json:"id"`
    Name        string      `json:"name"`
    Metric      string      `json:"metric"`      // cpu, memory, disk
    Operator    string      `json:"operator"`    // >, <, >=, <=
    Threshold   float64     `json:"threshold"`
    Duration    int         `json:"duration"`    // 持续时间（秒）
    Channels    []string    `json:"channels"`    // 通知渠道
    Status      string      `json:"status"`      // enabled, disabled
}

type AlertHistory struct {
    ID          string    `json:"id"`
    RuleID      string    `json:"ruleId"`
    TriggeredAt time.Time `json:"triggeredAt"`
    Value       float64   `json:"value"`
    Status      string    `json:"status"`      // firing, resolved
}
```

**告警检查：**
```go
// 定时检查告警规则
func checkAlerts() {
    rules := getEnabledAlertRules()
    for _, rule := range rules {
        value := getMetricValue(rule.Metric)
        if evaluateRule(rule, value) {
            sendAlert(rule, value)
        }
    }
}
```

### 验收标准

- [ ] 可配置告警规则
- [ ] 可配置通知渠道
- [ ] 告警触发后发送通知
- [ ] 告警历史可查询

---

## P5. WebSocket 集群支持

### 背景

当前 WebSocket 为单机模式，不支持分布式部署。

### 目标

支持多实例部署，会话共享。

### 实现方案

**方案选择：**
- 方案 A：Redis Pub/Sub（简单，依赖 Redis）
- 方案 B：消息队列（Kafka/RabbitMQ）
- 方案 C：etcd Watch

**推荐：** 方案 A（Redis Pub/Sub），已有 Redis 依赖。

**架构：**
```
客户端 -> Nginx -> [WS实例1, WS实例2, WS实例3]
                      |
                      v
                   Redis Pub/Sub
```

### 验收标准

- [ ] 多实例部署后消息正常推送
- [ ] 用户连接任一实例都能收到消息
- [ ] 实例故障时连接自动转移

---

## P6. API 文档自动化

### 背景

当前 Swagger 注解不完整，API 文档不全面。

### 目标

完善 Swagger 注解，自动生成完整 API 文档。

### 功能范围

- 补充所有 API 的 Swagger 注解
- 统一响应格式定义
- 错误码文档
- 接口示例

### 验收标准

- [ ] 所有 API 有 Swagger 注解
- [ ] Swagger UI 可访问
- [ ] 响应格式有示例

---

## P7. 国际化完善

### 背景

当前国际化缺少 Accept-Language 处理和语言代码校验。

### 功能范围

- 中间件处理 Accept-Language 请求头
- 语言代码格式校验（ISO 639-1）
- 语言变更审计日志

### 验收标准

- [ ] 请求自动匹配 Accept-Language
- [ ] 语言代码格式强制校验
- [ ] 语言变更记录审计日志

---

## 四期里程碑

| 里程碑 | 内容 | 预计完成 |
|--------|------|----------|
| M1 | 运行时日志查看 + 监控可视化 | 第 2 周 |
| M2 | 存储安全增强 + 告警配置 | 第 4 周 |
| M3 | WebSocket 集群 + API 文档 | 第 6 周 |
| M4 | 国际化完善 + 收尾 | 第 8 周 |

---

## 推荐阅读

- [三期路线图](/admin/phase-3-roadmap)
- [运营能力说明](/admin/operations-guide)
- [运营能力规划](/admin/operations-planning)
- [集成与扩展护栏](/admin/extension-guardrails)