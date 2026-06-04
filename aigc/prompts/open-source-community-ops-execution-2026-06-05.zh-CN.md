# mss-boot-io 开源社区运营执行记录

## 执行时间

- 执行日期：2026-06-05
- 执行人：Codex leader agent，使用本地已登录的 `gh` 身份 `lwnmengjing`
- 范围：`mss-boot-io/.github`、`mss-boot`、`mss-boot-admin`、`mss-boot-admin-antd`、`mss-boot-docs`
- 原则：只执行无需外部账号额外决策、无需密钥、无需部署、不会自动合并代码的社区运营动作；涉及 GitHub settings、Cloudflare、Sponsors、Discussions、Projects、branch protection、private vulnerability reporting 的事项继续记入待办。

## 已直接执行的公开动作

### 1. 同步 labels taxonomy

已将组织 `.github/.github/labels.yml` 中的标签同步到以下仓库，使用 `gh label create --force` 幂等创建/更新，不删除旧标签：

- `mss-boot-io/.github`
- `mss-boot-io/mss-boot`
- `mss-boot-io/mss-boot-admin`
- `mss-boot-io/mss-boot-admin-antd`
- `mss-boot-io/mss-boot-docs`

核心标签包括：

- `area/backend`
- `area/frontend`
- `area/docs`
- `area/devops`
- `type/bug`
- `type/feature`
- `type/question`
- `type/security`
- `needs-triage`
- `needs-info`
- `candidate/good-first-issue`
- `good first issue`
- `help wanted`
- `release-blocker`
- `breaking-change`

### 2. 创建首批 good first issue

已创建 12 个低风险、可验证、无需部署/密钥/外部账号的 `good first issue`：

| 仓库 | Issue |
| --- | --- |
| `mss-boot-docs` | [#4 docs: add first contribution guide](https://github.com/mss-boot-io/mss-boot-docs/issues/4) |
| `mss-boot-docs` | [#5 docs: add SECURITY policy FAQ](https://github.com/mss-boot-io/mss-boot-docs/issues/5) |
| `mss-boot-docs` | [#6 docs: add troubleshooting page for login failures](https://github.com/mss-boot-io/mss-boot-docs/issues/6) |
| `mss-boot` | [#348 docs: document go test and lint workflow](https://github.com/mss-boot-io/mss-boot/issues/348) |
| `mss-boot` | [#349 docs: add action/controller glossary](https://github.com/mss-boot-io/mss-boot/issues/349) |
| `mss-boot` | [#350 test: cover config env template fallback behavior](https://github.com/mss-boot-io/mss-boot/issues/350) |
| `mss-boot-admin` | [#344 docs: document make test prerequisites](https://github.com/mss-boot-io/mss-boot-admin/issues/344) |
| `mss-boot-admin` | [#345 docs: add RBAC glossary](https://github.com/mss-boot-io/mss-boot-admin/issues/345) |
| `mss-boot-admin` | [#346 test: add language middleware fallback coverage](https://github.com/mss-boot-io/mss-boot-admin/issues/346) |
| `mss-boot-admin-antd` | [#71 docs: add frontend local env matrix](https://github.com/mss-boot-io/mss-boot-admin-antd/issues/71) |
| `mss-boot-admin-antd` | [#72 docs: fix README badges for frontend workflows](https://github.com/mss-boot-io/mss-boot-admin-antd/issues/72) |
| `mss-boot-admin-antd` | [#73 docs: align frontend testing commands with package scripts](https://github.com/mss-boot-io/mss-boot-admin-antd/issues/73) |

每个 issue 都包含：

- 背景
- 修改范围
- 非目标
- 验证命令
- 验收标准

### 3. 历史 issue 轻量分流

对 `mss-boot-admin` 现有 open issue 只做标签分流，不关闭、不自动评论、不替维护者判断结论：

| Issue | 标签处理 |
| --- | --- |
| [#139 部署完成后无法正常登录](https://github.com/mss-boot-io/mss-boot-admin/issues/139) | `needs-triage`、`type/bug`、`area/devops`、`needs-info` |
| [#126 查看在线用户并强退](https://github.com/mss-boot-io/mss-boot-admin/issues/126) | `needs-triage`、`type/feature`、`area/backend`、`help wanted` |
| [#111 通知支持多种 provider](https://github.com/mss-boot-io/mss-boot-admin/issues/111) | `needs-triage`、`type/feature`、`area/backend`、`help wanted` |
| [#105 数据更新后查询未刷新](https://github.com/mss-boot-io/mss-boot-admin/issues/105) | `needs-triage`、`type/bug`、`area/backend`、`needs-info` |
| [#89 migration 支持纯 SQL 和回滚](https://github.com/mss-boot-io/mss-boot-admin/issues/89) | `needs-triage`、`type/feature`、`area/backend` |
| [#87 修改首次 migration](https://github.com/mss-boot-io/mss-boot-admin/issues/87) | `needs-triage`、`type/feature`、`area/backend` |
| [#75 Bug Report](https://github.com/mss-boot-io/mss-boot-admin/issues/75) | `needs-triage`、`type/bug`、`area/backend`、`needs-info` |
| [#73 虚拟模型支持关联模型](https://github.com/mss-boot-io/mss-boot-admin/issues/73) | `needs-triage`、`type/feature`、`area/backend` |
| [#55 数据统计功能实现](https://github.com/mss-boot-io/mss-boot-admin/issues/55) | `needs-triage`、`type/feature`、`area/backend` |
| [#53 手机号登录功能](https://github.com/mss-boot-io/mss-boot-admin/issues/53) | `needs-triage`、`type/feature`、`area/backend` |

### 4. Dependabot PR 队列化

对现有 Dependabot PR 增加 `needs-triage` 与 `area/backend`，不自动合并：

- `mss-boot`：#341、#342、#343、#344、#345、#346、#347
- `mss-boot-admin`：#337、#338、#339、#340、#341

## 不直接执行的事项

以下事项已确认需要外部账号设置、组织权限策略或维护者决策，继续保留在 `open-source-operations-backlog.zh-CN.md`：

- GitHub branch protection / rulesets
- required checks
- private vulnerability reporting
- secret scanning / push protection
- Discussions 分类与启用
- GitHub Projects 看板
- Sponsors / FUNDING
- Cloudflare 发布与域名设置
- 自动关闭历史 issue
- 自动合并 Dependabot PR

## 验收记录

- `gh auth status` 显示已登录 `github.com`，身份为 `lwnmengjing`。
- 四核心仓库均能查询到新建的 `good first issue`。
- `mss-boot-admin` 历史 open issue 已包含新的 taxonomy 标签。
- Dependabot PR 已通过 REST issue labels API 加入 triage 队列标签。

## 后续复核

本次公开运营动作已经直接生效，但本地源码侧社区健康文件、workflow 和文档仍需通过对应仓库的分支/PR 发布到 GitHub 后，GitHub Community Profile 与 Scorecard 才会反映新的治理状态。
