---
title: "CD only redeploys a leo-customer360 service when its own path changes (dorny paths-filter)"
created: 2026-09-16
type: gotcha
status: seedling
source: "session 2026-09-16"
tags: [leo-cdp, cicd, github-actions, paths-filter, deploy, gotcha]
---

# CD only redeploys a leo-customer360 service when its own path changes (dorny paths-filter)

In `leo-customer360`, a change that only touches a service`s **deploy script** (e.g. `deployments/server/deploy-api.sh`) will NOT cause that service to be rebuilt/redeployed by CI/CD. CI (`.github/workflows/ci.yml`) uses `dorny/paths-filter@v3` keyed on each service`s own directory:
```
customer360-api:  customer360-api/**
backend-system:   backend-system/**
ads-server:       ads-server/**
data-tracking-api: data-tracking-api/**
frontend-admin:   frontend-admin/**
```
Only a change under `customer360-api/**` makes CI build+push `customer360-api:sha-<commit>`. CD (`cd.yml`, `workflow_run` after CI succeeds on `main`) then pulls that exact tag — so if the image was never built, the api deploy has nothing to pull.

## Consequence / fix
To force a service redeploy when your real change is outside its dir (deploy script, shared lib), add a **small non-impact change under that service`s path** (e.g. a one-line comment in a source file) in the same PR. On merge to `main` the filter selects the service, CI builds the image, CD auto-deploys uat.

## Trigger model (for context)
- CD deploys **only** on a successful CI run whose ref is `main` (-> uat, tag `sha-<commit>`) or a `vX.Y.Z` tag (-> prod). PRs / feature branches are skipped by design.
- CI itself runs on push to any branch except `paths-ignore: ui-wireframes/**, docs/**`.

Related: [[Validate S3_REGION at the deploy boundary, not after boto3 fails]]

## Related

- [[Validate S3_REGION at the deploy boundary]]
- [[not after boto3 fails]]
