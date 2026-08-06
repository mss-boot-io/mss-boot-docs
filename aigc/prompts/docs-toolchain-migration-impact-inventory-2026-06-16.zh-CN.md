---
name: docs-toolchain-migration-impact-inventory-final
description: 文档站点工具链迁移影响面盘点（最终版本）：构建、路由、主题、侧栏/导航、部署相关的所有组件和影响文件
metadata:
  type: project
---

# Docs Toolchain Migration Impact Inventory (Final)

> Parent RFC: [#32](https://github.com/mss-boot-io/mss-boot-docs/issues/32)
> Related: [#33](https://github.com/mss-boot-io/mss-boot-docs/issues/33)
> Last updated: 2026-06-16

## 1. 工具链概览

| 组件 | 当前版本 | 来源 | 安全状态 |
|------|---------|------|---------|
| `dumi` | ^2.4.30 | 直接依赖 | 无直接告警 |
| `umi` (transitive) | 4.6.61 | dumi -> umi | 无直接告警 |
| `vite` | 6.4.3 (overrides) | dumi -> umi -> bundler-vite | 已修复 (CVE-2025-30208) |
| `@vitejs/plugin-react` | 4.7.0 (overrides) | vite | 已修复 |
| `react-router` | 6.30.4 (overrides) | umi router | 已修复 (CVE-2025-46711) |
| `react-router-dom` | 6.30.4 (overrides) | umi router | 已修复 (CVE-2025-46711) |
| `@babel/runtime` | 7.29.7 (overrides) | umi toolchain | 已修复 (CVE-2025-27789) |
| `esbuild` | 0.28.1 (overrides) | vite/esbuild | 已修复 (CVE-2025-27504) |
| `nth-check` | 2.1.1 (overrides) | css-select | 已修复 (CVE-2021-3803) |
| `path-to-regexp` | 1.9.0 (overrides) | umi router | 已修复 (CVE-2024-45296) |
| `send` | 0.19.2 (overrides) | express | 已修复 (CVE-2024-43799) |
| `uuid` | 11.1.1 (overrides) | umi deps | 已修复 |
| `elliptic` | 6.6.1 | webpack/node-libs-browser | 无上游修复 |

**依赖链分析**：
```
dumi@2.4.30
├── umi@4.6.61
│   ├── @umijs/bundler-vite -> vite@6.4.3 (overrides)
│   ├── @umijs/bundler-webpack -> webpack -> node-libs-browser -> elliptic@6.6.1
│   └── react-router@6.30.4 (overrides)
└── @umijs/max (可能)
```

**剩余安全告警**：
- `elliptic@6.6.1`：来自 webpack/node-libs-browser 兼容链路，advisory 标记 `first_patched_version=null`，无法通过 override 解决

## 2. 构建 (Build)

**构建入口**: `.dumirc.ts`

```ts
import { defineConfig } from 'dumi';

export default defineConfig({
  themeConfig: {
    name: 'mss-boot',
    rtl: true,
    logo: 'favicon.ico',
    footer: `Open-source MIT Licensed | Copyright © 2024-present
    <br />
    Powered by <a target="_blank" href="https://github.com/mss-boot-io">mss-boot-io</av>`,
    socialLinks: {
      github: 'https://github.com/mss-boot-io/mss-boot',
    },
  },
  sitemap: { hostname: 'https://docs.mss-boot-io.top' },
});
```

**构建命令**: `dumi build`

**产出目录**: `dist/` (静态 HTML/CSS/JS)

**构建流程**:
1. `pnpm install --frozen-lockfile` - 安装依赖
2. `dumi build` - 构建静态站点
3. 产出 `dist/` 目录

**受影响的关键文件**:
- `.dumirc.ts` — dumi 全局配置，包含主题配置和 sitemap
- `.dumi/favicon.ico` — 站点 favicon
- `package.json` — 构建脚本和 pnpm overrides
- `pnpm-lock.yaml` — 依赖锁定文件

## 3. 路由 (Routing)

dumi 基于文件系统路由，自动将 `docs/` 下的 Markdown 文件映射为页面路由。

**路由规则**:
- `docs/index.md` → `/`
- `docs/guide/index.md` → `/guide/`
- `docs/guide/config.md` → `/guide/config`
- `docs/admin/index.md` → `/admin/`
- `docs/coding/index.md` → `/coding/`
- `docs/devops/index.md` → `/devops/`
- `docs/aigc/index.md` → `/aigc/`
- `docs/vm/index.md` → `/vm/`

**路由生成机制**:
- dumi 自动扫描 `docs/` 目录下的 `.md` 文件
- 根据文件路径生成对应的路由
- 支持嵌套目录结构

**受影响的关键文件**:
- `docs/` 目录下所有 `*.md` 文件（54 个文件）
- 不修改 `@umijs/route` 或 `react-router` 路由定义，完全由文件系统决定

## 4. 主题 (Theme)

dumi 2.x 内置主题系统，当前使用默认主题。

**主题配置**: `.dumirc.ts` 中的 `themeConfig`
- `name`: 站点名称
- `rtl`: 启用 RTL 支持
- `logo`: favicon.ico
- `footer`: 自定义页脚
- `socialLinks`: GitHub 链接

**主题组件**:
- 使用 dumi 默认主题组件
- 无自定义 `.dumi/theme` 目录
- 无额外主题覆盖

**受影响的关键文件**:
- `.dumirc.ts` — 主题配置入口
- `.dumi/favicon.ico` — 站点图标
- 无自定义 `.dumi/theme` 覆盖目录

## 5. 侧栏/导航 (Sidebar/Navigation)

dumi 2.x 基于 `docs/` 目录结构自动生成侧边栏导航。

**导航生成规则**:
- 目录结构直接决定导航树
- `docs/admin/` → 侧边栏 "admin" 分组
- `docs/guide/` → 侧边栏 "guide" 分组
- 等等...

**受影响的关键文件**:
- `docs/` 目录的层级结构直接决定导航树
- 无额外 `.dumi/theme` 自定义组件
- 无自定义导航配置文件

## 6. 部署 (Deployment)

**部署平台**: Cloudflare Pages / Cloudflare Workers

**部署配置**: `wrangler.toml`
```toml
name = "mss-boot-docs"
account_id = "126f4a0c75180e9493cb3716f5178574"
workers_dev = true
compatibility_date = "2026-03-02"

[assets]
directory = "./dist"

[env.prod]
name = "mss-boot-docs"
routes = [
	{ pattern = "docs.mss-boot-io.top/*", zone_id = "83499b45e20e8e3c34b2c0b6507c2f66" }
]
```

**部署工作流**: `.github/workflows/docs.yml`
- 触发: `main` 分支 push 或 `workflow_dispatch`
- 构建 job: Node 20 构建 → 打包 `dist.tar.gz` → 上传 artifact
- 部署 job: Node 22 下载 artifact → Cloudflare Wrangler deploy
- 生产域名: `docs.mss-boot-io.top`

**部署流程**:
1. 推送到 `main` 分支
2. GitHub Actions 触发构建
3. 构建产出 `dist/` 目录
4. 打包为 `dist.tar.gz`
5. 上传到 GitHub Artifacts
6. 下载 artifact 并部署到 Cloudflare

**受影响的关键文件**:
- `wrangler.toml` — Cloudflare 部署配置
- `.github/workflows/docs.yml` — 生产部署流水线

## 7. CI/CD 流水线

### CI 构建验证
**文件**: `.github/workflows/ci.yml`
- 触发: PR to `main` 或 push to `main`/`codex/**`
- 内容: `pnpm install --frozen-lockfile` + `pnpm build`

### CodeQL 安全扫描
**文件**: `.github/workflows/codeql.yml`
- 触发: push/PR to `main`，每周日定时
- 内容: JavaScript/TypeScript + Actions 安全分析

### 文档漂移检测
**文件**: `.github/workflows/docs-drift.yml`
- 触发: PR to `main`
- 内容: 检测文档内容与代码的一致性

### OpenSSF Scorecard
**文件**: `.github/workflows/scorecard.yml`
- 触发: push to `main`，每周日定时
- 内容: 项目安全评分

### Dependabot 自动更新
**文件**: `.github/dependabot.yml`
- npm 依赖: 每周更新，分组 minor/patch
- GitHub Actions: 每周更新，分组

### 其他自动化工作流（非 docs 构建相关）
以下工作流不参与 docs 构建/部署链路，迁移时无需改动，仅作完整记录：
- `.github/workflows/pr-guard.yml` — PR 校验/拦截（合并前检查，与 CI 链路相关，迁移后需确认仍通过）
- `.github/workflows/copilot-setup-steps.yml` — Copilot 环境初始化步骤
- `.github/workflows/issue-triage.yml` — Issue 自动分流
- `.github/workflows/release-draft.yml` — Release 草稿生成
- `.github/workflows/weekly-digest.yml` — 每周摘要

## 8. 验证命令 (Verification Commands)

### 本地开发
```bash
pnpm install           # 安装依赖
pnpm dev               # 启动 dumi dev 本地预览 (localhost:8000)
pnpm start             # 等价于 pnpm dev
```

### 本地构建
```bash
pnpm build             # dumi build 生成 dist/
CI=true pnpm build     # CI 模式下构建
```

### 依赖审计
```bash
pnpm audit --audit-level=moderate   # 检查安全漏洞
pnpm why vite                       # 查看 vite 依赖链
pnpm why react-router               # 查看 react-router 依赖链
```

### CI 验证
```bash
# 等效于 CI 流程
CI=true pnpm install --frozen-lockfile
CI=true pnpm build
```

### 部署验证
```bash
# 等效于 docs.yml 部署流程
pnpm build
tar -czvf dist.tar.gz dist
# 需要 cf_api_token 才能执行:
# wrangler deploy --env prod
```

## 9. 迁移影响分析

### 低影响区域
| 区域 | 影响评估 | 原因 |
|------|---------|------|
| 文档内容 (docs/) | 无 | 纯 Markdown，不依赖构建工具链 |
| 侧边栏导航 | 无 | 由文件系统路由自动生成 |
| 主题配置 | 低 | 当前只用基础 themeConfig，无自定义主题组件 |
| 静态资源 (public/) | 低 | 纯静态文件，迁移通常不影响 |

### 中等影响区域
| 区域 | 影响评估 | 原因 |
|------|---------|------|
| 构建 (dumi build) | 中 | 需替换 `dumi dev` 和 `dumi build` |
| Sitemap 生成 | 低-中 | 如新工具链不支持自动 sitemap，需手动配置 |
| pnpm overrides | 中 | 需要根据新工具链调整版本覆盖 |

### 高影响区域
| 区域 | 影响评估 | 原因 |
|------|---------|------|
| CI/CD 流水线 | 高 | `pnpm build` 命令和依赖项变化需同步更新 docs.yml |
| Cloudflare 部署 | 低-中 | 只要产出 `dist/` 目录结构不变即可，需验证 |
| Dependabot 配置 | 中 | 新的直接依赖需要重新配置告警规则 |
| 依赖链 (pnpm-lock.yaml) | 高 | 依赖树变化会影响所有下游依赖 |

### 迁移注意项

1. **不改变后端/管理后台发布流程**: RFC #32 明确约束，docs 迁移不得影响 mss-boot 主项目的发布
2. **elliptic 无解**: 这是最底层的 crypto 依赖，来自 webpack 兼容链。即使迁移到纯 Vite 也可能存在，除非项目完全不使用 browserify/webpack node polyfills
3. **部署安全**: Cloudflare 部署使用 artifact 传递，构建和部署使用不同 Node 版本 (20 vs 22)，迁移后需验证一致性
4. **回滚策略**: 由于部署是静态文件上传，任何回滚只需恢复 `dist/` 内容为上一版本即可，无需特殊工具
5. **CodeQL**: 迁移后需确认 CodeQL 仍能在 JS/TS 代码上正常运行，特别是如果路由方式改变
6. **pnpm overrides**: 当前通过 overrides 修复了多个安全告警，迁移后需要重新评估哪些 override 仍然需要

## 10. 当前配置文件清单

```
.dumirc.ts              # dumi 主配置
.dumi/favicon.ico       # 站点 favicon（dumi 主题用）
public/favicon.ico      # public 目录 favicon（构建直接拷贝到 dist 根）
package.json            # 脚本 + pnpm overrides
pnpm-lock.yaml          # 依赖锁定 (lockfileVersion 9.0)
wrangler.toml           # Cloudflare 部署配置
tsconfig.json           # TypeScript 配置
.editorconfig           # 编辑器配置
.prettierrc.js          # Prettier 配置
.prettierignore         # Prettier 忽略文件
.gitignore              # Git 忽略文件
.husky/commit-msg       # Git commit hook (commitlint)
.husky/pre-commit       # Git pre-commit hook (lint-staged)
.github/workflows/docs.yml       # 生产部署
.github/workflows/ci.yml         # CI 构建验证
.github/workflows/codeql.yml     # CodeQL 安全扫描
.github/workflows/docs-drift.yml # 文档漂移检测
.github/workflows/scorecard.yml  # OpenSSF Scorecard
.github/workflows/pr-guard.yml   # PR 校验/拦截
.github/workflows/copilot-setup-steps.yml # Copilot 环境初始化
.github/workflows/issue-triage.yml # Issue 自动分流
.github/workflows/release-draft.yml # Release 草稿生成
.github/workflows/weekly-digest.yml # 每周摘要
.github/dependabot.yml           # 依赖更新自动化
docs/                   # 54 个 Markdown 文档文件
public/                 # 静态资源 (favicon.ico, images/*.jpg 等)
aigc/                   # AI 生成文档 (非构建产物)
```

## 11. 依赖覆盖详情

当前 `pnpm.overrides` 配置：

```json
{
  "pnpm": {
    "overrides": {
      "@babel/runtime": "7.29.7",
      "@vitejs/plugin-react@4.0.0": "4.7.0",
      "esbuild": "0.28.1",
      "nth-check": "2.1.1",
      "path-to-regexp": "1.9.0",
      "react-router@6.3.0": "6.30.4",
      "react-router-dom@6.3.0": "6.30.4",
      "send": "0.19.2",
      "uuid": "11.1.1",
      "vite@4.5.2": "6.4.3"
    }
  }
}
```

**覆盖说明**：
- `@babel/runtime`: 修复 CVE-2025-27789
- `@vitejs/plugin-react@4.0.0`: 升级到 4.7.0 修复安全问题
- `esbuild`: 修复 CVE-2025-27504
- `nth-check`: 修复 CVE-2021-3803
- `path-to-regexp`: 修复 CVE-2024-45296
- `react-router@6.3.0`: 升级到 6.30.4 修复 CVE-2025-46711
- `react-router-dom@6.3.0`: 升级到 6.30.4 修复 CVE-2025-46711
- `send`: 修复 CVE-2024-43799
- `uuid`: 升级到 11.1.1 修复安全问题
- `vite@4.5.2`: 升级到 6.4.3 修复 CVE-2025-30208

## 12. 文档内容结构

```
docs/
├── admin/          # 36 个文件 - 管理后台相关文档
├── aigc/           # 1 个文件 - AIGC 相关文档
├── coding/         # 4 个文件 - 编码相关文档
├── devops/         # 5 个文件 - DevOps 相关文档
├── guide/          # 6 个文件 - 指南相关文档
├── index.md        # 1 个文件 - 首页
└── vm/             # 1 个文件 - 虚拟机相关文档
```

**总计**: 54 个 Markdown 文件

## 13. 代码质量工具

### Prettier
- 配置: `.prettierrc.js`
- 规则: 80 字符宽度，单引号，尾随逗号
- 忽略: `.prettierignore`

### EditorConfig
- 配置: `.editorconfig`
- 规则: 2 空格缩进，LF 换行，UTF-8 编码

### Husky Git Hooks
- `commit-msg`: 使用 commitlint 验证提交信息
- `pre-commit`: 使用 lint-staged 运行 Prettier

### lint-staged
- 配置: `package.json` 中
- 规则: 对 `*.md` 和 `*.json` 文件运行 Prettier