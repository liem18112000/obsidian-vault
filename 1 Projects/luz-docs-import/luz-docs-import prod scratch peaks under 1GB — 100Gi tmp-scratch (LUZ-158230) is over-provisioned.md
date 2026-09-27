---
ai_hash: 7fb749868eef1f2b
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-13
entities:
- luz-docs-import
- prod scratch volume
- 1GB
- 100Gi tmp-scratch
- LUZ-158230
- Cloud Monitoring
- klara-prod
- 0.78 GiB
- 800 MB
- pod-0
- pod-1
- 294 GiB
- 0.27% utilisation
- Sub-gigabyte scratch
- apply-file-store branch
- PVC
- /tmp
- 125x observed prod peak
- 10-20Gi
- pd-standard
- IOPS
- disk size
- bulk imports
- temp I/O
- file-store rollout
- '2026-08-13'
- 16Gi
- kubernetes/luz-docs-import/k8s.yaml
- inline comment
- repo
- live prod
- 'replicas: 1'
- 2 replicas
- namespace prod
- temp-storage
- tmp-scratch/100Gi block
- performance cluster
- Read GKE PVC/volume peak usage from Cloud Monitoring when pod RBAC is denied
source: session 2026-08-13
status: seedling
tags:
- luz-docs-import
- gke
- LUZ-158230
- sizing
- kubernetes
- prod
title: luz-docs-import prod scratch peaks under 1GB — 100Gi tmp-scratch (LUZ-158230)
  is over-provisioned
type: observation
---

# luz-docs-import prod scratch peaks under 1GB — 100Gi tmp-scratch (LUZ-158230) is over-provisioned

Cloud Monitoring telemetry over 42 days shows the `luz-docs-import` scratch volume on **klara-prod** peaks at **~0.78 GiB (~800 MB)** used on pod-0 (pod-1 ~0), against a live capacity of ~294 GiB — i.e. **0.27%** utilisation. Sub-gigabyte scratch is the real steady-state.

**Implication for LUZ-158230 (`apply-file-store` branch):** the branch introduces a `tmp-scratch` PVC of `storage: 100Gi` at `/tmp`. That is ~125x the observed prod peak and is unjustified by usage — it is inherited-generous headroom, not a measured requirement. A right-sized target is ~10-20Gi (still >10x peak), keeping some margin because on `pd-standard` IOPS scale with disk size and bulk imports lean on temp I/O. Validate again after the file-store rollout, since re-roling `/tmp` could shift the profile.

**Decision applied (2026-08-13):** set `storage: 16Gi` in `kubernetes/luz-docs-import/k8s.yaml` (>20x the ~0.8Gi peak), with an inline comment citing the Cloud Monitoring peak so the value carries its own justification.

**Drift facts found (repo vs live):**
- Repo `kubernetes/luz-docs-import/k8s.yaml` declares `replicas: 1` and a `tmp-scratch` 100Gi PVC. LIVE prod runs **2 replicas** in namespace `prod` and the scratch volume is still named **`temp-storage`** (the old manifest) — so the `tmp-scratch`/100Gi block is NOT deployed to prod yet; it is pending on this branch.
- Live capacity reads **~294 GiB** on BOTH prod and the performance cluster, despite the manifest saying 100Gi — the deployed volumes were provisioned/expanded well beyond the repo value. Do not trust the repo size as deployed reality.

## Related

- [[Read GKE PVC/volume peak usage from Cloud Monitoring when pod RBAC is denied]]

%% ai-graph-start %%

**Related notes:**
- [[luz-docs-import k8s is a StatefulSet with a 300Gi block-disk temp VCT for upload and tmp]]
- [[GKE pd-standard disk throughput is low and scales with provisioned volume size]]
- [[Luz shared Filestore has an automated cleanup cronjob with per-env subPath prefixes]]
- [[luz_docs_import upload-zip is slow for large files due to a synchronous double-write]]
- [[Luz performance env cluster topology]]

**Relations:**
- luz-docs-import — *has_component* — prod scratch volume
- prod scratch volume — *peaks_under* — 1GB
- 100Gi tmp-scratch — *is_associated_with* — LUZ-158230
- 100Gi tmp-scratch — *is* — over-provisioned
- Cloud Monitoring — *provides_telemetry_for* — prod scratch volume
- prod scratch volume — *located_on* — klara-prod
- prod scratch volume — *peaks_at* — 0.78 GiB
- 0.78 GiB — *is_approximately* — 800 MB
- 0.78 GiB — *used_by* — pod-0
- prod scratch volume — *has_live_capacity* — 294 GiB
- prod scratch volume — *has_utilisation* — 0.27% utilisation
- Sub-gigabyte scratch — *is* — steady-state
- LUZ-158230 — *involves* — apply-file-store branch
- apply-file-store branch — *introduces* — tmp-scratch PVC
- tmp-scratch PVC — *has_storage* — 100Gi
- tmp-scratch PVC — *mounted_at* — /tmp
- 100Gi — *is* — 125x observed prod peak
- 100Gi — *is* — unjustified by usage
- Right-sized target — *is* — 10-20Gi
- IOPS — *scales_with* — disk size
- disk size — *is_relevant_for* — pd-standard
- bulk imports — *rely_on* — temp I/O
- Decision applied — *on* — 2026-08-13
- Decision — *sets_storage_to* — 16Gi
- 16Gi — *specified_in* — kubernetes/luz-docs-import/k8s.yaml
- 16Gi — *is* — >20x the ~0.8Gi peak
- kubernetes/luz-docs-import/k8s.yaml — *declares* — replicas: 1
- kubernetes/luz-docs-import/k8s.yaml — *declares* — tmp-scratch 100Gi PVC
- live prod — *runs* — 2 replicas
- live prod — *is_in* — namespace prod
- scratch volume — *named* — temp-storage
- tmp-scratch/100Gi block — *is_not_deployed_to* — prod
- tmp-scratch/100Gi block — *is* — pending on this branch
- Live capacity — *is* — 294 GiB on prod
- Live capacity — *is* — 294 GiB on performance cluster
- manifest — *specifies* — 100Gi
- deployed volumes — *exceed* — repo value
- this note — *is_related_to* — Read GKE PVC/volume peak usage from Cloud Monitoring when pod RBAC is denied

%% ai-graph-end %%