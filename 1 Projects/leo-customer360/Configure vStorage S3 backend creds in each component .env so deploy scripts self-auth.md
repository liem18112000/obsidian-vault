---
ai_hash: a0f3b6f15f603643
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-22
entities:
- vStorage S3 backend
- .env
- deploy scripts
- AWS_ACCESS_KEY_ID
- AWS_SECRET_ACCESS_KEY
- terraform
- Terraform S3 backend
- terraform.tfvars
- Terraform backend block
- PROVIDER creds
- vngcloud
- client_id
- client_secret
- db_password
- modules
- deploy-api.sh
- server/.env
- postgres module
- TF_VAR_access_key
- TF_VAR_secret_key
- aws_s3 PROVIDER inputs
- git
- ~/.aws/credentials
- '[default] profile'
- AWS SDK
- orchestrator
- deploy-all.sh
- deployments/lib/tfstate.sh
- ensure_vstorage_creds
- access_key
- secret_key
- storage/terraform.tfvars
- deployments/storage/.env
- ensure_remote_init
- terraform init
- remote-backend modules
- full-stack deploys
- individual module script
- leo-customer360
- Running leo-customer360 deploys locally needs vStorage backend creds; CI can't do
  monitoring/LB
source: session 2026-08-22
status: seedling
tags:
- leo-customer360
- terraform
- vstorage
- s3-backend
- deployment
- env
title: Configure vStorage S3 backend creds in each component .env so deploy scripts
  self-auth
type: howto
---

# Configure vStorage S3 backend creds in each component .env so deploy scripts self-auth

To make leo-customer360 `deploy-*.sh` run without a manual `export AWS_ACCESS_KEY_ID/SECRET`, put the vStorage S3 backend creds in each component's `.env`.

## Why it works
Every deploy script (server, ads-server, frontend, monitoring, proxy, sso, storage, cache, postgres, load_balancer) runs `set -a; source ./.env; set +a` — `set -a` **exports** the sourced vars to the terraform child process. So two lines in each `.env`:
```
AWS_ACCESS_KEY_ID=<vStorage key>
AWS_SECRET_ACCESS_KEY=<vStorage secret>
```
let terraform's S3 backend authenticate.

## Key points
- **Must be `.env`, NOT `terraform.tfvars`** — a Terraform *backend* block cannot read tfvars/input variables; it only reads env vars + literal backend config. tfvars still supply the PROVIDER creds (vngcloud `client_id`/`client_secret`, `db_password`), so a **minimal `.env` with only the two AWS_* lines is sufficient** for modules whose other secrets live in tfvars.
- The export **propagates to sibling terraform reads** — e.g. `deploy-api.sh` sources `server/.env` then does `cd ../postgres && terraform output`; the child inherits the exported AWS_*.
- The vStorage key/secret already live in `deployments/storage/.env` as `TF_VAR_access_key`/`TF_VAR_secret_key` (the storage module's aws_s3 PROVIDER inputs) — reuse those exact values.
- `.env` is git-ignored in every module dir, so the secret stays out of git.
- **Universal single-file alternative:** `~/.aws/credentials` with a `[default]` profile (aws_access_key_id / aws_secret_access_key) — the S3 backend's AWS SDK reads it for ALL modules with zero `.env` edits (backend has `skip_credentials_validation=true`, so no STS).

## The orchestrator has its OWN creds path (don't confuse the two)
`deploy-all.sh` sources `deployments/lib/tfstate.sh` and calls `ensure_vstorage_creds` — which, if `AWS_*` is unset, exports them by parsing `access_key`/`secret_key` from `storage/terraform.tfvars` (or `TF_VAR_access_key`/`TF_VAR_secret_key` from `storage/.env`), then `ensure_remote_init` `terraform init`s the remote-backend modules. So **`deploy-all.sh` was ALWAYS creds-self-sufficient** (that's why full-stack deploys worked). The per-component `.env` AWS_* only matters when you run an INDIVIDUAL module script directly (e.g. `server/deploy-api.sh uat`), which does NOT source tfstate.sh — it only sources its own `.env`. Verified via `./deploy-all.sh uat --dry-run` (EXIT=0, 14/14 steps printed, nothing executed).

## Related
[[Running leo-customer360 deploys locally needs vStorage backend creds; CI can't do monitoringLB|Running leo-customer360 deploys locally needs vStorage backend creds; CI can't do monitoring/LB]]

## The orchestrator has its OWN creds path (don't confuse the two)
`deploy-all.sh` sources `deployments/lib/tfstate.sh` and calls `ensure_vstorage_creds` — which, if `AWS_*` is unset, exports them by parsing `access_key`/`secret_key` from `storage/terraform.tfvars` (or `TF_VAR_access_key`/`TF_VAR_secret_key` from `storage/.env`), then `ensure_remote_init` `terraform init`s the remote-backend modules. So **`deploy-all.sh` was ALWAYS creds-self-sufficient** (that's why full-stack deploys worked). The per-component `.env` AWS_* only matters when you run an INDIVIDUAL module script directly (e.g. `server/deploy-api.sh uat`), which does NOT source tfstate.sh — it only sources its own `.env`. Verified via `./deploy-all.sh uat --dry-run` (EXIT=0, 14/14 steps printed, nothing executed).

## Related

- [[Running leo-customer360 deploys locally needs vStorage backend creds; CI can't do monitoringLB|Running leo-customer360 deploys locally needs vStorage backend creds; CI can't do monitoring/LB]]

%% ai-graph-start %%

**Related notes:**
- [[Running leo-customer360 deploys locally needs vStorage backend creds; CI can't do monitoringLB]]
- [[Remote Terraform state needs no manual sync — bake creds + init into the deploy orchestrator to guarantee alignment]]
- [[customer360 secretconfig flow on VNG GitHub Actions + .env + tfstate-on-vStorage]]
- [[Terraform S3 remote backend for VNG vStorage (S3-compatible) config recipe]]
- [[CI-driven CD cannot resolve local gitignored Terraform state — needs remote backend or IPs via secrets]]

**Relations:**
- deploy scripts — *configure* — vStorage S3 backend
- vStorage S3 backend — *requires* — AWS_ACCESS_KEY_ID
- vStorage S3 backend — *requires* — AWS_SECRET_ACCESS_KEY
- deploy scripts — *store creds in* — .env
- deploy scripts — *source* — .env
- .env — *exports* — variables to terraform
- Terraform S3 backend — *authenticates with* — AWS_ACCESS_KEY_ID
- Terraform S3 backend — *authenticates with* — AWS_SECRET_ACCESS_KEY
- Terraform backend block — *cannot read* — terraform.tfvars
- Terraform backend block — *reads* — env vars
- Terraform backend block — *reads* — literal backend config
- terraform.tfvars — *supplies* — PROVIDER creds
- PROVIDER creds — *include* — vngcloud client_id
- PROVIDER creds — *include* — vngcloud client_secret
- PROVIDER creds — *include* — db_password
- deploy-api.sh — *sources* — server/.env
- deploy-api.sh — *accesses* — postgres module
- exported AWS_* — *propagates to* — sibling terraform reads
- TF_VAR_access_key — *is* — vStorage key
- TF_VAR_secret_key — *is* — vStorage secret
- deployments/storage/.env — *contains* — TF_VAR_access_key
- deployments/storage/.env — *contains* — TF_VAR_secret_key
- TF_VAR_access_key — *are* — aws_s3 PROVIDER inputs
- TF_VAR_secret_key — *are* — aws_s3 PROVIDER inputs
- .env — *is* — git-ignored
- ~/.aws/credentials — *contains* — [default] profile
- AWS SDK — *reads* — ~/.aws/credentials
- Terraform S3 backend — *uses* — AWS SDK
- deploy-all.sh — *is a type of* — orchestrator
- orchestrator — *has* — OWN creds path
- deploy-all.sh — *sources* — deployments/lib/tfstate.sh
- deploy-all.sh — *calls* — ensure_vstorage_creds
- ensure_vstorage_creds — *exports* — AWS_*
- ensure_vstorage_creds — *parses* — access_key from storage/terraform.tfvars
- ensure_vstorage_creds — *parses* — secret_key from storage/terraform.tfvars
- ensure_vstorage_creds — *parses* — TF_VAR_access_key from deployments/storage/.env
- ensure_vstorage_creds — *parses* — TF_VAR_secret_key from deployments/storage/.env
- deploy-all.sh — *calls* — ensure_remote_init
- ensure_remote_init — *performs* — terraform init
- terraform init — *initializes* — remote-backend modules
- deploy-all.sh — *is* — creds-self-sufficient
- individual module script — *does not source* — deployments/lib/tfstate.sh
- individual module script — *sources* — its own .env
- leo-customer360 — *uses* — deploy scripts
- This note — *is related to* — Running leo-customer360 deploys locally needs vStorage backend creds; CI can't do monitoring/LB

%% ai-graph-end %%