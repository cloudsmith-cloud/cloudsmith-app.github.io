# Workstream: Operate

## Purpose

Execute Azure infrastructure changes for cloudsmith.app with mandatory human approval gate.

## Invoked by

`/operate <action>` — e.g., `/operate deploy to Azure Container Apps`, `/operate update DNS record`

## Pipeline

### Stage 1 — Security review (cloudsmith-reviewer)

Invoke `cloudsmith-reviewer` with `task`: "Security review for operate action: {{$ARGUMENTS}}"

If `verdict: block`: stop immediately. Surface issues to human. Do not proceed.

### Stage 2 — Plan and reconnoiter (cloudsmith-operator)

Invoke `cloudsmith-operator` with:
- `action`: "{{$ARGUMENTS}}"
- `security_review`: Stage 1 output

Operator runs **read-only** reconnaissance (az list, az show, gh api) and produces a precise change plan with the exact `az` commands to be run.

### Stage 3 — REQUIRED human approval (pause)

Present the full change plan to the human.

**Human must type APPROVE to continue.** Any other response cancels the operation.

### Stage 4 — Execute (cloudsmith-operator)

Pass `human_approved: true` to the operator. It executes the plan step by step and logs every operation to `.claude/logs/operate.jsonl`.

## Failure handling

- If the human does not type APPROVE: cancel and log the cancellation
- If any step fails during execution: stop, do not attempt recovery, surface the failure to human
- Never retry a failed infrastructure operation automatically
