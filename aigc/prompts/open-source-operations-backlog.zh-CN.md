# mss-boot-io 开源组织外部运营待办

## 记录时间

- 记录时间：2026-06-05
- 适用范围：`mss-boot-io` GitHub 组织与四个核心仓库
- 背景：本地已补齐可通过源码落地的社区健康、CI、安全扫描和 AI 自动化。以下事项需要 GitHub/Cloudflare/赞助平台等外部账号权限或维护者决策，不能仅靠本地文件完成。

## P0：影响 9.2+ 真实评分的外部设置

| 事项 | 建议配置 | 原因 | 执行人 |
| --- | --- | --- | --- |
| Branch protection / rulesets | 保护 `main`，禁止 force push，要求 PR，要求 CI、CodeQL、govulncheck、Scorecard 通过 | OpenSSF Scorecard 与真实贡献门禁都依赖此设置 | Maintainer |
| Required checks | Go 仓：CI、CodeQL、govulncheck、Scorecard；前端/文档：CI、CodeQL、Scorecard | 防止未验证改动进入主分支 | Maintainer |
| Private vulnerability reporting | 对核心仓库启用 GitHub private vulnerability reporting | 避免漏洞通过 public issue 披露 | Maintainer |
| Security contact | 决定公开安全邮箱或 GitHub Advisories-only 策略 | `SECURITY.md` 当前记录为待定，需最终入口 | Maintainer |
| Secret scanning / push protection | GitHub Advanced Security 可用时开启；免费能力可先依赖 secret scanning alert | 降低密钥泄漏风险 | Maintainer |
| Dependabot security updates | 启用 security updates 与 grouped version updates | 依赖安全修复自动化 | Maintainer |
| Content reporting | 将 `mss-boot`、`mss-boot-docs` 等仓库的 content reporting 打开 | GitHub Community Profile 复核显示这两个仓库 `content_reports_enabled=false`，会影响公开健康度 | Maintainer |

## P1：低成本社区运营设置

| 事项 | 建议配置 | 原因 | 执行人 |
| --- | --- | --- | --- |
| GitHub Discussions | 分类：Q&A、RFC、Roadmap、Help Wanted、中文交流 | 把答疑从 issue 中分流，减少维护负担 | Maintainer |
| Labels 同步 | 按组织 `.github/.github/labels.yml` 同步到四核心仓库 | 让 Issue Triage/Weekly Digest 输出可用 | Maintainer 或自动脚本 |
| Good first issue 批量创建 | 先从任务工厂挑 10 个，确认范围后创建 | 给外部贡献者 30 分钟可参与入口 | Maintainer |
| GitHub Projects | 建一个 lightweight board：Triage、Ready、In Progress、Review、Done | 个人维护者低成本看板 | Maintainer |
| 月报节奏 | 每月从 Weekly Digest 汇总成公开运营月报 | 用 AI 辅助生成，降低社区运营成本 | Doc/Leader |

## P2：赞助与外部分发

| 事项 | 建议配置 | 原因 | 执行人 |
| --- | --- | --- | --- |
| GitHub Sponsors / FUNDING | 由用户决定收款主体、平台和展示口径后再启用 | 涉及外部账号和财务主体 | Maintainer |
| 中文内容分发 | README/docs 为主，掘金/公众号/Bilibili 只同步高价值内容 | 少渠道、低维护、可复用 | Maintainer |
| 社群 webhook | 暂不接 Slack/飞书/Discord 自动推送，除非已有稳定社区渠道 | 避免维护额外机器人和泄漏 token | Maintainer |
| 跨仓组织周报 | 需要 org-level PAT 或 GitHub App | `GITHUB_TOKEN` 只能稳定操作当前仓库 | Maintainer |

## P3：后续供应链增强

| 事项 | 建议配置 | 原因 | 执行人 |
| --- | --- | --- | --- |
| SBOM | tag release 生成 SBOM artifact | 增强可审计发布 | Release/Security |
| Artifact attestation | 后端镜像与前端产物添加 provenance/attestation | 提升供应链可信度 | Release/Security |
| Docker image scanning | Trivy 或 GHCR scanning | 镜像依赖风险可见 | Security |
| CODEOWNERS | 确认真实 reviewer 后再落地 | 不能虚构不存在的 team/user | Maintainer |

## 暂不建议自动化的事项

- 自动 merge：风险高，个人维护阶段先不要开。
- 自动 close issue：容易误伤新贡献者，先只打 `needs-info` 和评论。
- LLM 自动改代码后直发：不符合开源维护责任边界。
- Cloudflare 前端高频预览发布：与当前发布策略冲突，会消耗次数并污染公开 beta。

## 验收方式

外部设置完成后，记录：

- 仓库名
- 设置项
- 执行时间
- 执行人
- 截图或 `gh api` 导出片段
- 对应 required checks
- 是否影响 alpha/beta/prod 发布策略

该记录应追加到本文件或后续治理记录文档中。

## 2026-06-05 已完成项

- 组织 `.github` 治理基线已合并到 `main`。
- 核心仓库治理 PR 已合并：`mss-boot`、`mss-boot-admin`、`mss-boot-admin-antd`、`mss-boot-docs`。
- Labels 已同步到 `.github`、`mss-boot`、`mss-boot-admin`、`mss-boot-admin-antd`、`mss-boot-docs`。
- 已创建 12 个低风险 `good first issue`，四个核心仓库各 3 个。
- 已对 `mss-boot-admin` 历史 open issue 做轻量标签分流。
- 已对现有 Dependabot PR 增加 triage 标签，不自动合并。
- 未手动发布 Cloudflare beta/prod；仅 `mss-boot-admin-antd` PR #74 触发并通过已有 Cloudflare alpha build 检查。
