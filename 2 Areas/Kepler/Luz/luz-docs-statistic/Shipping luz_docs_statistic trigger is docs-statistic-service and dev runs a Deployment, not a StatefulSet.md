---
ai_hash: 388223ce97410e2a
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-06-11
entities:
- luz_docs_statistic
- docs-statistic-service
- Deployment
- StatefulSet
- Cloud Build
- luz-skill-ship
- gcloud builds triggers run
- google-skill-rollout-latest
- kubectl
- ship.sh
- dev cluster
- PubSub
- EJB timer
- $facet aggregation
- luz-docs-statistic
- europe-west6-docker.pkg.dev/klara-repo/artifact-registry-container-images
- commit-sha
- image tags
- TRIGGER_NAME
source: ship of LUZ-155460, session 2026-06-11
status: seedling
tags:
- luz
- luz-docs-statistic
- cloud-build
- kubernetes
- deploy
title: 'Shipping luz_docs_statistic: trigger is docs-statistic-service and dev runs
  a Deployment, not a StatefulSet'
type: howto
---

# Shipping luz_docs_statistic: trigger is docs-statistic-service and dev runs a Deployment, not a StatefulSet

Two facts that broke the standard luz-skill-ship flow for the luz_docs_statistic repo (discovered 2026-06-11):

1. The Cloud Build trigger is named **`docs-statistic-service`** — NOT `luz-docs-statistic` (the repo/image/workload name). Guessing the image name yields `NOT_FOUND` from `gcloud builds triggers run` for all retries.
2. In the dev cluster the workload is a **Deployment** `deployment/luz-docs-statistic` (namespace `dev`), not a StatefulSet — so `google-skill-rollout-latest` (StatefulSet-only) fails with NotFound after a successful build. Roll out manually:
   `kubectl set image deployment/luz-docs-statistic -n dev luz-docs-statistic=europe-west6-docker.pkg.dev/klara-repo/artifact-registry-container-images/luz-docs-statistic:<commit-sha>` then `kubectl rollout status`.

Working recipe: stage → `TRIGGER_NAME=docs-statistic-service ship.sh "<msg>"` (ship.sh is safe to re-run; it skips commit/push when already done) → expect the rollout step to fail → kubectl set image as above. Image tags are full commit SHAs.

Related: [[luz_docs_statistic updates stats via 1-minute EJB timer over PubSub and $facet aggregation]]

## Related

- [[luz_docs_statistic updates stats via 1-minute EJB timer over PubSub and $facet aggregation]]

%% ai-graph-start %%

**Related notes:**
- [[luz-store on dev is a Deployment, not a StatefulSet — roll out with kubectl set image]]
- [[Build and roll out luz-jsonstore to dev (Cloud Build trigger + Deployment rollout)]]
- [[luz-docs Cloud Build deploys only on master; feature-branch builds just build+push]]
- [[luz-docs Cloud Build pushes an image for every branch but only master updates luz_kubernetes]]
- [[luz-docs-it-staging-trigger-poller-mismatch]]

**Relations:**
- luz_docs_statistic — *is a* — repo
- luz_docs_statistic — *has Cloud Build trigger* — docs-statistic-service
- docs-statistic-service — *is a* — Cloud Build trigger
- luz-docs-statistic — *is a* — workload name
- luz-docs-statistic — *is an* — image name
- luz-docs-statistic — *runs as* — Deployment
- luz-docs-statistic — *runs in* — dev cluster
- luz-docs-statistic — *does not run as* — StatefulSet
- luz_docs_statistic — *broke* — luz-skill-ship
- gcloud builds triggers run — *expects image name* — luz-docs-statistic
- gcloud builds triggers run — *yields NOT_FOUND for* — luz-docs-statistic
- google-skill-rollout-latest — *requires* — StatefulSet
- google-skill-rollout-latest — *fails for* — Deployment
- kubectl — *performs* — set image
- kubectl — *performs* — rollout status
- ship.sh — *uses variable* — TRIGGER_NAME
- luz_docs_statistic — *updates stats via* — EJB timer
- luz_docs_statistic — *updates stats via* — PubSub
- luz_docs_statistic — *updates stats via* — $facet aggregation
- luz-docs-statistic — *image is located at* — europe-west6-docker.pkg.dev/klara-repo/artifact-registry-container-images
- image tags — *are* — commit-sha

%% ai-graph-end %%