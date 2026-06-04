---
title: Open Source Governance
order: 10
nav:
  order: 5
  title: "devops"
description: mss-boot-io open source governance, quality gates, release policy, and AI memory rules
keywords: [open source, governance, quality gates, release, AI memory]
---

# Open Source Governance

`mss-boot-io` uses a governance-first open source workflow. The goal is to keep
the project readable by contributors, maintainers, and AI agents.

## Quality Gates

- Pull requests must include tests, documentation impact, security impact, and
  release impact.
- Go repositories run CI, CodeQL, govulncheck, Dependabot, and OpenSSF
  Scorecard.
- Frontend and docs repositories run CI, CodeQL, Dependabot, and OpenSSF
  Scorecard.
- API, config, deployment, or workflow changes should update documentation or AI
  memory.

## Release Policy

- Alpha/dev is for fast integration.
- Beta is public preview and must pass smoke testing before frontend Cloudflare
  deployment.
- Production uses tagged backend releases and verified frontend deployments.

## Security

Do not report vulnerabilities as public issues. Follow the security policy of
the affected repository and use private GitHub Security Advisories when enabled.

## AI Memory

Organization-level workflow, release policy, and cross-repository decisions must
be recorded in `mss-boot-docs/aigc/prompts/`.
