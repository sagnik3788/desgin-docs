# OpenTofu Plugin

## 📄 Metadata

| Field           | Value                                                                                                                                                                                                                                                                               |
| --------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Stability**   | In Development                                                                                                                                                                                                                                                                      |
| **Issues**      | [![Open issues](https://img.shields.io/github/issues-search/pipe-cd/community-plugins?query=is%3Aissue%20is%3Aopen%20label%3Aplugin%2Fopentofu%20\&label=open\&color=orange)](https://github.com/pipe-cd/community-plugins/issues?q=is%3Aopen+is%3Aissue+label%3Aplugin%2Fopentofu) |
| **Code Owners** |                                                                                                                                                                                                                                                                                |

---

## Supported Features

* `PipelineSync`
* `QuickSync`
* `PlanPreview`
* `DriftDetection`

---

## Overview

The OpenTofu plugin enables **PipeCD** to manage infrastructure deployments using [OpenTofu](https://opentofu.org), a community-driven Terraform alternative.
It supports standard OpenTofu workflows like planning, applying, rollback, and syncing infrastructure state — tightly integrated with PipeCD pipelines.

---

##  Stages

### `OPEN_TOFU_SYNC`

Applies changes directly using `opentofu apply`.

```
Initializing OpenTofu...
Applying changes...
Successfully applied changes!
```

---

### `OPEN_TOFU_PLAN`

Runs `opentofu plan` and previews what changes would be made.

```
Initializing OpenTofu...
Planning changes...
Detected X imports, Y adds, Z changes, W destroys.
```

---

### `OPEN_TOFU_APPLY`

Applies a previously saved plan file using `opentofu apply`.

```
Initializing OpenTofu...
Applying changes...
Successfully applied changes!
```

---

### 🔹 `OPEN_TOFU_ROLLBACK`

Rolls back to the previous state using a saved plan or state file.

```
Initializing OpenTofu...
Rolling back to previous state...
Successfully rolled back changes!
```

---

## Plugin Scope Configuration (`piped.yaml`)

Configure defaults for **all OpenTofu applications** under `plugins.opentofu`.

| Field                | Type   | Description                              | Required | Default     |
| -------------------- | ------ | ---------------------------------------- | -------- | ----------- |
| `version`            | string | OpenTofu version to use                  | ✅ Yes    | `"1.6.0"`   |
| `defaultConfig`      | string | Default config file name (e.g., main.tf) | ❌ No     | `"main.tf"` |
| `defaultWorkingDir`  | string | Default working directory                | ❌ No     | `"."`       |
| `defaultEnv`         | array  | Default environment variables            | ❌ No     | `[]`        |
| `defaultInit`        | bool   | Whether to run `init` automatically      | ❌ No     | `true`      |
| `defaultPlanStorage` | object | Backend config for storing plan files    | ❌ No     | -           |

---

## Deploy Target Config (`app.pipecd.yaml → deployTargets`)

Overrides specific settings **per environment**.

| Field    | Type   | Description                            | Required | Default |
| -------- | ------ | -------------------------------------- | -------- | ------- |
| `name`   | string | Environment name (e.g., `dev`, `prod`) | ✅ Yes    | -       |
| `config` | object | Config overrides for this environment  | ✅ Yes    | -       |

Inside `config`, the following fields are supported:

| Field        | Type   | Description                           |
| ------------ | ------ | ------------------------------------- |
| `version`    | string | OpenTofu version for this environment |
| `workingDir` | string | Working directory to run OpenTofu     |
| `env`        | array  | Environment-specific variables        |
| `init`       | bool   | Whether to run `opentofu init`        |

---

##  Application Scope Config (`app.pipecd.yaml → plugins.opentofu.input`)

Used for per-application configuration.

| Field        | Type   | Description                        | Required | Default     |
| ------------ | ------ | ---------------------------------- | -------- | ----------- |
| `version`    | string | OpenTofu version                   | ❌ No     | From plugin |
| `config`     | string | Main config file (e.g., `main.tf`) | ❌ No     | From plugin |
| `workingDir` | string | Working directory                  | ❌ No     | `"."`       |
| `env`        | array  | List of environment variables      | ❌ No     | `[]`        |
| `init`       | bool   | Whether to run `opentofu init`     | ❌ No     | `true`      |

---

##  Example: `app.pipecd.yaml`

```yaml
apiVersion: pipecd.dev/v1beta1
kind: Application
spec:
  name: aws-opentofu-app
  git:
    repoID: tofu-example
    path: terraform-dev
    branch: main
  kind: PLUGIN
  plugins:
    opentofu:
      input:
        version: "1.9.1"
        config: "main.tf"
        workingDir: "."  
        env:
          - TF_VAR_aws_region=us-east-1
          - TF_VAR_bucket_name=tofu-example-bucket-123
          - TF_VAR_environment=dev
        init: true
      quickSync:
        autoApprove: true
```

---


