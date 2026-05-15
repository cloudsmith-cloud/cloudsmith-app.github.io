# /investigate

Debug an issue with cloudsmith.app.

## Usage

```
/investigate <issue>
```

## Examples

```
/investigate build failing on Azure deploy
/investigate page not rendering in Container Apps
```

## What happens

Invokes `workstream-investigate`: read-only diagnosis → findings → your decision. Azure failures route to `/operate`.

$ARGUMENTS describe the issue.
