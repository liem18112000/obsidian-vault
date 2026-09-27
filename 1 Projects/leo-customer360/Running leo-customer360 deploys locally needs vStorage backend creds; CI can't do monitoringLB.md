---
ai_hash: 5056a90026af6823
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-21
entities:
- leo-customer360
- vStorage
- CI
- CD pipeline
- monitoring
- load-balancer
- deploy script
- vStorage S3 backend creds
- Terraform OUTPUTS
- VNG
- S3-compatible
- leocdp360-tfstate bucket
- AWS_ACCESS_KEY_ID
- AWS_SECRET_ACCESS_KEY
- VSTORAGE_ACCESS_KEY
- VSTORAGE_SECRET_KEY
- CI secrets
- Local secret .env files
- Portainer pw
- oauth2 client+cookie secrets
- KEYCLOAK_ADMIN_PASSWORD
- SSH key
- Terraform
- python3
- CD deploy job
- DB/REDIS passwords
- server
- postgres
- cache
- infra Terraform deploy.sh scripts
- storage
- Jaeger-gate
- LB-backend changes
- operator
- Docker containers
- VNG vServer VMs
- vServer IPs
- DB host
- redis host
- deploy-all.sh
source: session 2026-08-21
status: seedling
tags:
- leo-customer360
- terraform
- vstorage
- deployment
- credentials
- gotcha
title: Running leo-customer360 deploys locally needs vStorage backend creds; CI can't
  do monitoring/LB
type: lesson
---

# Running leo-customer360 deploys locally needs vStorage backend creds; CI can't do monitoring/LB

To run any **leo-customer360** deploy script locally you need TWO credential sets — and the CD pipeline deliberately can't cover the monitoring / load-balancer steps.

## Local requirements
1. **vStorage S3 backend creds** — every deploy script reads vServer IPs / DB host / redis host from Terraform OUTPUTS, whose state lives on VNG **vStorage** (S3-compatible, bucket `leocdp360-tfstate`, endpoint `https://hcm04.vstorage.vngcloud.vn`, region `us-east-1`, `workspace_key_prefix=env`). Auth is purely env: `AWS_ACCESS_KEY_ID` / `AWS_SECRET_ACCESS_KEY` (the `VSTORAGE_ACCESS_KEY`/`VSTORAGE_SECRET_KEY` CI secrets). Without them terraform errors **'No valid credential sources found'** and the deploy dies at the `terraform output servers` step (before any SSH). Set them via `export` or a `~/.aws/credentials` `[default]` profile (the S3 backend has `skip_credentials_validation=true`, so no STS needed).
2. **Local secret .env files** — `deployments/monitoring/.env` (Portainer pw, oauth2 client+cookie secrets) and `deployments/sso/.env` (`KEYCLOAK_ADMIN_PASSWORD`). Git-ignored, machine-local.
3. SSH key `~/.ssh/c360-api_ed25519`; terraform >= 1.14.8; python3.

## Why CI can't deploy monitoring or load_balancer
The CD deploy job has AWS_* (vStorage), DEPLOY_SSH_KEY, DB/REDIS passwords — and `terraform init`s only **server/postgres/cache** read-only. It runs `deploy-all.sh <env> --only <services>` and by design **NEVER** runs the infra Terraform `deploy.sh` scripts (server/storage/postgres/**load_balancer**). And **monitoring** needs the KEYCLOAK_ADMIN_PASSWORD + oauth2 `.env` secrets which are NOT in CI. So Jaeger-gate / LB-backend changes must be applied by an **operator locally** (creds #1 + #2), not by `--deploy-uat` nor `workflow_dispatch`.

## Related
[[leo-customer360 CD UAT deploys only from main + --deploy-uat marker]]
[[leo-customer360 deploys as Docker containers on VNG vServer VMs over SSH]]

## Related

- [[leo-customer360 CD UAT deploys only from main + --deploy-uat marker]]
- [[leo-customer360 deploys as Docker containers on VNG vServer VMs over SSH]]

%% ai-graph-start %%

**Related notes:**
- [[Configure vStorage S3 backend creds in each component .env so deploy scripts self-auth]]
- [[CI-driven CD cannot resolve local gitignored Terraform state — needs remote backend or IPs via secrets]]
- [[deploy-tracking.sh uat needs GHCR auth gh auth token or BUILD_LOCAL=1]]
- [[Remote Terraform state needs no manual sync — bake creds + init into the deploy orchestrator to guarantee alignment]]
- [[customer360 secretconfig flow on VNG GitHub Actions + .env + tfstate-on-vStorage]]

**Relations:**
- leo-customer360 — *needs for local deploys* — vStorage S3 backend creds
- leo-customer360 — *needs for local deploys* — Local secret .env files
- CI — *cannot do* — monitoring
- CI — *cannot do* — load-balancer
- deploy script — *reads* — vServer IPs
- deploy script — *reads* — DB host
- deploy script — *reads* — redis host
- vServer IPs — *from* — Terraform OUTPUTS
- DB host — *from* — Terraform OUTPUTS
- redis host — *from* — Terraform OUTPUTS
- Terraform OUTPUTS — *state lives on* — vStorage
- vStorage — *is* — S3-compatible
- vStorage — *is part of* — VNG
- vStorage — *uses* — leocdp360-tfstate bucket
- vStorage S3 backend creds — *are* — AWS_ACCESS_KEY_ID
- vStorage S3 backend creds — *are* — AWS_SECRET_ACCESS_KEY
- VSTORAGE_ACCESS_KEY — *is alias for* — AWS_ACCESS_KEY_ID
- VSTORAGE_SECRET_KEY — *is alias for* — AWS_SECRET_ACCESS_KEY
- CI secrets — *contain* — VSTORAGE_ACCESS_KEY
- CI secrets — *contain* — VSTORAGE_SECRET_KEY
- Local secret .env files — *contain* — Portainer pw
- Local secret .env files — *contain* — oauth2 client+cookie secrets
- Local secret .env files — *contain* — KEYCLOAK_ADMIN_PASSWORD
- leo-customer360 — *requires for local deploys* — SSH key
- leo-customer360 — *requires for local deploys* — Terraform
- leo-customer360 — *requires for local deploys* — python3
- CD deploy job — *has access to* — AWS_*
- CD deploy job — *has access to* — DEPLOY_SSH_KEY
- CD deploy job — *has access to* — DB/REDIS passwords
- CD deploy job — *runs* — terraform init
- terraform init — *for* — server
- terraform init — *for* — postgres
- terraform init — *for* — cache
- server — *is* — read-only
- postgres — *is* — read-only
- cache — *is* — read-only
- CD deploy job — *runs* — deploy-all.sh
- CD deploy job — *NEVER runs* — infra Terraform deploy.sh scripts
- infra Terraform deploy.sh scripts — *include* — server
- infra Terraform deploy.sh scripts — *include* — storage
- infra Terraform deploy.sh scripts — *include* — postgres
- infra Terraform deploy.sh scripts — *include* — load-balancer
- monitoring — *needs* — KEYCLOAK_ADMIN_PASSWORD
- monitoring — *needs* — oauth2 client+cookie secrets
- KEYCLOAK_ADMIN_PASSWORD — *is NOT in* — CI
- oauth2 client+cookie secrets — *are NOT in* — CI
- Jaeger-gate — *changes applied by* — operator
- LB-backend changes — *applied by* — operator
- operator — *uses* — vStorage S3 backend creds
- operator — *uses* — Local secret .env files
- leo-customer360 — *deploys as* — Docker containers
- Docker containers — *on* — VNG vServer VMs
- VNG vServer VMs — *accessed via* — SSH
- CD pipeline — *cannot cover* — monitoring
- CD pipeline — *cannot cover* — load-balancer

%% ai-graph-end %%