---
ai_hash: 9d2d97555f1aef7e
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-25
entities:
- luz-jsonstore
- dev
- Cloud Build trigger
- Deployment rollout
- jsonstore-service
- klara-infra
- europe-west6
- cloudbuild.yaml
- gcloud builds triggers run
- gcloud builds describe
- Artifact Registry
- Docker image
- git commit SHA
- kubectl
- kubectl set image
- kubectl rollout status
- kubectl rollout restart
- Kubernetes Deployment
- Kubernetes StatefulSet
- google-skill-rollout-latest
- luz-skill-ship
- api-forwarder gateway
- Kubernetes service
- kubectl port-forward
- Run local luz-jsonstore against a real tenant GKE Mongo via port-forwards
source: session 2026-08-25
status: seedling
tags:
- luz-jsonstore
- cloud-build
- gke
- rollout
- dev
title: Build and roll out luz-jsonstore to dev (Cloud Build trigger + Deployment rollout)
type: howto
---

# Build and roll out luz-jsonstore to dev (Cloud Build trigger + Deployment rollout)

luz-jsonstore ships to dev in TWO steps; the build does NOT deploy.

1. Build: run the Cloud Build trigger `jsonstore-service` (project klara-infra, region europe-west6, buildfile cloudbuild.yaml):
   gcloud builds triggers run jsonstore-service --project=klara-infra --region=europe-west6 --branch=<branch>   (or --sha=<sha>)
   It builds + pushes an image tagged with the FULL git commit SHA to:
   europe-west6-docker.pkg.dev/klara-repo/artifact-registry-container-images/luz-jsonstore:<sha>
   cloudbuild.yaml has no kubectl/rollout step, so nothing deploys yet.
   Poll: gcloud builds describe <build-id> --project=klara-infra --region=europe-west6 --format='value(status)' until SUCCESS.

2. Rollout: the dev workload is a DEPLOYMENT named luz-jsonstore (container luz-jsonstore) in namespace dev - NOT a StatefulSet. So the StatefulSet-oriented skills (google-skill-rollout-latest, luz-skill-ship) do not fit the rollout. Use:
   kubectl set image deployment/luz-jsonstore luz-jsonstore=<registry>/luz-jsonstore:<sha> -n dev
   kubectl rollout status deployment/luz-jsonstore -n dev
   (If the tag is reused, kubectl rollout restart deployment/luz-jsonstore -n dev re-pulls instead.)

The dev app is reached through the api-forwarder gateway: http://<forward>/luz_jsonstore/api/... (kubectl port-forward -n dev services/api-forwarder 8080:8080).

## Related

- [[Run local luz-jsonstore against a real tenant GKE Mongo via port-forwards]]

%% ai-graph-start %%

**Related notes:**
- [[luz-store on dev is a Deployment, not a StatefulSet — roll out with kubectl set image]]
- [[Shipping luz_docs_statistic trigger is docs-statistic-service and dev runs a Deployment, not a StatefulSet]]
- [[luz-docs Cloud Build deploys only on master; feature-branch builds just build+push]]
- [[luz-docs Cloud Build pushes an image for every branch but only master updates luz_kubernetes]]
- [[Run local luz-jsonstore against a real tenant GKE Mongo via port-forwards]]

**Relations:**
- luz-jsonstore — *deploys to* — dev
- luz-jsonstore — *is built by* — Cloud Build trigger
- Cloud Build trigger — *is named* — jsonstore-service
- jsonstore-service — *is in project* — klara-infra
- jsonstore-service — *is in region* — europe-west6
- jsonstore-service — *uses build file* — cloudbuild.yaml
- gcloud builds triggers run — *executes* — jsonstore-service
- Cloud Build trigger — *produces* — Docker image
- Docker image — *is tagged with* — git commit SHA
- Docker image — *is stored in* — Artifact Registry
- Artifact Registry — *hosts* — luz-jsonstore
- cloudbuild.yaml — *lacks* — kubectl/rollout step
- gcloud builds describe — *monitors* — Cloud Build trigger
- dev — *workload is a* — Kubernetes Deployment
- Kubernetes Deployment — *is named* — luz-jsonstore
- Kubernetes Deployment — *is in namespace* — dev
- Kubernetes Deployment — *manages container* — luz-jsonstore
- Kubernetes Deployment — *is not a* — Kubernetes StatefulSet
- google-skill-rollout-latest — *is unsuitable for* — Deployment rollout
- luz-skill-ship — *is unsuitable for* — Deployment rollout
- kubectl set image — *updates* — Kubernetes Deployment
- kubectl rollout status — *checks status of* — Kubernetes Deployment
- kubectl rollout restart — *restarts* — Kubernetes Deployment
- luz-jsonstore — *is accessed via* — api-forwarder gateway
- api-forwarder gateway — *is a* — Kubernetes service
- kubectl port-forward — *accesses* — Kubernetes service
- luz-jsonstore — *has related topic* — Run local luz-jsonstore against a real tenant GKE Mongo via port-forwards

%% ai-graph-end %%