---
title: "luz-docs Cloud Build deploys only on master; feature-branch builds just build+push"
created: 2026-09-09
type: lesson
status: seedling
source: "ship-to-dev 2026-09-09"
tags: [cloud-build, gke, luz-docs, deploy, gotcha, kepler-luz]
---

# luz-docs Cloud Build deploys only on master; feature-branch builds just build+push

The `luz-docs` Cloud Build (`cloudbuild.yaml`, trigger `luz-docs`) gates three of its steps on `BRANCH_NAME == "master"`: updating the `luz_kubernetes` image hash (`update_image_version.sh`), the in-build `kubectl rollout` of the `luz-docs` StatefulSet on dev, and the `dev-luz-docs-integration-test-scheduleEvent` trigger.

Consequence: a build run manually on a **feature branch** (via `gcloud builds triggers run luz-docs --branch=<feature>`) will compile the Maven project, build the Docker image, and **push** it to GAR (`europe-west6-docker.pkg.dev/klara-repo/artifact-registry-container-images/luz-docs:<sha>`), but will **NOT** deploy anything. You must roll out the StatefulSet(s) yourself afterwards (e.g. `google-skill-rollout-latest`), and remember any sibling deployment that shares the image — notably `luz-docs-batch` — needs its own rollout too.

## Related

- [[Read RESTEasy multipart parts eagerly in-request; never pass InputPart around]]
