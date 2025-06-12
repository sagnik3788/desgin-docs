
# OpenTofu Plugin

## Metadata

| Field         | Value |
|---------------|-------|
| **Stability** | In Development |
| **Issues**    | [![Open issues](https://img.shields.io/github/issues-search/pipe-cd/community-plugins?query=is%3Aissue%20is%3Aopen%20label%3Aplugin%2Fopentofu%20&label=open&color=orange)](https://github.com/pipe-cd/community-plugins/issues?q=is%3Aopen+is%3Aissue+label%3Aplugin%2Fopentofu) |
| **Code Owners** |  |

---

## Supported Features

- PipelineSync
- QuickSync
- PlanPreview
- LiveState View
- DriftDetection

---

## Overview

This plugin enables PipeCD to manage OpenTofu deployments, providing infrastructure-as-code capabilities with support for planning, applying, and managing OpenTofu configurations.

---

## Stages

### `OPEN_TOFU_SYNC`

Combines plan and apply in one step for quick deployments.

```
Initializing OpenTofu...
Planning changes...
Applying changes...
Successfully applied changes!
```

### `OPEN_TOFU_PLAN`

Shows planned changes before applying.

```
Initializing OpenTofu...
Planning changes...
Detected X imports, Y adds, Z changes, W destroys.
```

### `OPEN_TOFU_APPLY`

Applies previously planned changes.

```
Initializing OpenTofu...
Applying changes...
Successfully applied changes!
```

### `OPEN_TOFU_ROLLBACK`

Reverts to the previous state.

```
Initializing OpenTofu...
Rolling back to previous state...
Successfully rolled back changes!
```

---

## Plugin Configuration

### Plugin Scope Config

| Field     | Type   | Description                          | Required | Default    |
|-----------|--------|--------------------------------------|----------|------------|
| `version` | string | OpenTofu version to use              | Yes      | `"1.6.0"`  |
| `workspace` | string | Workspace to use                    | No       | `"default"`|
| `backend` | object | Backend configuration                | No       | `{}`       |
| `providers` | array | Provider configurations             | No       | `[]`       |
| `vars`    | array  | Variables to pass to OpenTofu        | No       | `[]`       |

---
### Deploy Target Config

| Field | Type | Description | Required | Default |
|-|-|-|-|-|
| `version` | string | OpenTofu version to use | Yes | "1.6.0" |
| `workspace` | string | Workspace to use | No | "default" |
| `backend` | object | Backend configuration | No | {} |
| `providers` | array | Provider configurations | No | [] |
| `vars` | array | Variables to pass to OpenTofu | No | [] |

### Planner Config

| Field | Type | Description | Required | Default |
|-|-|-|-|-|
| `autoRollback` | boolean | Enable automatic rollback on failure | No | `false` |
| `alwaysUsePipeline` | boolean | Force using pipeline sync | No | `false` |

## Application Configuration

### Stage Options

#### `OPEN_TOFU_PLAN` Stage

| Field | Type | Description | Required | Default |
|-|-|-|-|-|
| `exitOnNoChanges` | boolean | Exit stage if no changes detected | No | `false` |
| `timeout` | string | Stage timeout | No | `"10m"` |

#### `OPEN_TOFU_APPLY` Stage

| Field | Type | Description | Required | Default |
|-|-|-|-|-|
| `requiresApproval` | boolean | Require manual approval | No | `true` |
| `timeout` | string | Stage timeout | No | `"15m"` |

#### `OPEN_TOFU_ROLLBACK` Stage

| Field | Type | Description | Required | Default |
|-|-|-|-|-|
| `timeout` | string | Stage timeout | No | `"10m"` |
