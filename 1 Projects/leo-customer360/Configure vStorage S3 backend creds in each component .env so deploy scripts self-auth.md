---
ai_hash: 8f7652312fead537
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-22
entities:
- vStorage S3 backend credentials
- .env file
- deploy scripts
- leo-customer360
- AWS_ACCESS_KEY_ID
- AWS_SECRET_ACCESS_KEY
- Terraform
- Terraform S3 backend
- server component
- ads-server component
- frontend component
- monitoring component
- proxy component
- sso component
- storage component
- cache component
- postgres component
- load_balancer component
- terraform.tfvars file
- Terraform backend block
- PROVIDER credentials
- vngcloud
- client_id
- client_secret
- db_password
- deploy-api.sh script
- terraform output command
- TF_VAR_access_key
- TF_VAR_secret_key
- aws_s3 PROVIDER inputs
- ~/.aws/credentials file
- '[default] profile'
- AWS SDK
- deploy-all.sh script
- deployments/lib/tfstate.sh script
- ensure_vstorage_creds function
- access_key
- secret_key
- storage/terraform.tfvars file
- storage/.env file
- ensure_remote_init function
- terraform init command
- remote-backend modules
- full-stack deploys
- individual module script
- CI
- 'Related Note: Running leo-customer360 deploys locally needs vStorage backend creds;
  CI can''t do monitoring/LB'
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
[[Running leo-customer360 deploys locally needs vStorage backend creds; CI can't do monitoring/LB]]

## The orchestrator has its OWN creds path (don't confuse the two)
`deploy-all.sh` sources `deployments/lib/tfstate.sh` and calls `ensure_vstorage_creds` — which, if `AWS_*` is unset, exports them by parsing `access_key`/`secret_key` from `storage/terraform.tfvars` (or `TF_VAR_access_key`/`TF_VAR_secret_key` from `storage/.env`), then `ensure_remote_init` `terraform init`s the remote-backend modules. So **`deploy-all.sh` was ALWAYS creds-self-sufficient** (that's why full-stack deploys worked). The per-component `.env` AWS_* only matters when you run an INDIVIDUAL module script directly (e.g. `server/deploy-api.sh uat`), which does NOT source tfstate.sh — it only sources its own `.env`. Verified via `./deploy-all.sh uat --dry-run` (EXIT=0, 14/14 steps printed, nothing executed).

## Related

- [[Running leo-customer360 deploys locally needs vStorage backend creds; CI can't do monitoring/LB]]

%% ai-graph-start %%

**Related notes:**
- [[Running leo-customer360 deploys locally needs vStorage backend creds; CI can't do monitoringLB]]
- [[Remote Terraform state needs no manual sync — bake creds + init into the deploy orchestrator to guarantee alignment]]
- [[customer360 secretconfig flow on VNG GitHub Actions + .env + tfstate-on-vStorage]]
- [[Terraform S3 remote backend for VNG vStorage (S3-compatible) config recipe]]
- [[Terraform S3 backend on a non-AWS store (vStorageMinIO) needs skip-checks + path-style]]

**Relations:**
- vStorage S3 backend credentials — *are configured in* — .env file
- deploy scripts — *use* — vStorage S3 backend credentials
- deploy scripts — *enable* — self-authentication
- leo-customer360 — *uses* — deploy scripts
- deploy scripts — *source* — .env file
- deploy scripts — *export variables to* — Terraform
- AWS_ACCESS_KEY_ID — *is a component of* — vStorage S3 backend credentials
- AWS_SECRET_ACCESS_KEY — *is a component of* — vStorage S3 backend credentials
- AWS_ACCESS_KEY_ID — *and* — AWS_SECRET_ACCESS_KEY
- AWS_ACCESS_KEY_ID — *authenticate* — Terraform S3 backend
- AWS_SECRET_ACCESS_KEY — *authenticate* — Terraform S3 backend
- Terraform S3 backend — *is part of* — Terraform
- Terraform backend block — *cannot read* — terraform.tfvars file
- Terraform backend block — *reads* — environment variables
- Terraform backend block — *reads* — literal backend config
- terraform.tfvars file — *supplies* — PROVIDER credentials
- PROVIDER credentials — *include* — vngcloud client_id
- PROVIDER credentials — *include* — vngcloud client_secret
- PROVIDER credentials — *include* — db_password
- .env file — *is sufficient for* — modules
- modules — *store other secrets in* — terraform.tfvars file
- exported variables — *propagate to* — sibling terraform reads
- deploy-api.sh script — *sources* — server component .env file
- deploy-api.sh script — *executes* — terraform output command
- terraform output command — *inherits* — exported AWS_ACCESS_KEY_ID
- terraform output command — *inherits* — exported AWS_SECRET_ACCESS_KEY
- TF_VAR_access_key — *is a type of* — vStorage S3 backend credentials
- TF_VAR_secret_key — *is a type of* — vStorage S3 backend credentials
- TF_VAR_access_key — *and* — TF_VAR_secret_key
- TF_VAR_access_key — *are* — aws_s3 PROVIDER inputs
- TF_VAR_secret_key — *are* — aws_s3 PROVIDER inputs
- vStorage S3 backend credentials — *are stored in* — storage/.env file
- storage/.env file — *stores* — TF_VAR_access_key
- storage/.env file — *stores* — TF_VAR_secret_key
- .env file — *is* — git-ignored
- ~/.aws/credentials file — *is an alternative for* — vStorage S3 backend credentials
- ~/.aws/credentials file — *contains* — [default] profile
- [default] profile — *contains* — AWS_ACCESS_KEY_ID
- [default] profile — *contains* — AWS_SECRET_ACCESS_KEY
- AWS SDK — *reads* — ~/.aws/credentials file
- Terraform S3 backend — *uses* — AWS SDK
- deploy-all.sh script — *sources* — deployments/lib/tfstate.sh script
- deployments/lib/tfstate.sh script — *calls* — ensure_vstorage_creds function
- ensure_vstorage_creds function — *exports* — AWS_ACCESS_KEY_ID
- ensure_vstorage_creds function — *exports* — AWS_SECRET_ACCESS_KEY
- ensure_vstorage_creds function — *parses* — access_key
- ensure_vstorage_creds function — *parses* — secret_key
- access_key — *from* — storage/terraform.tfvars file
- secret_key — *from* — storage/terraform.tfvars file
- ensure_vstorage_creds function — *parses* — TF_VAR_access_key
- ensure_vstorage_creds function — *parses* — TF_VAR_secret_key
- TF_VAR_access_key — *from* — storage/.env file
- TF_VAR_secret_key — *from* — storage/.env file
- ensure_vstorage_creds function — *calls* — ensure_remote_init function
- ensure_remote_init function — *executes* — terraform init command
- terraform init command — *initializes* — remote-backend modules
- deploy-all.sh script — *is* — creds-self-sufficient
- full-stack deploys — *rely on* — deploy-all.sh script
- individual module script — *does not source* — deployments/lib/tfstate.sh script
- individual module script — *sources* — its own .env file
- per-component .env file — *is relevant for* — individual module script
- Related Note: Running leo-customer360 deploys locally needs vStorage backend creds; CI can't do monitoring/LB — *is related to* — vStorage S3 backend credentials
- Related Note: Running leo-customer360 deploys locally needs vStorage backend creds; CI can't do monitoring/LB — *is related to* — leo-customer360
- Related Note: Running leo-customer360 deploys locally needs vStorage backend creds; CI can't do monitoring/LB — *is related to* — CI

%% ai-graph-end %%