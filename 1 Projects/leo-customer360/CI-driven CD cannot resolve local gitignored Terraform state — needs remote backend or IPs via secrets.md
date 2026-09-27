---
ai_hash: 717e6adf4214f231
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-20
entities:
- CI-driven CD
- Local gitignored Terraform state
- Remote backend
- IPs via secrets
- leo-customer360 deploy scripts
- VM IPs
- DB host
- Terraform state
- server module
- postgres module
- cache module
- uat workspace
- operator machine
- git
- GitHub Actions runner
- cd.yml
- '`terraform workspace select uat`'
- Terraform workspace
- Terraform providers
- '`init -upgrade`'
- CD run 32384496374
- '`bash deploy-all.sh`'
- secrets
- Remote Terraform backend
- vStorage
- S3
- Terraform Cloud
- CI
- GitHub secrets/vars
- Environment overrides
- App scripts
- '`terraform output`'
- deploy-backend.sh
- BASTION override
- deploy-api.sh
- deploy-ads.sh
- deploy-frontend.sh
- Self-hosted runner
- Operator network
- leo-customer360 CD
- App containers
- vServers
- vDB
- vLB
- vStorage Terraform
source: session 2026-08-20, CD run 32384496374
status: seedling
tags:
- leo-customer360
- cd
- terraform
- state
- ci
- gotcha
title: CI-driven CD cannot resolve local gitignored Terraform state — needs remote
  backend or IPs via secrets
type: observation
---

# CI-driven CD cannot resolve local gitignored Terraform state — needs remote backend or IPs via secrets

The leo-customer360 deploy scripts resolve VM IPs / DB host from **local** Terraform state (`terraform output` in the `server`/`postgres`/`cache` modules, workspace `uat`). That state is **gitignored** (`server/.gitignore`: `*.tfstate`, `terraform.tfstate.d/`, `.terraform/`) — it exists only on the operator machine, not in git.

Consequence: running the same scripts from a GitHub Actions runner (fresh checkout, no state) fails at `terraform workspace select uat` → **"no uat server workspace"**, so cd.yml cannot deploy even with `init -upgrade` (init downloads providers but does not create the workspace or restore state). Confirmed in CD run 32384496374 (2026-08-20): select job OK (uat), deploy job failed here. All prior blockers were already fixed (exit 126 → `bash deploy-all.sh`; secrets set).

Fix options (architectural, pick one):
1. **Remote Terraform backend** (proper) — move `server`/`postgres`/`cache` state to a shared backend (vStorage/S3 or Terraform Cloud) so CI can read it.
2. **Bypass TF in CI** — pass VM IPs + DB host + redis host as GitHub secrets/vars and add env overrides to the app scripts so they skip `terraform output` when provided. `deploy-backend.sh` already honors a `BASTION` override; `deploy-api.sh`/`deploy-ads.sh`/`deploy-frontend.sh` would need the same for their host + DB host.
3. **Self-hosted runner** on the operator network that already holds the local state (least clean).
Do NOT commit tfstate (contains secrets; deliberately gitignored). Context: [[leo-customer360 CD deploys app containers to vServers only, never the vDB/vLB/vStorage Terraform]].

## Related

- [[leo-customer360 CD deploys app containers to vServers only]]
- [[never the vDB/vLB/vStorage Terraform]]

%% ai-graph-start %%

**Related notes:**
- [[leo-customer360 CD deploys app containers to vServers only, never the vDBvLBvStorage Terraform]]
- [[Running leo-customer360 deploys locally needs vStorage backend creds; CI can't do monitoringLB]]
- [[Remote Terraform state needs no manual sync — bake creds + init into the deploy orchestrator to guarantee alignment]]
- [[Deploy an unmerged feature branch to leo-customer360 UAT with BUILD_LOCAL=1]]
- [[leo-customer360 CD builds images on the VM instead of pulling from GHCR (CICD gap)]]

**Relations:**
- CI-driven CD — *cannot resolve* — Local gitignored Terraform state
- CI-driven CD — *needs* — Remote backend
- CI-driven CD — *needs* — IPs via secrets
- leo-customer360 deploy scripts — *resolve* — VM IPs
- leo-customer360 deploy scripts — *resolve* — DB host
- VM IPs — *from* — Terraform state
- DB host — *from* — Terraform state
- Terraform state — *is in* — server module
- Terraform state — *is in* — postgres module
- Terraform state — *is in* — cache module
- Terraform state — *is for* — uat workspace
- Local gitignored Terraform state — *exists only on* — operator machine
- Local gitignored Terraform state — *is not in* — git
- GitHub Actions runner — *lacks* — Terraform state
- GitHub Actions runner — *causes failure in* — cd.yml
- cd.yml — *fails at* — `terraform workspace select uat`
- cd.yml — *cannot deploy* — App containers
- `init -upgrade` — *downloads* — Terraform providers
- `init -upgrade` — *does not create* — Terraform workspace
- `init -upgrade` — *does not restore* — Terraform state
- CD run 32384496374 — *confirmed failure in* — cd.yml
- exit 126 — *fixed by* — `bash deploy-all.sh`
- Remote Terraform backend — *is a fix option for* — CI-driven CD
- Remote Terraform backend — *moves Terraform state to* — vStorage
- Remote Terraform backend — *moves Terraform state to* — S3
- Remote Terraform backend — *moves Terraform state to* — Terraform Cloud
- CI — *can read* — Remote Terraform backend
- Bypass TF in CI — *is a fix option for* — CI-driven CD
- Bypass TF in CI — *passes* — VM IPs
- Bypass TF in CI — *passes* — DB host
- VM IPs — *as* — GitHub secrets/vars
- DB host — *as* — GitHub secrets/vars
- Bypass TF in CI — *adds* — Environment overrides
- Environment overrides — *to* — App scripts
- App scripts — *skip* — `terraform output`
- App scripts — *when provided with* — Environment overrides
- deploy-backend.sh — *honors* — BASTION override
- deploy-api.sh — *would need* — Environment overrides
- deploy-ads.sh — *would need* — Environment overrides
- deploy-frontend.sh — *would need* — Environment overrides
- Self-hosted runner — *is a fix option for* — CI-driven CD
- Self-hosted runner — *is on* — Operator network
- Operator network — *holds* — Local gitignored Terraform state
- Terraform state — *contains* — secrets
- Terraform state — *is* — gitignored
- leo-customer360 CD — *deploys* — App containers
- App containers — *to* — vServers
- leo-customer360 CD — *never deploys* — vDB
- leo-customer360 CD — *never deploys* — vLB
- leo-customer360 CD — *never deploys* — vStorage Terraform

%% ai-graph-end %%