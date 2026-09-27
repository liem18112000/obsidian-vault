---
title: "Deploying the test-agent-v2 Cloud Run stack (names, tags, plan)"
created: 2026-09-16
type: howto
status: seedling
source: "Testing-Agent run-188f96b8 deploy"
tags: [testing-agent, deploy, cloud-run, terraform, klara-nonprod]
---

# Deploying the test-agent-v2 Cloud Run stack (names, tags, plan)

Facts for deploying the **test-agent-v2** stack (deployments/test-agent-v2/deploy.sh, project klara-nonprod):

- **Cloud Run service names carry a `-v2` suffix**: the TPD service is `test-plan-definition-agent-v2` (a `gcloud run services describe test-plan-definition-agent` without `-v2` returns "Cannot find service"). Terraform addresses them as `module.kga|tpd|tev.google_cloud_run_v2_service.this[0]`.
- **Image tag convention**: `europe-west6-docker.pkg.dev/klara-nonprod/kga-v2/test-agent-v2:<shortsha>-<label>` (e.g. `a15b62d-chunked-implement`, `5603766-benchmark-cache`). The registry is `klara-nonprod/kga-v2` — the tfvars `image=` overrides build-image.sh's `klara-repo` default. All 4 services (kga/tpd/tev + gateway) share ONE image.
- **`PLAN=1 bash deploy.sh`** is a safe read-only preview. For the 5603766 "benchmark + pluggable cache (Redis/Memorystore)" commit the plan was `0 to add, 5 to change, 0 to destroy` — i.e. the cache feature provisions NO new terraform infra (no Memorystore created by terraform); the 5 changes are just in-place Cloud Run image updates.
- **New revision guarantee**: terraform only rolls a new Cloud Run revision when the image STRING changes. Deploying the same tag that's already live is a no-op (no new revision → new build not pulled) — use a fresh tag when the live tag already matches. Here live was `a15b62d-...` vs target `5603766-...`, so it rolled cleanly.
- deploy.sh builds via `gcloud builds submit --async` + polls `builds describe` (tolerant of the "can only stream logs" exit-code quirk); builds BEFORE the full apply.

## Related

- [[Fix TPD scenario generator truncation — raise max_tokens]]
- [[keep one call]]
