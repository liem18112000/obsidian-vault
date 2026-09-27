---
ai_hash: 8b7795626e87eb7f
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-14
entities:
- Feature-branch images
- GHCR
- leo-customer360 CI
- .github/workflows/ci.yml
- build-and-push job
- github.ref
- refs/heads/main
- vX.Y.Z tag
- pull_request events
- sha-<branch-sha> image
- CD
- not found error
- cd.yml
- GITHUB_TOKEN
- 'packages: read permission'
- E2E (customer360-api -> UAT) job
- live UAT
- beta.leocdp.com/c360api
- DB migration
- main branch
- sha-<main-sha> image
- changes job
- test job
- auto-deploy
- monitoring
- Portainer agents
- CD deploy can transiently 404 on a just-built GHCR digest - re-run the failed job
  (note)
- leo-customer360 push to main skips the monitoring step; deploy Portainer agents
  manually (note)
- transient 404
- just-built digest
- branch code
- CI success
- red E2E
- UAT
- new code
- image
- Feature-branch images are never pushed to GHCR in leo-customer360 CI (note)
source: 'session 2026-09-14, PR #66'
status: seedling
tags:
- leo-customer360
- ci-cd
- github-actions
- ghcr
- e2e
- gotcha
- SCRUM-92
title: Feature-branch images are never pushed to GHCR in leo-customer360 CI
type: lesson
---

# Feature-branch images are never pushed to GHCR in leo-customer360 CI

In `.github/workflows/ci.yml` the **build-and-push** job only logs in to GHCR and sets `push: true` when `github.ref == refs/heads/main` **or** a `vX.Y.Z` tag. On feature branches and `pull_request` events it is **build-only by design** — so **no `sha-<branch-sha>` image is ever pushed to GHCR** for a feature branch.

Consequences:
- The CD `not found` error when deploying a feature-branch image tag is **EXPECTED**, not a broken/underscoped token. `cd.yml` pulls with the built-in `GITHUB_TOKEN` (`packages: read`), which would read the image fine **if it existed**. The image simply was never pushed. (Distinct from a *transient* 404 on a real just-built digest — see [[CD deploy can transiently 404 on a just-built GHCR digest - re-run the failed job]].)
- The `E2E (customer360-api -> UAT)` job tests **live UAT** (`beta.leocdp.com/c360api`) — i.e. whatever is **deployed**. So a feature branch E2E can **never** go green from a DB migration alone; the branch code must first **land on main** (only main/tag pushes publish images + auto-deploy UAT).

Convergence sequence after merging the feature to `main`:
1. CI `build-and-push` publishes `sha-<main-sha>` **regardless** of the E2E result (build-and-push depends on `[changes, test]`, not on e2e).
2. Manually dispatch CD: `gh workflow run cd.yml -f environment=uat -f image_tag=sha-<main-sha> -f services=api,...` (auto-deploy only fires when CI *concludes success*, which a red E2E prevents).
3. Re-run the failed E2E job (`gh run rerun <id> --failed`) once UAT serves the new code -> green.

Note the auto-deploy default set excludes monitoring: see [[leo-customer360 push to main skips the monitoring step; deploy Portainer agents manually]].

## Related

- [[CD deploy can transiently 404 on a just-built GHCR digest - re-run the failed job]]
- [[leo-customer360 push to main skips the monitoring step; deploy Portainer agents manually]]

%% ai-graph-start %%

**Related notes:**
- [[leo-customer360 CD builds images on the VM instead of pulling from GHCR (CICD gap)]]
- [[CD deploy can transiently 404 on a just-built GHCR digest - re-run the failed job]]
- [[leo-customer360 CD UAT deploys only from main + --deploy-uat marker]]
- [[CD didn't fire on an infra-only merge (CI paths-ignore); re-run a workflow_run deploy via workflow_dispatch, not run rerun]]
- [[Chain a CD workflow after CI with workflow_run, gating on conclusion and ref]]

**Relations:**
- Feature-branch images — *are never pushed to* — GHCR
- leo-customer360 CI — *uses* — .github/workflows/ci.yml
- .github/workflows/ci.yml — *defines* — build-and-push job
- build-and-push job — *logs in to* — GHCR
- build-and-push job — *pushes images when* — github.ref == refs/heads/main
- build-and-push job — *pushes images when* — vX.Y.Z tag
- feature branches — *trigger* — build-only
- pull_request events — *trigger* — build-only
- build-only — *prevents pushing* — sha-<branch-sha> image
- sha-<branch-sha> image — *is pushed to* — GHCR
- CD — *encounters* — not found error
- CD — *when deploying* — sha-<branch-sha> image
- not found error — *is expected for* — sha-<branch-sha> image
- cd.yml — *pulls with* — GITHUB_TOKEN
- GITHUB_TOKEN — *has* — packages: read permission
- GITHUB_TOKEN — *can read* — image
- sha-<branch-sha> image — *was never pushed* — GHCR
- not found error — *is distinct from* — transient 404
- transient 404 — *applies to* — just-built digest
- transient 404 — *is explained in* — CD deploy can transiently 404 on a just-built GHCR digest - re-run the failed job (note)
- E2E (customer360-api -> UAT) job — *tests* — live UAT
- live UAT — *is* — beta.leocdp.com/c360api
- feature branch E2E — *cannot go green from* — DB migration
- branch code — *must land on* — main branch
- main branch — *causes publication of* — images
- vX.Y.Z tag — *causes publication of* — images
- main branch — *causes* — auto-deploy UAT
- vX.Y.Z tag — *causes* — auto-deploy UAT
- build-and-push job — *publishes* — sha-<main-sha> image
- build-and-push job — *depends on* — changes job
- build-and-push job — *depends on* — test job
- build-and-push job — *is independent of* — red E2E
- CD — *is dispatched via* — cd.yml
- cd.yml — *deploys to* — UAT
- cd.yml — *uses* — sha-<main-sha> image
- auto-deploy — *requires* — CI success
- red E2E — *prevents* — CI success
- E2E (customer360-api -> UAT) job — *is re-run after* — UAT serves new code
- auto-deploy — *excludes* — monitoring
- monitoring — *is related to* — leo-customer360 push to main skips the monitoring step; deploy Portainer agents manually (note)
- Feature-branch images are never pushed to GHCR in leo-customer360 CI (note) — *is related to* — CD deploy can transiently 404 on a just-built GHCR digest - re-run the failed job (note)
- Feature-branch images are never pushed to GHCR in leo-customer360 CI (note) — *is related to* — leo-customer360 push to main skips the monitoring step; deploy Portainer agents manually (note)

%% ai-graph-end %%