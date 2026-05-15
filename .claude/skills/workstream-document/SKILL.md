# Workstream: Document

## Purpose

Write or update content for cloudsmith.app — landing copy, in-app text, onboarding flows.

## Invoked by

`/document <task>` — e.g., `/document coming soon page`, `/document onboarding step 1 copy`

## Pipeline

### Stage 1 — Write (cloudsmith-web-content)

Invoke `cloudsmith-web-content` with `task`: "{{$ARGUMENTS}}"

### Stage 2 — Human review (pause)

Show content to human. Ask: "Ready to commit?"

On approval: `git add`, `git commit -m "docs(<scope>): <description>"`, `git push`.
