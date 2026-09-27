---
ai_hash: 5ecd4018f2be351e
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-09
entities:
- luz-docs
- Cloud Build
- master branch
- feature branch
- cloudbuild.yaml
- luz-docs trigger
- BRANCH_NAME
- luz_kubernetes image hash
- update_image_version.sh
- kubectl rollout
- luz-docs StatefulSet
- dev environment
- dev-luz-docs-integration-test-scheduleEvent trigger
- Maven project
- Docker image
- GAR
- gcloud builds triggers run
- google-skill-rollout-latest
- luz-docs-batch
- RESTEasy multipart parts
- InputPart
- StatefulSet
- deployment
source: ship-to-dev 2026-09-09
status: seedling
tags:
- cloud-build
- gke
- luz-docs
- deploy
- gotcha
- kepler-luz
title: luz-docs Cloud Build deploys only on master; feature-branch builds just build+push
type: lesson
---

# luz-docs Cloud Build deploys only on master; feature-branch builds just build+push

The `luz-docs` Cloud Build (`cloudbuild.yaml`, trigger `luz-docs`) gates three of its steps on `BRANCH_NAME == "master"`: updating the `luz_kubernetes` image hash (`update_image_version.sh`), the in-build `kubectl rollout` of the `luz-docs` StatefulSet on dev, and the `dev-luz-docs-integration-test-scheduleEvent` trigger.

Consequence: a build run manually on a **feature branch** (via `gcloud builds triggers run luz-docs --branch=<feature>`) will compile the Maven project, build the Docker image, and **push** it to GAR (`europe-west6-docker.pkg.dev/klara-repo/artifact-registry-container-images/luz-docs:<sha>`), but will **NOT** deploy anything. You must roll out the StatefulSet(s) yourself afterwards (e.g. `google-skill-rollout-latest`), and remember any sibling deployment that shares the image — notably `luz-docs-batch` — needs its own rollout too.

## Related

- [[Read RESTEasy multipart parts eagerly in-request; never pass InputPart around]]

%% ai-graph-start %%

**Related notes:**
- [[luz-docs Cloud Build pushes an image for every branch but only master updates luz_kubernetes]]
- [[Shipping luz_docs_statistic trigger is docs-statistic-service and dev runs a Deployment, not a StatefulSet]]
- [[Build and roll out luz-jsonstore to dev (Cloud Build trigger + Deployment rollout)]]
- [[Klara Cloud Build pushes images to klara-repo Artifact Registry with the SA on the trigger]]
- [[luz-store on dev is a Deployment, not a StatefulSet — roll out with kubectl set image]]

**Relations:**
- luz-docs — *uses* — Cloud Build
- Cloud Build — *deploys only on* — master branch
- Cloud Build — *builds and pushes on* — feature branch
- luz-docs — *configured by* — cloudbuild.yaml
- cloudbuild.yaml — *defines trigger* — luz-docs trigger
- Cloud Build — *gates steps on* — BRANCH_NAME
- BRANCH_NAME — *condition for deployment* — master
- deployment steps — *update* — luz_kubernetes image hash
- luz_kubernetes image hash — *updated by script* — update_image_version.sh
- deployment steps — *perform* — kubectl rollout
- kubectl rollout — *targets* — luz-docs StatefulSet
- luz-docs StatefulSet — *deployed to* — dev environment
- deployment steps — *trigger* — dev-luz-docs-integration-test-scheduleEvent trigger
- feature branch build — *compiles* — Maven project
- feature branch build — *builds* — Docker image
- feature branch build — *pushes* — Docker image
- Docker image — *pushed to* — GAR
- GAR — *path* — europe-west6-docker.pkg.dev/klara-repo/artifact-registry-container-images/luz-docs:<sha>
- feature branch build — *does not deploy* — luz-docs StatefulSet
- gcloud builds triggers run — *initiates* — feature branch build
- manual rollout — *required for* — StatefulSet
- manual rollout — *can use* — google-skill-rollout-latest
- luz-docs-batch — *shares image with* — luz-docs
- luz-docs-batch — *requires* — manual rollout
- luz-docs — *is a* — StatefulSet
- luz-docs-batch — *is a* — deployment
- luz-docs — *is related to* — Read RESTEasy multipart parts eagerly in-request; never pass InputPart around
- Read RESTEasy multipart parts eagerly in-request; never pass InputPart around — *involves* — RESTEasy multipart parts
- Read RESTEasy multipart parts eagerly in-request; never pass InputPart around — *mentions* — InputPart

%% ai-graph-end %%