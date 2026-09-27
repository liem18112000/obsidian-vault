---
title: "Luz Kubernetes Terraform"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/49366466601/Luz+Kubernetes+Terraform
space: "LUZ"
topic: infra
relevance: 0.844
depth: 3
updated: 2026-05-07
attachments: 0
tags:
  - confluence
  - infra
  - space/luz
---

# Luz Kubernetes Terraform

> [!info] Imported from Confluence
> Space **LUZ** · updated 2026-05-07 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/49366466601/Luz+Kubernetes+Terraform)
> Relevance 0.844 · topic `infra`

This directory provisions **Google Cloud Run**, **Pub/Sub**, and related **IAM** for ePost / EPC workloads (microservices, brokers, thumbnail, antivirus, websocket, etc.).

It is separate from `kubernetes/` and `kubernetes-overlays/`, which deploy apps to **GKE** with customisations. Coordinate naming (URLs, gateways, regions) when changing either side.

## State Backend

Remote state lives in GCS:

<div>

|              |                 |
|--------------|-----------------|
| **Property** | **Value**       |
| Bucket       | `luz-terraform` |
| Prefix       | `state`         |

</div>

### Initialize

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="cbcad524-3786-4344-b4ce-351cb7235d2e" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
cd terraform
terraform init
```

</div>

</div>

## Environments: Workspaces

Environments are modeled as **Terraform workspaces**, not separate root modules. The active workspace name (`dev`, `prod`, …) drives resource naming via `terraform.workspace` and selects GCP project and region from root `locals` in `main.tf`.

### Workspace → GCP Project and Region

Root `locals` map each workspace to:

- `project_id` — e.g. `klara-nonprod`, `klara-prod`, `klara-performance`

- `location` — GCP region (e.g. `europe-west6`; `dev-vn` uses `asia-southeast1`)

<div hasbody="true" macro-id="b47a2e42-07e3-435e-a7a9-83785f85566a" macro-name="warning">

<span class="aui-icon aui-icon-small aui-iconfont-error confluence-information-macro-icon"> </span>

<div>

**Important:** If you add a **new workspace**, extend `locals.project_ids` and `locals.locations` in `main.tf`. Otherwise `lookup(..., null)` can break plans.

</div>

</div>

## Root Modules

<div>

|  |  |  |
|----|----|----|
| **Module** | **Path** | **Role** |
| `luz-epc-services` | `./luz-epc-services` | Shared Pub/Sub topics, dead-letter Cloud Run, many `./services` child modules (one Cloud Run + Pub/Sub pattern per microservice) |
| `luz-epc-websocket-service` | `./luz-epc-websocket-service` | Websocket notifier Cloud Run + Pub/Sub |
| `luz-antivirus` | `./luz-antivirus` | Cloud Run (app + ClamAV sidecar) |
| `luz-message-broker` | `./luz-message-broker` | Cloud Run; deployed only for selected workspaces (`count`) |
| `luz-thumbnail` | `./luz-thumbnail` | Thumbnail Cloud Run |

</div>

<div hasbody="true" macro-id="945f7630-5784-4aa9-8fc7-a59a0fc14d30" macro-name="info">

<span class="aui-icon aui-icon-small aui-iconfont-info confluence-information-macro-icon"> </span>

<div>

Commented modules (`common-cloudrun-resources`, `epost-workflow`) are reserved for future use.

</div>

</div>

### Conditional Deployment Example

`luz-message-broker` uses `count` so it only applies in some workspaces (e.g. `dev`, `dev-vn`, `performance`, `dev-staging`). Use the same pattern when a stack should not exist in `test` / `prod`.

## luz-epc-services (Largest Stack)

### Structure

- `main.tf` — Defines shared locals (API URLs, Keycloak, image digests per env in `service_versions_per_env`, project numbers, tenant IDs), enables `run.googleapis.com`, creates shared Pub/Sub topics and dead-letter plumbing, then instantiates many `module` blocks with `source = "./services"`.

- `services/` — Template module: Cloud Run service, optional Pub/Sub topic and push subscription, parameterized via `service_name`, `image_version`, `batch_processing`, `high_profile`, `overrides`, `dead-letter-queue`, etc.

### Adding a New EPC Microservice

1.  Add image tag(s) under `service_versions_per_env` for each workspace you support.

2.  Add a new `module "..." { source = "./services" ... }` block, copying a similar service.

<div hasbody="true" macro-id="7619a3a2-695e-46af-a08c-e4774257841e" macro-name="note">

<span class="aui-icon aui-icon-small aui-iconfont-warning confluence-information-macro-icon"> </span>

<div>

This is distinct from adding a **new top-level folder** under `terraform/` (see section below).

</div>

</div>

## Patterns in Standalone Modules

Examples: `luz-thumbnail`, `luz-antivirus`, etc.

1.  **Inputs:** `region`, `location`, `project_id` (passed from root).

2.  **Per-environment config:** Variable `environment_overlays` — a map keyed by workspace name (`dev`, `prod`, …). Inside the module:

    <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="3f496d37-f180-47ae-b067-a9222a7381bd" macro-name="code" style="border-width: 1px;">

    <div class="codeContent panelContent pdl">

    ``` syntaxhighlighter-pre
    local.environment = lookup(var.environment_overlays, terraform.workspace, null)
    ```

    </div>

    </div>

    then `lookup(local.environment, "vpc_connector", null)` for <span class="inline-comment-marker" ref="bc06c0d6-1ba9-4ee1-8bf1-7dbaf2fbd2fd">VPC connector</span>, gateways, scaling, secrets, etc.

3.  **Naming:** Resources often use `"${terraform.workspace}-<service-name>"` so each workspace gets isolated Cloud Run services.

4.  `dev-vn`**:** Several stacks use `asia-southeast1` and a **VN-specific VPC connector**; mirror existing `terraform.workspace == "dev-vn" ? ... : ...` logic when your service must match EPC networking.

## How to Add a New Terraform Module

### Step 1: Create a Folder

Example: `terraform/luz-my-service/`

### Step 2: Add variables.tf

**Minimum alignment with root:**

- `region`

- `location`

- `project_id`

**Recommended for consistency:**

- `environment_overlays` — map workspace → `{ vpc_connector, internal_api_gateway_base_uri, ... }` instead of hardcoding env differences in resources.

### Step 3: Add main.tf

- Use `terraform.workspace` in names and env branching.

- Prefer `lookup(var.environment_overlays, terraform.workspace, null)` for VPC, URLs, secrets, scaling.

- For parity with EPC networking, reuse `dev-vn` connector / region patterns from existing modules (`luz-epc-services/services`, `luz-epc-websocket-service`).

### Step 4: Wire the Module in Root main.tf

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="992cf265-fef8-4708-b371-ce64cc9628a4" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
module "luz-my-service" {
  source     = "./luz-my-service"
  region     = var.region
  location   = local.location   # match siblings that share the same region rules
  project_id = local.project_id
}
```

</div>

</div>

Use `count` or `for_each` if the module should only run in specific workspaces.

### Step 5: Optional Variables Per Environment

Root `variables.tf` defines values such as `cloudrun_subnet_ip_range`. Per-environment files live under `overlays/<env>/env.tfvars`.

## Typical Commands

### Select or Create Workspace

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="36a8e581-de14-4bac-b96d-da33a2e725f6" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
cd terraform
terraform workspace select dev    # or: terraform workspace new dev
```

</div>

</div>

### Plan & Apply

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="372e9459-9ed4-4480-a28a-9a54398f4172" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
terraform plan -var-file=overlays/env-dev/env.tfvars
terraform apply -var-file=overlays/env-dev/env.tfvars
```

</div>

</div>

### Credentials

Ensure GCP credentials are configured the way your team expects (for example application default credentials or `GOOGLE_APPLICATION_CREDENTIALS`). Provider defaults may come from environment or CI; confirm with platform owners.

## Checklist for a New Module

- <span class="placeholder-inline-tasks">New directory under `terraform/<name>/` with `variables.tf` and `main.tf` (plus `outputs.tf` if other modules consume outputs)</span>
- <span class="placeholder-inline-tasks">New `module` block in root `terraform/main.tf`</span>
- <span class="placeholder-inline-tasks">Extended root `locals` if adding a workspace or new project/region mapping</span>
- <span class="placeholder-inline-tasks">`environment_overlays` entries for every workspace you deploy to (if using that pattern)</span>
- <span class="placeholder-inline-tasks">Optional `count` on the module if deployment is env-specific</span>
- <span class="placeholder-inline-tasks">Run `terraform fmt` and `terraform validate`; plan in a non-prod workspace before prod</span>

## Deploy the services on GCP Cloud-Run:

1.  *Script name*: `deploy_terraform.sh`

2.  *This script should automatically create the state file for that particular environment (different state file for different environment) on the GCS bucket and deploy our services on Cloud-Run.*

3.  *Manual Script run command*:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="605c0134-c60b-4433-bcc4-e63d4928a25b" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
luz_kubernetes\deploy_terraform.sh <Environment Name> --target=module.luz-epc-services
#e.g for test
luz_kubernetes\deploy_terraform.sh test --target=module.luz-epc-services
```

</div>

</div>

4.  After running the script, if you see the message below, please enter "yes" to approve and apply the changes if they match the expected modifications:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="66cdd5cd-909f-4d1e-9da4-04559dd36cd6" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
make Plan: 0 to add, 28 to change, 0 to destroy.
Do you want to perform these actions?
Terraform will perform the actions described above.
Only 'yes' will be accepted to approve.
```

</div>

</div>
