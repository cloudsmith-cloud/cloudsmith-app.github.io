# /operate

Execute an Azure infrastructure change for cloudsmith.app. Requires your explicit APPROVE before anything runs.

## Usage

```
/operate <action>
```

## Examples

```
/operate deploy coming soon page to Azure Static Web Apps
/operate create Container Apps environment for cloudsmith.app
/operate update Cloudflare DNS for cloudsmith.app
/operate rotate the SWA deploy token
```

## What happens

Invokes `workstream-operate`: security review → operator reconnoiters (read-only) → presents change plan → **you type APPROVE** → operator executes + logs.

No Azure resource is created, modified, or deleted without your APPROVE.

$ARGUMENTS describe the infrastructure action.
