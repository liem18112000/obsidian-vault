---
ai_hash: 04cb6294a75629b0
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-20
entities:
- leo-customer360 CD
- VM
- GitHub Container Registry (GHCR)
- deploy scripts
- deployments/
- customer360-api
- server/deploy-api.sh
- backend-system
- server/deploy-backend.sh
- ads-server
- ads-server/deploy-ads.sh
- frontend-admin
- frontend/deploy-frontend.sh
- app services
- source code
- docker build
- docker run
- CI
- .github/workflows/ci.yml
- ghcr.io/leo-cdp/leo-customer360/<service>
- SHA-tagged images
- latest tag
- main branch
- CI/CD gap
- GHCR reference
- environments
- release→prod flow
- tag/release trigger
- semver tag
- Terraform workspaces
- overlays/<env>.tfvars
- uat environment
- prod environment
- deploy-all.sh
- Get the newest GHCR image tag or digest via gh api packages container versions
source: session 2026-08-20, deployments/ review
status: seedling
tags:
- leo-customer360
- cd
- ghcr
- deployment
- terraform
- vngcloud
title: leo-customer360 CD builds images on the VM instead of pulling from GHCR (CI/CD
  gap)
type: observation
---

# leo-customer360 CD builds images on the VM instead of pulling from GHCR (CI/CD gap)

As of 2026-08-20, the `leo-customer360` deploy scripts under `deployments/` do **not** consume images from GitHub Container Registry at all. All four app services — `customer360-api` (`server/deploy-api.sh`), `backend-system` (`server/deploy-backend.sh`), `ads-server` (`ads-server/deploy-ads.sh`), `frontend-admin` (`frontend/deploy-frontend.sh`) — use the same pattern: `tar -czf - <svc> | ssh … tar -xzf -` to ship source to the VM, then `docker build -t <name> /opt/c360/<svc>` and `docker run` **on the box**.

Meanwhile CI (`.github/workflows/ci.yml`) builds and pushes `ghcr.io/leo-cdp/leo-customer360/<service>` tagged `sha-<full-git-sha>` (from `type=sha,format=long`) plus `latest`, but **only on `main`** (`push:` gated to `refs/heads/main`); branches/PRs are build-only.

**The gap:** CI produces immutable SHA-tagged images in GHCR that CD never uses — CD rebuilds from source on the VM instead. Wiring true CD means changing the deploy scripts to `docker pull` a resolved GHCR reference per env instead of building (see [[Get the newest GHCR image tag or digest via gh api packages container versions]]), and — for a release→prod flow — adding a tag/release trigger + semver tag to CI (currently there is no version-tag build). Environments are Terraform workspaces + `overlays/<env>.tfvars`; only the **uat** overlay is provisioned so far (prod is documented as "to be added"). Orchestrated by `deploy-all.sh <uat|prod>` (15 ordered steps).

## Related

- [[Get the newest GHCR image tag or digest via gh api packages container versions]]

%% ai-graph-start %%

**Related notes:**
- [[leo-customer360 VNG deploy builds app images on the VM from a tarred local checkout, not from a registry]]
- [[Feature-branch images are never pushed to GHCR in leo-customer360 CI]]
- [[CD deploy can transiently 404 on a just-built GHCR digest - re-run the failed job]]
- [[Get the newest GHCR image tag or digest via gh api packages container versions]]
- [[leo-customer360 CD UAT deploys only from main + --deploy-uat marker]]

**Relations:**
- leo-customer360 CD — *builds images on* — VM
- leo-customer360 CD — *does not pull from* — GitHub Container Registry (GHCR)
- deploy scripts — *are located in* — deployments/
- deploy scripts — *do not consume images from* — GitHub Container Registry (GHCR)
- customer360-api — *uses* — server/deploy-api.sh
- backend-system — *uses* — server/deploy-backend.sh
- ads-server — *uses* — ads-server/deploy-ads.sh
- frontend-admin — *uses* — frontend/deploy-frontend.sh
- customer360-api — *is an* — app services
- backend-system — *is an* — app services
- ads-server — *is an* — app services
- frontend-admin — *is an* — app services
- app services — *ship* — source code
- app services — *perform* — docker build
- app services — *perform* — docker run
- CI — *is defined in* — .github/workflows/ci.yml
- CI — *builds and pushes* — ghcr.io/leo-cdp/leo-customer360/<service>
- ghcr.io/leo-cdp/leo-customer360/<service> — *is tagged with* — SHA-tagged images
- ghcr.io/leo-cdp/leo-customer360/<service> — *is tagged with* — latest tag
- CI — *pushes to* — GitHub Container Registry (GHCR)
- CI — *pushes only on* — main branch
- CI — *produces* — SHA-tagged images
- SHA-tagged images — *are in* — GitHub Container Registry (GHCR)
- leo-customer360 CD — *never uses* — SHA-tagged images
- leo-customer360 CD — *rebuilds from* — source code
- CI/CD gap — *is* — CD rebuilds from source on VM instead of using CI-built images
- Wiring true CD — *requires changing* — deploy scripts
- deploy scripts — *should* — docker pull
- docker pull — *a* — GHCR reference
- GHCR reference — *is for* — environments
- release→prod flow — *requires* — tag/release trigger
- release→prod flow — *requires* — semver tag
- semver tag — *to* — CI
- environments — *are implemented as* — Terraform workspaces
- environments — *use* — overlays/<env>.tfvars
- uat environment — *is an example of* — environments
- prod environment — *is an example of* — environments
- uat environment — *is provisioned* — so far
- deploy-all.sh — *orchestrates* — environments
- deploy-all.sh — *supports* — uat environment
- deploy-all.sh — *supports* — prod environment
- CI/CD gap — *is related to* — Get the newest GHCR image tag or digest via gh api packages container versions
- Get the newest GHCR image tag or digest via gh api packages container versions — *provides* — GHCR reference

%% ai-graph-end %%