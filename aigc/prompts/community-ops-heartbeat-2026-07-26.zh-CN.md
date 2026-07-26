# mss-boot-io 社区运营巡检记忆 - 2026-07-26

## 背景

- 自动化任务：`mss-boot-community-ops-triage`
- 记录时点：2026-07-26 09:36:41 Asia/Shanghai
- UTC 基线：2026-07-26T01:30:20.979Z
- 数据范围：只看 `mss-boot-io` 四个核心开源仓库：`mss-boot`、`mss-boot-admin`、`mss-boot-admin-antd`、`mss-boot-docs`
- 数据来源：GitHub public REST、公开 Discussions 页面、GitHub connector workflow job logs、当前本地工作区状态
- 权限状态：`gh auth status` 仍返回未登录；GitHub connector 可以读 workflow job logs，但创建 PR 仍对 `mss-boot-docs` 返回 `422 must be a collaborator`
- 约束：外部非 GitHub 平台只准备草稿，不在没有维护者当次确认的情况下发布；持久记忆必须写入 `mss-boot-docs/aigc`

## 巡检结论

- GitHub Discussions 公开列表页仍主要显示 2026-06-08 的社区种子讨论，以及历史讨论 `#99 How to generate a data table？`；本轮未观察到 2026-07-26 新人工互动。
- 自 2026-07-24T01:32:15Z 后，公开 issue/PR comments 中仅观察到机器人评论：
  - `mss-boot#399` 由 Dependabot 标记为已有其他更新路径，本轮公开 PR 列表已不再显示该 PR。
  - `mss-boot-admin#417` 有 `github-actions[bot]` 覆盖率评论，总覆盖率 `15.4%`。
- 当前不能执行 approve、comment、merge、create PR 等写操作；本轮只做读侧治理、风险判断和记忆落盘。
- `mss-boot-docs` 的 2026-07-24 记忆分支仍未能通过 API 创建 PR；本轮继续把 2026-07-26 记忆追加到同一远端分支，便于后续一次性补 PR。

## PR 队列

### mss-boot

- `#402 build(deps): bump the github-actions group across 1 directory with 2 updates`
  - 变更：`.github/workflows/ci.yml`、`.github/workflows/copilot-setup-steps.yml`、`.github/workflows/scorecard.yml`
  - 状态：mergeable `unstable`
  - 检查：CodeQL、Docs Drift、PR Guard、lint、test、Copilot Setup Steps、Analyze 均为 success；`govulncheck` 为 failure
  - 日志结论：`govulncheck` 命中 `GO-2026-5970`，模块 `golang.org/x/text`，当前 `v0.37.0`，修复版本 `v0.39.0`
  - 判断：不是 actions 升级本身导致的失败；需要先处理 `golang.org/x/text` 安全升级，再考虑合并。

- `#401 build(deps): bump google.golang.org/grpc from 1.81.1 to 1.82.1`
  - 变更：`go.mod`、`go.sum`
  - 状态：mergeable `unstable`
  - 检查：CodeQL、Docs Drift、PR Guard、lint、test、Analyze 均为 success；`govulncheck` 为 failure
  - 日志结论：同样命中 `GO-2026-5970` / `golang.org/x/text@v0.37.0`，修复版本 `v0.39.0`
  - 判断：不是 grpc 升级本身导致的失败；需要先处理 `golang.org/x/text` 安全升级，再考虑合并。

- `#397 build(deps): bump golang.org/x/image from 0.38.0 to 0.41.0`
  - 状态：open，前序记忆已确认 checks 全绿且适合合并。
  - 阻塞：当前 GitHub 写权限不可用，暂不能 squash merge。

### mss-boot-admin

- `#417 build(deps): bump google.golang.org/grpc from 1.81.1 to 1.82.1`
  - 变更：`go.mod`、`go.sum`
  - 状态：mergeable `unstable`
  - 检查：CodeQL、Docs Drift、PR Guard、build、Analyze 均为 success；`govulncheck` 为 failure
  - 日志结论：`govulncheck` 命中两项：
    - `GO-2026-5970` / `golang.org/x/text@v0.38.0`，修复版本 `v0.39.0`
    - `GO-2026-5932` / `golang.org/x/crypto/openpgp`，修复版本 N/A
  - 判断：`#417` 不能直接合并。`x/text` 可通过依赖升级处理；`openpgp` 仍是需要代码迁移或隔离的安全 blocker。

- `#415 build(deps): bump actions/setup-go from 6 to 7 in the github-actions group`
  - 状态：继续视为 blocked queue；同样受 `govulncheck` blocker 影响，不应因为 build/CodeQL 通过而合并。

### mss-boot-admin-antd

- `#128 chore(deps): bump immutable from 3.8.3 to 4.3.9`
  - 状态：mergeable `clean`
  - 检查：CodeQL、Docs Drift、PR Guard、build、Analyze 均为 success
  - 判断：仍是安全依赖治理候选；技术上适合在恢复写权限后优先 squash merge。合并 main 不等于发布 Cloudflare，仍需遵守前端发布节奏。

- `#124 chore(deps): bump actions/setup-node from 6 to 7 in the github-actions group across 1 directory`
  - 状态：mergeable `clean`
  - 检查：CodeQL、Docs Drift、PR Guard、build、Copilot Setup Steps、Analyze 均为 success
  - 判断：技术上适合合并；属于 GitHub Actions runtime 更新。

### mss-boot-docs

- `#59 chore(deps): bump actions/setup-node from 6 to 7 in the github-actions group across 1 directory`
  - 状态：mergeable `clean`
  - 检查：CodeQL、Docs Drift、PR Guard、build、Copilot Setup Steps、Analyze 均为 success
  - 判断：技术上适合合并。

- `#55 chore(deps-dev): bump the npm-minor-patch group across 1 directory with 5 updates`
  - 状态：mergeable `clean`
  - 更新：`@commitlint/cli`、`@commitlint/config-conventional`、`dumi`、`lint-staged`、`prettier`
  - 检查：CodeQL、Docs Drift、PR Guard、build、Analyze 均为 success
  - 判断：技术上适合合并；lockfile 变更较大，但 CI 已通过。

- `#45 docs: add toolchain migration impact inventory`
  - 外部贡献者历史 PR；前序已有 review 和变更请求。
  - 判断：保持人工 review 队列，不在本轮自动合并。

## Workflow 与安全事项

### mss-boot

- `#401` 和 `#402` 的 `govulncheck` 失败均指向 `golang.org/x/text` 的 `GO-2026-5970`。
- 建议新增 focused PR：将 `golang.org/x/text` 提升到 `v0.39.0+`，跑 `go test ./...`、CI、govulncheck 后再回头处理 `#401/#402`。

### mss-boot-admin

- `#417` 的 `govulncheck` 同时暴露 `x/text` 与 `openpgp` 两类问题。
- `x/text` 可以走依赖升级；`openpgp` 需要继续从 `pkg/gist.go` 引入链路评估替代库、功能降级或隔离策略。
- 这类 blocker 不应压到 Dependabot PR 本身，应拆成维护者可 review 的安全专项 PR。

### mss-boot-admin-antd

- `#124` 和 `#128` 都是 clean queue。
- Dynamic Dependabot 中 `fast-uri`、`hono`、`svgo`、`@hono/node-server`、`js-yaml` 仍有失败记录，需后续按日志拆分处理；当前最明确可先合并的是 `#128` 与 `#124`。

### mss-boot-docs

- `#59` 和 `#55` 是 clean queue。
- 最近 Dependabot dynamic `immutable` 失败日志显示：`latest-resolvable-version` 为 `4.3.8`，`lowest-non-vulnerable-version` 为 `4.3.9`，类型为 `security_update_not_possible`。
- 本地 `pnpm-lock.yaml` 中 `sass@1.54.0` 仍指向 `immutable@4.3.8`，`sass@1.100.0` 指向 `immutable@5.1.6`；建议优先合并 `#55` 后复跑 Dependabot，再决定是否增加 `pnpm.overrides.immutable = 4.3.9` 或升级上游链路。

## 外部社区草稿

### 中文平台草稿

标题：mss-boot 开源治理更新：依赖升级队列与安全扫描进展

正文要点：

- 本轮继续巡检 `mss-boot`、`mss-boot-admin`、`mss-boot-admin-antd`、`mss-boot-docs` 四个核心仓库。
- 前端和文档侧已有多项 clean 依赖 PR：`admin-antd#128/#124`、`docs#59/#55`，适合恢复写权限后优先合并。
- Go 侧当前主要问题不是单个 Dependabot PR，而是 `govulncheck` 暴露的 `golang.org/x/text` 安全升级，以及 `mss-boot-admin` 中 `openpgp` 的替换/隔离工作。
- 欢迎社区参与 review：Go 安全依赖、docs 工具链、前端依赖兼容性、Dependabot dynamic failure 复核。
- 反馈请尽量沉淀到 GitHub Issues / Discussions，便于形成可追踪任务。

### English Draft

Title: mss-boot governance update: dependency queue and security scan follow-up

Key points:

- The maintenance sweep covered `mss-boot`, `mss-boot-admin`, `mss-boot-admin-antd`, and `mss-boot-docs`.
- Several frontend/docs dependency PRs are clean merge candidates once write access is restored: `admin-antd#128/#124` and `docs#59/#55`.
- The Go-side blocker is now clear from `govulncheck`: `golang.org/x/text` needs a security upgrade, and `mss-boot-admin` still needs an `openpgp` replacement or isolation plan.
- Reviewer help is useful on Go security dependencies, docs tooling, frontend dependency compatibility, and Dependabot dynamic update failures.
- Please keep durable feedback in GitHub Issues or Discussions so it can become tracked work.

## 下轮建议

- 恢复 GitHub 写权限或 `gh auth login` 后，优先创建/合并本记忆分支的 docs PR。
- 恢复写权限后按低风险顺序 squash merge：`mss-boot-admin-antd#128`、`mss-boot-admin-antd#124`、`mss-boot-docs#59`、`mss-boot-docs#55`、`mss-boot#397`。
- 单独开安全专项处理 `mss-boot` 的 `golang.org/x/text@v0.37.0 -> v0.39.0+`，通过后再复核 `mss-boot#401/#402`。
- 单独开安全专项处理 `mss-boot-admin` 的 `golang.org/x/text@v0.38.0 -> v0.39.0+` 与 `openpgp` 引入链路。
- 对外部平台只保留草稿，等维护者当次确认再发布。
