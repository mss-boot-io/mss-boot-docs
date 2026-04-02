---
title: 运营能力说明
order: 19
nav:
  order: 1
  title: admin
description: mss-boot-admin 系统配置、通知公告、定时任务、监控统计等运营能力说明
keywords: [admin operations config notice task monitor statistics]
---

## 概述

本文档描述 `mss-boot-admin` 的运营能力架构，覆盖：

- 系统配置管理
- 通知公告系统
- 定时任务与作业调度
- 系统监控信息
- 统计查询能力

这些能力使 `mss-boot-admin` 不仅是权限管理平台，更是具备日常运营维护能力的后台系统。

## 1. 系统配置管理

### 数据模型

**SystemConfig** (`models/system_config.go`)

```
SystemConfig
├── Name        → 配置文件名称
├── Extension   → 文件类型枚举 (YAML/JSON/TOML/INI)
├── Content     → 配置内容 (文本)
├── Remarks     → 配置说明
├── Status      → 启用状态
```

**存储位置**: `mss_boot_system_configs` 表

### API 入口

| 路径 | 方法 | 功能 |
|------|------|------|
| `/admin/api/system-configs` | GET | 配置列表查询 |
| `/admin/api/system-configs` | POST | 创建新配置 |
| `/admin/api/system-configs/:id` | GET | 获取单个配置 |
| `/admin/api/system-configs/:id` | PUT | 更新配置内容 |
| `/admin/api/system-configs/:id` | DELETE | 删除配置 |

### 使用场景

- 存储系统级 YAML/JSON 配置文件
- 支持多种配置格式，适配不同子系统需求
- 配置内容可在线编辑，无需重启服务
- 与 mss-boot 核心配置系统集成

### 运行机制

配置存储为文本内容，系统通过 `source.Scheme` 解析不同格式，集成到 mss-boot 配置体系中。

## 2. 通知公告系统

### 数据模型

**Notice** (`models/notice.go`)

```
Notice
├── Type        → 通知类型枚举
│   ├── notification → 系统公告
│   ├── message      → 个人消息
│   ├── event        → 事件通知
│   ├── mail         → 邮件通知
├── UserID      → 接收用户
├── Title       → 通知标题
├── Description → 通知内容
├── ReadAt      → 阅读时间 (未读为空)
├── CreatedAt
├── Status
```

**存储位置**: `mss_boot_notices` 表

### API 入口

| 路径 | 方法 | 功能 |
|------|------|------|
| `/admin/api/notice` | GET | 通知列表 (当前用户) |
| `/admin/api/notice/unread` | GET | 未读通知列表 |
| `/admin/api/notice/read/:id` | POST | 标记为已读 |
| `/admin/api/notice` | POST | 创建通知 (管理员) |
| `/admin/api/notice/:id` | DELETE | 删除通知 |

### 实时通知 (WebSocket)

系统支持通过 WebSocket 实时推送通知：

**WebSocket 连接**

| 路径 | 说明 |
|------|------|
| `/admin/api/ws/connect` | WebSocket 连接入口 (需认证) |
| `/admin/api/ws/online` | 在线用户统计 |

**消息格式**

```json
{
  "event": "notify",
  "code": 200,
  "data": {
    "id": "xxx",
    "type": "notification",
    "title": "系统通知标题",
    "description": "通知内容",
    "createdAt": "2024-01-01T00:00:00Z"
  },
  "timestamp": 1704067200
}
```

**事件类型**

| 事件 | 说明 |
|------|------|
| `ping/pong` | 心跳保活 |
| `notify` | 通知推送 |
| `kick` | 强制下线 |
| `join/quit` | 连接管理 |

### 使用场景

- 系统公告发布与推送
- 个人消息通知 (支持实时推送)
- 运营事件提醒
- 集成邮件发送能力

### 运行机制

1. 管理员创建通知后，系统根据 `UserID` 投递给目标用户
2. 用户通过 `/unread` 接口获取未读消息列表
3. 点击通知后调用 `/read` 标记为已读
4. 支持按类型筛选和时间排序

## 3. 定时任务与作业调度

### 数据模型

**Task** (`models/task.go`)

```
Task
├── Provider    → 执行提供者枚举
│   ├── default → 内置 cron 调度
│   ├── k8s     → Kubernetes CronJob
│   ├── func    → 注册函数调用
├── Spec        → 调度表达式 (cron 格式)
├── Command     → 执行命令 (HTTP/脚本/gRPC)
├── Args        → 命令参数
├── Image       → 镜像地址 (K8s 模式)
├── Cluster     → 集群名称 (K8s 模式)
├── Name, Remarks
├── Status      → 启用状态
└── TaskRuns    → 执行历史记录
```

**TaskRun** (执行记录)

```
TaskRun
├── TaskID      → 关联任务
├── Status      → 执行状态 (success/failed)
├── Message     → 执行结果/错误信息
├── StartTime
├── EndTime
```

**存储位置**: `mss_boot_tasks` + `mss_boot_task_runs` 表

### API 入口

| 路径 | 方法 | 功能 |
|------|------|------|
| `/admin/api/task` | GET | 任务列表 |
| `/admin/api/task` | POST | 创建任务 |
| `/admin/api/task/:id` | GET/PUT/DELETE | 任务 CRUD |
| `/admin/api/task/:operate/:id` | POST | 任务操作 (start/stop/run) |
| `/admin/api/task/func-list` | GET | 可用函数列表 |

### 执行提供者

#### Default Provider (内置 cron)

- 使用标准 cron 库实现调度
- 支持 HTTP、gRPC、Shell 脚本执行
- 任务在当前进程内运行

#### K8s Provider (Kubernetes CronJob)

- 自动创建/更新/删除 K8s CronJob 资源
- 支持镜像配置和集群选择
- 适合大规模分布式任务

#### Func Provider (注册函数)

- 通过 `TaskFuncMap` 注册自定义函数
- 任务执行时直接调用 Go 函数
- 适合轻量级内部逻辑

### 使用场景

- 定时数据清理
- 周期性报表生成
- 定时消息推送
- K8s 环境下的容器化任务

### 配置参数

```yaml
# config/task.go
Task:
  Enable: true        # 全局任务开关
  DefaultSpec: "0 * * * *"  # 默认调度表达式
```

### 运行机制

1. 系统启动时加载所有启用的 Task
2. 根据 Provider 类型注册调度器
3. 到达触发时间时执行任务并记录 TaskRun
4. `/operate` 接口支持手动启停和立即执行

## 4. 系统监控

### 监控指标

通过 `gopsutil` 库和 Go 运行时实时采集系统指标：

| 指标类别 | 采集项 |
|----------|--------|
| CPU | 逻辑核心数、物理核心数、使用率、型号信息 |
| 内存 | 总量、已用、可用、空闲、使用率 |
| 磁盘 | 总容量、已用空间、使用率 |
| 网络 | 发送/接收字节数、包数、错误数、丢包数、连接状态统计 |
| 运行时 | 协程数、堆内存、栈内存、GC 统计 |
| 系统信息 | Go 版本、启动时间、运行时长 |

### API 入口

| 路径 | 方法 | 功能 |
|------|------|------|
| `/admin/api/monitor` | GET | 系统监控信息 |

### 返回示例

```json
{
  "cpuPhysicalCore": 4,
  "cpuLogicalCore": 8,
  "cpuInfo": [...],
  "memoryTotal": 16384,
  "memoryUsage": 5120,
  "memoryUsagePercent": 31.25,
  "diskTotal": 500,
  "diskUsage": 150,
  "diskUsagePercent": 30,
  "network": {
    "bytesSent": 1234567,
    "bytesRecv": 7654321,
    "connectionCount": {
      "established": 45,
      "listen": 10,
      "timeWait": 5,
      "closeWait": 2,
      "total": 62
    }
  },
  "runtime": {
    "goroutines": 128,
    "heapAlloc": 52428800,
    "heapSys": 67108864,
    "numGC": 15
  },
  "goVersion": "go1.22.0",
  "startTime": 1704067200,
  "uptime": 86400
}
```

### 使用场景

- 运维仪表盘展示
- 健康检查与告警阈值判断
- 负载评估与容量规划
- 性能调优与问题排查

### 运行机制

每次请求时实时采集，不持久化存储。适合即时监控场景，不适合历史趋势分析。

## 5. 统计查询

### 数据模型

**Statistics** (`models/statistics.go`)

```
Statistics
├── Name        → 统计项名称
├── Type        → 统计类型
├── Value       → 统计值 (*100 精度)
├── Time        → 统计时间
├── Remarks
```

**存储位置**: `mss_boot_statistics` 表

### API 入口

| 路径 | 方法 | 功能 |
|------|------|------|
| `/admin/api/statistics/:name` | GET | 查询指定统计项 |

### 统计接口

**StatisticsObject** (`center/type.go`)

```go
type StatisticsObject interface {
    StatisticsCalibrate()   // 校准值 (设置精确值)
    StatisticsNowIncrease() // 增量
    StatisticsNowReduce()   // 减量
}
```

### 使用场景

- 用户数统计 (`User` 模型已实现)
- 业务指标追踪
- 运营报表数据源

### 运行机制

1. 需要统计的业务模型实现 `StatisticsObject` 接口
2. 通过 `center.StatisticsImp` 调用统计方法
3. 增减时写入 Statistics 表并 *100 提升精度
4. 查询时返回时间序列数据

## 与 HotGo 等项目的对比

| 能力维度 | mss-boot-admin | HotGo |
|----------|----------------|-------|
| 系统配置 | 数据库存储多格式配置 | 配置中心 + 系统参数 |
| 通知系统 | Notice 表 + 类型区分 | WebSocket 实时推送 |
| 定时任务 | Default/K8s/Func 三种 Provider | CronJob + 插件任务 |
| 系统监控 | CPU/Memory/Disk 实时采集 | 服务监控 + 日志系统 |
| 统计查询 | Statistics 表 + 接口实现 | 统计报表 + 可视化 |

**当前差距**

- HotGo 的 WebSocket 实时通知更即时
- HotGo 有更完善的服务日志系统
- HotGo 监控包含堆栈、网络等更多维度

**建议补强方向**

1. 集成 WebSocket 实时通知推送
2. 增加服务日志查询与管理界面
3. 扩展监控维度（网络、堆栈、连接数）
4. 统计数据可视化图表支持

## 推荐阅读

- [产品方向调整](/admin/product-direction)
- [权限与组织治理说明](/admin/governance-guide)
- [当前功能总览](/admin/current-capabilities)
- [三期路线图](/admin/phase-3-roadmap)