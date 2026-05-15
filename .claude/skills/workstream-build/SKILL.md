# Workstream: Build

## Purpose

Implement new pages, features, or components for cloudsmith.app.

## Invoked by

`/build <task>` — e.g., `/build coming soon landing page`, `/build email signup form`

## Pipeline

### Stage 1 — Plan (cloudsmith-planner, if task > 3 files or > 1 hour)

Invoke `cloudsmith-planner` with `task`: "{{$ARGUMENTS}}"

Show the plan to human. Ask: "Does this plan look right?" Incorporate feedback.

Skip for trivial tasks.

### Stage 2 — Implement (cloudsmith-coder)

Invoke `cloudsmith-coder` with `task`: "{{$ARGUMENTS}}" and `plan`: Stage 1 output (if present).

### Stage 3 — Review (cloudsmith-reviewer)

Invoke `cloudsmith-reviewer` with `task`: "{{$ARGUMENTS}}" and `coder_output`: Stage 2 output.

If `verdict: request_changes`: invoke `cloudsmith-coder` again (max 2 cycles).
If `verdict: block`: stop, present to human. Do not commit.

### Stage 4 — Human approval (pause)

On approval:
```
git add <changed files>
git commit -m "feat(<scope>): <description>"
git push
```

## Failure handling

- Azure deployment changes must route through `/operate` — coder should not touch deployment config; reviewer will block if it does
