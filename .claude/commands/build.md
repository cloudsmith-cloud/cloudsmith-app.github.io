# /build

Build a new page, feature, or component for cloudsmith.app.

## Usage

```
/build <task>
```

## Examples

```
/build coming soon landing page
/build email interest capture form
/build countdown timer to launch
```

## What happens

Invokes `workstream-build`: planner (if needed) → coder → reviewer → your approval → commit. Azure deployment changes route to `/operate` instead.

$ARGUMENTS are passed as the task description.
