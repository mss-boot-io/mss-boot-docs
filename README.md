# mss-boot-docs

`mss-boot-docs` 是 `mss-boot` 与 `mss-boot-admin` 的文档站点，基于 [dumi](https://d.umijs.org) 构建。

当前文档重点围绕以下方向组织：

- `mss-boot` 框架能力
- `mss-boot-admin` 的治理、运营与扩展能力
- 单租户后台系统的启动、部署、测试与演进路线
- AI 注解协同相关文档沉淀

## 推荐阅读顺序

如果你主要关注 `mss-boot-admin`，建议按以下顺序阅读：

1. `/admin` 介绍页
2. `/admin/current-capabilities` 当前功能总览
3. `/admin/phase-4-roadmap` 四期路线图
4. `/admin/phase-5-roadmap` 五期路线图
5. `/admin/docker` 容器化与生产部署
6. `/admin/production-standardization` 生产部署标准化
7. `/admin/release-verification-checklist` 发布验证清单
8. `/admin/observability-guide` 性能与可观测性指南
9. `/admin/security-baseline` 安全基线指南
10. `/admin/integration-test-guide` 集成测试指南

## 本地开发

```bash
pnpm install
pnpm start
```

默认会启动本地文档站点，用于预览 `docs/` 下的内容。

## 构建

```bash
pnpm run build
```

## 贡献说明

- 文档面向开源协作者，尽量使用可验证、可复现、可讨论的表述。
- 与产品实现相关的结论应尽量基于仓库代码、配置或已验证行为。
- 中文文档优先保持术语一致，避免同一能力出现多种叫法。

## License

MIT
