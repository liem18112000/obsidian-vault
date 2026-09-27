---
ai_hash: aee4d00cf97d4825
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-20
entities:
- CI/CD
- Local Terraform state
- Remote Terraform backend
- IPs
- Secrets
- leo-customer360 deploy scripts
- VM IPs
- DB Host
- Terraform state
- Server module
- Postgres module
- Cache module
- UAT workspace
- Gitignore patterns
- Operator machine
- Git
- GitHub Actions runner
- terraform workspace select
- cd.yml
- terraform init -upgrade
- Terraform providers
- CD run
- Select job
- Deploy job
- deploy-all.sh script
- vStorage
- S3
- Terraform Cloud
- GitHub secrets/vars
- Environment overrides
- Application scripts
- terraform output
- deploy-backend.sh
- BASTION override
- deploy-api.sh
- deploy-ads.sh
- deploy-frontend.sh
- Self-hosted runner
- Operator network
- leo-customer360 CD
- Application containers
- vServers
- vDB/vLB/vStorage Terraform
- Fix option
- Bypass TF in CI
- CI
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
Do NOT commit tfstate (contains secrets; deliberately gitignored). Context: [[leo-customer360 CD deploys app containers to vServers only, never the vDBvLBvStorage Terraform|leo-customer360 CD deploys app containers to vServers only, never the vDB/vLB/vStorage Terraform]].

## Related

- [[leo-customer360 CD deploys app containers to vServers only, never the vDBvLBvStorage Terraform]]

%% ai-graph-start %%

**Related notes:**
- [[leo-customer360 CD deploys app containers to vServers only, never the vDBvLBvStorage Terraform]]
- [[Running leo-customer360 deploys locally needs vStorage backend creds; CI can't do monitoringLB]]
- [[Remote Terraform state needs no manual sync — bake creds + init into the deploy orchestrator to guarantee alignment]]
- [[Deploy an unmerged feature branch to leo-customer360 UAT with BUILD_LOCAL=1]]
- [[leo-customer360 CD builds images on the VM instead of pulling from GHCR (CICD gap)]]

**Relations:**
- CI/CD — *cannot resolve* — Local Terraform state
- CI/CD — *needs* — Remote Terraform backend
- CI/CD — *needs* — IPs
- IPs — *via* — Secrets
- leo-customer360 deploy scripts — *resolves* — VM IPs
- leo-customer360 deploy scripts — *resolves* — DB Host
- leo-customer360 deploy scripts — *resolves from* — Local Terraform state
- Terraform state — *from module* — Server module
- Terraform state — *from module* — Postgres module
- Terraform state — *from module* — Cache module
- Terraform state — *uses workspace* — UAT workspace
- Local Terraform state — *is* — Gitignored
- Gitignore patterns — *applies to* — Local Terraform state
- Local Terraform state — *exists on* — Operator machine
- Local Terraform state — *not in* — Git
- GitHub Actions runner — *lacks* — Local Terraform state
- GitHub Actions runner — *runs* — leo-customer360 deploy scripts
- terraform workspace select — *fails for* — UAT workspace
- cd.yml — *cannot perform* — Deploy job
- terraform init -upgrade — *downloads* — Terraform providers
- terraform init -upgrade — *does not create* — UAT workspace
- terraform init -upgrade — *does not restore* — Terraform state
- CD run — *includes* — Select job
- CD run — *includes* — Deploy job
- Select job — *succeeded in* — CD run
- Deploy job — *failed in* — CD run
- Remote Terraform backend — *is a* — Fix option
- Remote Terraform backend — *uses* — vStorage
- Remote Terraform backend — *uses* — S3
- Remote Terraform backend — *uses* — Terraform Cloud
- Server module — *state moved to* — Remote Terraform backend
- Postgres module — *state moved to* — Remote Terraform backend
- Cache module — *state moved to* — Remote Terraform backend
- CI — *can read* — Remote Terraform backend
- Bypass TF in CI — *is a* — Fix option
- Bypass TF in CI — *uses* — GitHub secrets/vars
- GitHub secrets/vars — *contains* — VM IPs
- GitHub secrets/vars — *contains* — DB Host
- GitHub secrets/vars — *contains* — IPs
- Application scripts — *uses* — Environment overrides
- Environment overrides — *skips* — terraform output
- deploy-backend.sh — *honors* — BASTION override
- deploy-api.sh — *needs* — Environment overrides
- deploy-ads.sh — *needs* — Environment overrides
- deploy-frontend.sh — *needs* — Environment overrides
- Self-hosted runner — *is a* — Fix option
- Self-hosted runner — *on* — Operator network
- Self-hosted runner — *holds* — Local Terraform state
- Terraform state — *contains* — Secrets
- Terraform state — *is* — Gitignored
- leo-customer360 CD — *deploys* — Application containers
- leo-customer360 CD — *deploys to* — vServers
- leo-customer360 CD — *does not deploy* — vDB/vLB/vStorage Terraform

%% ai-graph-end %%