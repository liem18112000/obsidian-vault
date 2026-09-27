---
title: "Feature-branch images are never pushed to GHCR in leo-customer360 CI"
created: 2026-09-14
type: lesson
status: seedling
source: "session 2026-09-14, PR #66"
tags: [leo-customer360, ci-cd, github-actions, ghcr, e2e, gotcha, SCRUM-92]
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
