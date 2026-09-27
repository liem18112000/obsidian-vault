---
ai_hash: 89c2ed09972470ed
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-31
entities:
- deploy-tracking.sh
- uat
- GHCR auth
- gh auth token
- BUILD_LOCAL=1
- leo-customer360
- CI image
- ghcr.io/leo-cdp/leo-customer360/data-tracking-api
- PRIVATE GHCR package
- registry auth
- 'Error response from daemon: error from registry: denied'
- GitHub CLI
- PAT
- read:packages
- GHCR_USER
- GHCR_TOKEN
- docker login
- docker manifest inspect
- Build on the VM
- Terraform S3 backend
- vStorage
- AWS_ACCESS_KEY_ID
- AWS_SECRET_ACCESS_KEY
- environment variables
- Terraform state
- deployments/server/.env
- manual terraform call
- No valid credential sources found
- Scale one uvicorn service into N replicas on one VM with a docker bridge + local
  nginx LB
source: session 2026-08-31 uat deploy
status: seedling
tags:
- leo-customer360
- ghcr
- deployment
- terraform
- gotcha
title: 'deploy-tracking.sh uat needs GHCR auth: gh auth token or BUILD_LOCAL=1'
type: lesson
---

# deploy-tracking.sh uat needs GHCR auth: gh auth token or BUILD_LOCAL=1

Deploying `deployments/server/deploy-tracking.sh <env>` in leo-customer360 pulls the CI image `ghcr.io/leo-cdp/leo-customer360/data-tracking-api`, which is a **PRIVATE** GHCR package. Without registry auth the pull fails with `Error response from daemon: error from registry: denied`.

## Two fixes

- **Auth the pull** with the logged-in GitHub CLI (no PAT needed if `gh` has `read:packages`):
  ```bash
  GHCR_USER=<gh-username> GHCR_TOKEN="$(gh auth token)" ./deploy-tracking.sh uat
  ```
  The script does `docker login ghcr.io -u $GHCR_USER --password-stdin` only when `GHCR_TOKEN` is set. Verify access first with `docker manifest inspect <image@sha>` after `gh auth token | docker login ghcr.io -u <user> --password-stdin`.
- **Build on the VM instead** (no registry access at all):
  ```bash
  BUILD_LOCAL=1 ./deploy-tracking.sh uat
  ```

## Also required
The Terraform **S3 backend on vStorage** needs `AWS_ACCESS_KEY_ID` / `AWS_SECRET_ACCESS_KEY` in the environment to read state (`terraform workspace select` / `terraform output servers`). The script auto-sources `deployments/server/.env`, which carries them — but a bare manual `terraform` call outside the script fails with "No valid credential sources found" until you `set -a; source ./.env`.

Related: [[Scale one uvicorn service into N replicas on one VM with a docker bridge + local nginx LB]].

## Related

- [[Scale one uvicorn service into N replicas on one VM with a docker bridge + local nginx LB]]

%% ai-graph-start %%

**Related notes:**
- [[Running leo-customer360 deploys locally needs vStorage backend creds; CI can't do monitoringLB]]
- [[leo-customer360 VNG deploy builds app images on the VM from a tarred local checkout, not from a registry]]
- [[Local deploy pull from GHCR needs a token with readpackages — gh default token lacks it (403)]]
- [[Configure vStorage S3 backend creds in each component .env so deploy scripts self-auth]]
- [[CI-driven CD cannot resolve local gitignored Terraform state — needs remote backend or IPs via secrets]]

**Relations:**
- deploy-tracking.sh — *deploys to* — uat
- deploy-tracking.sh — *requires* — GHCR auth
- GHCR auth — *can be obtained via* — gh auth token
- GHCR auth — *can be bypassed by* — BUILD_LOCAL=1
- deploy-tracking.sh — *pulls* — CI image
- CI image — *is* — ghcr.io/leo-cdp/leo-customer360/data-tracking-api
- ghcr.io/leo-cdp/leo-customer360/data-tracking-api — *is a* — PRIVATE GHCR package
- lack of — *registry auth results in* — Error response from daemon: error from registry: denied
- gh auth token — *is part of* — GitHub CLI
- GitHub CLI — *needs* — read:packages permission
- GHCR_TOKEN — *is set by* — gh auth token
- deploy-tracking.sh — *uses* — GHCR_USER
- deploy-tracking.sh — *uses* — GHCR_TOKEN
- deploy-tracking.sh — *performs* — docker login
- docker login — *uses* — GHCR_USER
- docker login — *uses* — GHCR_TOKEN
- docker manifest inspect — *verifies* — access
- Build on the VM — *is an alternative to* — GHCR auth
- Build on the VM — *is enabled by* — BUILD_LOCAL=1
- Terraform S3 backend — *is hosted on* — vStorage
- Terraform S3 backend — *requires* — AWS_ACCESS_KEY_ID
- Terraform S3 backend — *requires* — AWS_SECRET_ACCESS_KEY
- AWS_ACCESS_KEY_ID — *are* — environment variables
- AWS_SECRET_ACCESS_KEY — *are* — environment variables
- Terraform — *reads* — Terraform state
- deployments/server/.env — *contains* — AWS_ACCESS_KEY_ID
- deployments/server/.env — *contains* — AWS_SECRET_ACCESS_KEY
- deploy-tracking.sh — *sources* — deployments/server/.env
- manual terraform call — *fails with* — No valid credential sources found
- deploy-tracking.sh — *is related to* — Scale one uvicorn service into N replicas on one VM with a docker bridge + local nginx LB

%% ai-graph-end %%