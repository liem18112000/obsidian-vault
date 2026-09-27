---
ai_hash: 06a45baa663a2a2d
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-04
entities:
- luz-store
- dev
- Deployment
- StatefulSet
- kubectl set image
- google-skill-rollout-latest
- ReplicaSet-hashed pod names
- ordinal pod names
- kubectl get statefulset
- Container name
- europe-west6-docker.pkg.dev/klara-repo/artifact-registry-container-images/luz-store
- git commit SHA
- Cloud Build
- gcloud artifacts docker images list
- git rev-parse HEAD
- kubectl context
- gke_klara-nonprod_europe-west6-a_klara-nonprod
- postgres skill dev port-forward needs the klara-nonprod GKE context
- kind-customer360
- kubectl rollout status
- Image tags
source: session 2026-08-04
status: seedling
tags:
- luz_store
- kubernetes
- deployment
- rollout
- gke
- dev
title: luz-store on dev is a Deployment, not a StatefulSet — roll out with kubectl
  set image
type: howto
---

# luz-store on dev is a Deployment, not a StatefulSet — roll out with kubectl set image

On **dev**, the `luz-store` workload is a **Deployment**, not a StatefulSet. Tell by the pod names — ReplicaSet-hashed (`luz-store-66d75c9d7c-vfp7m`, `luz-store-85589bbd66-kxwtv`), not ordinal (`luz-store-0`). `kubectl get statefulset luz-store -n dev` returns `NotFound`.

## Consequence
The `google-skill-rollout-latest` skill targets **StatefulSets**, so it does NOT apply to luz-store. Roll out with a direct `kubectl set image` on the Deployment instead:

```bash
kubectl set image deployment/luz-store \
  luz-store=europe-west6-docker.pkg.dev/klara-repo/artifact-registry-container-images/luz-store:<FULL_COMMIT_SHA> \
  -n dev
kubectl rollout status deployment/luz-store -n dev --timeout=180s
```

## Details that matter
- Container name inside the pod = `luz-store` (the deployment spec has 1 container; pods show `2/2` because of an injected sidecar not in the spec).
- Image repo: `europe-west6-docker.pkg.dev/klara-repo/artifact-registry-container-images/luz-store`.
- **Image tags are the full git commit SHA** (40 hex), e.g. `8dfee39a7b7a8423bd045b89e23c354b5a221088`. Cloud Build pushes one tag per built commit. Confirm a tag exists before rolling: `gcloud artifacts docker images list <repo>/luz-store --include-tags --filter="tags:<SHA>"`.
- To roll out "the current branch image", use `git rev-parse HEAD` as the tag (not "latest" — latest may be someone else`s newer push).
- Requires kubectl context `gke_klara-nonprod_europe-west6-a_klara-nonprod` (see [[postgres skill dev port-forward needs the klara-nonprod GKE context, not kind-customer360]]).

## Related

- [[postgres skill dev port-forward needs the klara-nonprod GKE context, not kind-customer360]]

%% ai-graph-start %%

**Related notes:**
- [[luz-person is a Deployment not a StatefulSet in klara dev]]
- [[Build and roll out luz-jsonstore to dev (Cloud Build trigger + Deployment rollout)]]
- [[Shipping luz_docs_statistic trigger is docs-statistic-service and dev runs a Deployment, not a StatefulSet]]
- [[rollout-latest skill auto-detects StatefulSet vs Deployment]]
- [[postgres skill dev port-forward needs the klara-nonprod GKE context, not kind-customer360]]

**Relations:**
- luz-store — *is_on_environment* — dev
- luz-store — *is_a* — Deployment
- luz-store — *is_not_a* — StatefulSet
- Deployment — *uses_pod_naming_convention* — ReplicaSet-hashed pod names
- StatefulSet — *uses_pod_naming_convention* — ordinal pod names
- kubectl get statefulset — *returns_NotFound_for* — luz-store
- google-skill-rollout-latest — *targets* — StatefulSet
- google-skill-rollout-latest — *does_not_apply_to* — luz-store
- luz-store — *should_be_rolled_out_with* — kubectl set image
- luz-store — *has_container_name* — luz-store
- luz-store — *uses_image_repo* — europe-west6-docker.pkg.dev/klara-repo/artifact-registry-container-images/luz-store
- Image tags — *are_defined_as* — git commit SHA
- Cloud Build — *pushes* — Image tags
- gcloud artifacts docker images list — *confirms_existence_of* — Image tags
- git rev-parse HEAD — *provides_value_for* — Image tags
- kubectl set image — *requires_context* — gke_klara-nonprod_europe-west6-a_klara-nonprod
- kubectl rollout status — *requires_context* — gke_klara-nonprod_europe-west6-a_klara-nonprod
- postgres skill dev port-forward needs the klara-nonprod GKE context — *is_related_to* — gke_klara-nonprod_europe-west6-a_klara-nonprod
- postgres skill dev port-forward needs the klara-nonprod GKE context — *contrasts_with* — kind-customer360

%% ai-graph-end %%