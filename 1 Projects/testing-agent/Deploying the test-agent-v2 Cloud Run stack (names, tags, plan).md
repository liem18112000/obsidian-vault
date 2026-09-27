---
ai_hash: 8480a98f14a5e711
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-16
entities:
- test-agent-v2 Cloud Run stack
- deployments/test-agent-v2/deploy.sh
- klara-nonprod
- Cloud Run service names
- TPD service
- test-plan-definition-agent-v2
- gcloud run services describe test-plan-definition-agent
- Terraform
- module.kga|tpd|tev.google_cloud_run_v2_service.this[0]
- Image tag convention
- europe-west6-docker.pkg.dev/klara-nonprod/kga-v2/test-agent-v2:<shortsha>-<label>
- a15b62d-chunked-implement
- 5603766-benchmark-cache
- klara-nonprod/kga-v2
- tfvars `image=`
- build-image.sh
- klara-repo
- kga
- tpd
- tev
- gateway
- ONE image
- '`PLAN=1 bash deploy.sh`'
- read-only preview
- 5603766 "benchmark + pluggable cache (Redis/Memorystore)" commit
- cache feature
- Terraform infra
- Memorystore
- Cloud Run image updates
- Cloud Run revision
- image STRING
- same tag
- fresh tag
- live tag
- target tag
- deploy.sh
- gcloud builds submit --async
- builds describe
- builds
- full apply
- Fix TPD scenario generator truncation — raise max_tokens
- keep one call
- Redis
source: Testing-Agent run-188f96b8 deploy
status: seedling
tags:
- testing-agent
- deploy
- cloud-run
- terraform
- klara-nonprod
title: Deploying the test-agent-v2 Cloud Run stack (names, tags, plan)
type: howto
---

# Deploying the test-agent-v2 Cloud Run stack (names, tags, plan)

Facts for deploying the **test-agent-v2** stack (deployments/test-agent-v2/deploy.sh, project klara-nonprod):

- **Cloud Run service names carry a `-v2` suffix**: the TPD service is `test-plan-definition-agent-v2` (a `gcloud run services describe test-plan-definition-agent` without `-v2` returns "Cannot find service"). Terraform addresses them as `module.kga|tpd|tev.google_cloud_run_v2_service.this[0]`.
- **Image tag convention**: `europe-west6-docker.pkg.dev/klara-nonprod/kga-v2/test-agent-v2:<shortsha>-<label>` (e.g. `a15b62d-chunked-implement`, `5603766-benchmark-cache`). The registry is `klara-nonprod/kga-v2` — the tfvars `image=` overrides build-image.sh's `klara-repo` default. All 4 services (kga/tpd/tev + gateway) share ONE image.
- **`PLAN=1 bash deploy.sh`** is a safe read-only preview. For the 5603766 "benchmark + pluggable cache (Redis/Memorystore)" commit the plan was `0 to add, 5 to change, 0 to destroy` — i.e. the cache feature provisions NO new terraform infra (no Memorystore created by terraform); the 5 changes are just in-place Cloud Run image updates.
- **New revision guarantee**: terraform only rolls a new Cloud Run revision when the image STRING changes. Deploying the same tag that's already live is a no-op (no new revision → new build not pulled) — use a fresh tag when the live tag already matches. Here live was `a15b62d-...` vs target `5603766-...`, so it rolled cleanly.
- deploy.sh builds via `gcloud builds submit --async` + polls `builds describe` (tolerant of the "can only stream logs" exit-code quirk); builds BEFORE the full apply.

## Related

- [[Fix TPD scenario generator truncation — raise max_tokens, keep one call]]

%% ai-graph-start %%

**Related notes:**
- [[test-agent-v2 hardened deploy.sh flow and the unique image-tag bump that forces a new revision]]
- [[test-agent-v2 Cloud Run services use a -v2 name suffix]]
- [[Adding a Cloud Run service that shares one image var build first, targeted apply]]
- [[test-agent-v2 deploy + get_deliverables E2E verification]]
- [[test-agent-v2 cloud resource and credential map (klara-nonprod)]]

**Relations:**
- test-agent-v2 Cloud Run stack — *is deployed by* — deployments/test-agent-v2/deploy.sh
- test-agent-v2 Cloud Run stack — *is in project* — klara-nonprod
- Cloud Run service names — *carry suffix* — `-v2`
- TPD service — *is named* — test-plan-definition-agent-v2
- gcloud run services describe test-plan-definition-agent — *describes* — TPD service
- Terraform — *addresses service as* — module.kga|tpd|tev.google_cloud_run_v2_service.this[0]
- Image tag convention — *is* — europe-west6-docker.pkg.dev/klara-nonprod/kga-v2/test-agent-v2:<shortsha>-<label>
- a15b62d-chunked-implement — *is an example of* — Image tag convention
- 5603766-benchmark-cache — *is an example of* — Image tag convention
- klara-nonprod/kga-v2 — *is the registry for* — Image tag convention
- tfvars `image=` — *overrides* — klara-repo
- klara-repo — *is default in* — build-image.sh
- kga — *shares* — ONE image
- tpd — *shares* — ONE image
- tev — *shares* — ONE image
- gateway — *shares* — ONE image
- `PLAN=1 bash deploy.sh` — *is a* — read-only preview
- 5603766 "benchmark + pluggable cache (Redis/Memorystore)" commit — *implements* — cache feature
- cache feature — *provisions no new* — Terraform infra
- Memorystore — *is not created by* — Terraform
- Cloud Run image updates — *are* — 5 changes
- Terraform — *rolls new* — Cloud Run revision
- Cloud Run revision — *requires change in* — image STRING
- Deploying the same tag — *is a* — no-op
- fresh tag — *should be used over* — live tag
- live tag — *was* — a15b62d-chunked-implement
- target tag — *was* — 5603766-benchmark-cache
- deploy.sh — *builds via* — gcloud builds submit --async
- deploy.sh — *polls* — builds describe
- builds — *occur before* — full apply
- test-agent-v2 Cloud Run stack — *is related to* — Fix TPD scenario generator truncation — raise max_tokens
- test-agent-v2 Cloud Run stack — *is related to* — keep one call
- cache feature — *can use* — Redis
- cache feature — *can use* — Memorystore

%% ai-graph-end %%