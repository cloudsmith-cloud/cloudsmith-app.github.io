# CLAUDE.md — cloudsmith-app.github.io

Claude Code context for `cloudsmith-app.github.io` — the hosted application site for CloudSmith at cloudsmith.app.

## What cloudsmith-app.github.io is

This is the source for `cloudsmith.app` — the live PaaS-hosted instance of CloudSmith running in Azure.

| Phase | Purpose |
|---|---|
| Phase 0–1 (now) | Coming soon landing page — stake the claim, collect early interest |
| Phase 2+ | Live demo environment with sandboxed sample data — try CloudSmith without self-hosting |
| Phase 8+ | Potential full managed SaaS for MSPs who prefer not to self-host |

**Hosting:** Azure (Container Apps or App Service — see ADR-006). **DNS:** Cloudflare. Note: CloudSmith runs on CloudSmith — `cloudsmith.app` will be the first real production deployment of the platform itself.

## Repo structure

Structure depends on ADR-002 (web framework) and ADR-006 (hosting model). Currently a placeholder.

```
cloudsmith-app.github.io/
├── README.md        — repo purpose
└── .claude/         — agents, skills, commands, hooks, logs
```

## Stack / conventions

- **Language / runtime:** TBD — pending ADR-002 (React/Blazor/Vue) and ADR-006 (hosting)
- **Commit format:** `type(scope): short description` (conventional commits)
- **ADO reference:** `AB#<id>` in commit messages when applicable
- **Never:** commit secrets, tokens, connection strings, or API keys — this will be a public repo

## Subagents

| Agent | Model | Purpose |
|---|---|---|
| cloudsmith-web-content | sonnet | Writes app UI content, landing copy, and user-facing text |
| cloudsmith-coder | opus | Implements app features and components once framework is chosen |
| cloudsmith-planner | sonnet | Plans features — read-only, no code changes |
| cloudsmith-reviewer | sonnet | Reviews correctness, security, and platform standards |
| cloudsmith-operator | opus | Azure infra changes — REQUIRES human APPROVE before executing any write |

## Slash commands

| Command | Skill | Purpose |
|---|---|---|
| `/build <task>` | workstream-build | New features: planner → coder → reviewer → human approve → commit |
| `/document <task>` | workstream-document | Content writing → human approve → commit |
| `/investigate <issue>` | workstream-investigate | Debug: investigator → findings → human decides |
| `/operate <action>` | workstream-operate | Azure infra changes — operator → security-reviewer → REQUIRED human APPROVE |

## Hard rules

- No secrets, API keys, or credentials committed — this will be a public repo
- Touching auth, secrets, or Azure permissions auto-routes to `operate` workstream
- Reviewer model family ≠ coder model family
- Max 2 revision rounds before human escalation
- `/operate` workstream: human must type **APPROVE** before any Azure write executes

## What Claude may do autonomously

- Read, search, and grep any file in this repo
- Write and edit files within this repo
- `git add`, `git commit`, `git push`
- `gh issue` and `gh pr` commands

## Always confirm before

- Deploying to Azure (any environment)
- Creating or deleting Azure resources
- Making this repo public
- Modifying auth, secrets, or permissions

## Runtime structure

```
.claude/
├── agents/     — cloudsmith-web-content, cloudsmith-coder, cloudsmith-planner, cloudsmith-reviewer, cloudsmith-operator
├── commands/   — build, document, investigate, operate
├── skills/     — workstream-build, workstream-document, workstream-investigate, workstream-operate
├── hooks/      — block-secrets, validate-path, log-tokens, summarize-session
├── logs/       — tokens.jsonl, sessions.jsonl, operate.jsonl (gitignored)
└── settings.json
```
