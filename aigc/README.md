# mss-boot-io AI Memory

This directory stores durable organization memory for AI agents and maintainers.
It is not a scratchpad. Important cross-repository decisions should be recorded
here so they survive local multi-git workspace changes and context resets.

## Entry Points

- [Organization memory](./prompts/organization-memory.zh-CN.md)
- [Release environment strategy](./prompts/release-environment-strategy.zh-CN.md)
- [Multi-agent delivery workflow](./prompts/multi-agent-delivery-workflow.zh-CN.md)
- [Open source AI-era growth plan](./prompts/open-source-ai-era-growth-plan.zh-CN.md)
- [Good first issue factory](./prompts/open-source-good-first-issue-factory.zh-CN.md)
- [Operations backlog](./prompts/open-source-operations-backlog.zh-CN.md)
- [CI stability incident](./prompts/ci-stability-incident-2026-06-05.zh-CN.md)
- [Open source ecosystem playbook memory](./prompts/open-source-ecosystem-playbook-2026-06-05.zh-CN.md)

## Rules

- Record decisions that affect more than one repository.
- Keep environment and release policy changes in this docs repository first.
- Do not store secrets, tokens, private endpoints, or account credentials.
- Prefer concrete commands, file paths, validation evidence, and rollback notes.
- When a repository-level change is important, record a local `aigc` memory in
  that repository and summarize the organization-level decision here.

## Maintainer Workflow

1. Before a cross-repository change, read the relevant memory file.
2. During implementation, update repository-local `aigc` files when behavior or
   workflow changes.
3. After implementation, update this organization memory when the decision
   affects release flow, CI, governance, community operations, or AI workflows.
4. Link memory updates from PR bodies so PR Guard and Docs Drift can explain the
   change.
