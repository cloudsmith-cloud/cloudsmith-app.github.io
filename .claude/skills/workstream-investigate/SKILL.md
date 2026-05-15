# Workstream: Investigate

## Purpose

Root cause analysis for issues in cloudsmith.app — build failures, Azure deployment errors, rendering bugs.

## Invoked by

`/investigate <issue>`

## Pipeline

### Stage 1 — Investigate (read-only)

Read relevant files, run read-only diagnostics. Produce findings:
```yaml
issue: <one sentence>
root_cause: <identified or suspected>
evidence: [<file:line or output excerpt>]
affected_files: [...]
recommended_fix: <what to do>
azure_involved: <true|false>
confidence: <high|medium|low>
```

### Stage 2 — Human review (pause)

Present findings. If fix involves Azure changes: recommend `/operate` workstream.

If code fix only: route to `/build`.
