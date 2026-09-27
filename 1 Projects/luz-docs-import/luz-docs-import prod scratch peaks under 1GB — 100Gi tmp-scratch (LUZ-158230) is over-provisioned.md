---
ai_hash: e6ef8ca73b9f108d
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-13
entities:
- luz-docs-import
- prod scratch
- tmp-scratch
- LUZ-158230
- Cloud Monitoring
- klara-prod
- apply-file-store
- PVC
- pd-standard
- kubernetes/luz-docs-import/k8s.yaml
- temp-storage
- prod
- repo
- live prod
- performance cluster
- GKE
- pod RBAC
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

- [[Read GKE PVCvolume peak usage from Cloud Monitoring when pod RBAC is denied|Read GKE PVC/volume peak usage from Cloud Monitoring when pod RBAC is denied]]

%% ai-graph-start %%

**Related notes:**
- [[luz-docs-import k8s is a StatefulSet with a 300Gi block-disk temp VCT for upload and tmp]]
- [[GKE pd-standard disk throughput is low and scales with provisioned volume size]]
- [[Luz shared Filestore has an automated cleanup cronjob with per-env subPath prefixes]]
- [[luz_docs_import upload-zip is slow for large files due to a synchronous double-write]]
- [[Luz performance env cluster topology]]

**Relations:**
- luz-docs-import — *HAS_SCRATCH_VOLUME* — prod scratch
- prod scratch — *LOCATED_ON* — klara-prod
- prod scratch — *PEAKS_AT* — ~0.78 GiB
- prod scratch — *HAS_LIVE_CAPACITY* — ~294 GiB
- prod scratch — *HAS_UTILISATION* — 0.27%
- Cloud Monitoring — *PROVIDES_TELEMETRY_FOR* — prod scratch
- LUZ-158230 — *IS_ASSOCIATED_WITH* — apply-file-store
- apply-file-store — *INTRODUCES* — tmp-scratch
- tmp-scratch — *IS_A* — PVC
- tmp-scratch — *HAS_DECLARED_STORAGE* — 100Gi
- tmp-scratch — *MOUNTED_AT* — /tmp
- tmp-scratch — *IS_OVER_PROVISIONED* — true
- pd-standard — *HAS_PROPERTY* — IOPS scale with disk size
- kubernetes/luz-docs-import/k8s.yaml — *DECLARES_REPLICAS* — 1
- kubernetes/luz-docs-import/k8s.yaml — *DECLARES* — tmp-scratch
- live prod — *RUNS_REPLICAS* — 2
- live prod — *USES_SCRATCH_VOLUME* — temp-storage
- temp-storage — *HAS_LIVE_CAPACITY* — ~294 GiB
- tmp-scratch — *IS_PENDING_DEPLOYMENT_TO* — prod
- kubernetes/luz-docs-import/k8s.yaml — *SETS_STORAGE_TO* — 16Gi
- kubernetes/luz-docs-import/k8s.yaml — *INCLUDES_COMMENT_CITING* — Cloud Monitoring peak
- GKE — *RELATED_TO* — PVC/volume peak usage
- pod RBAC — *CAN_BE* — denied
- repo — *CONTAINS* — kubernetes/luz-docs-import/k8s.yaml
- live prod — *HAS* — deployed volumes
- deployed volumes — *HAVE_CAPACITY* — ~294 GiB
- deployed volumes — *WERE_PROVISIONED_BEYOND* — repo
- live prod — *HAS_SCRATCH_VOLUME* — temp-storage
- tmp-scratch — *HAS_RIGHT_SIZED_TARGET* — 10-20Gi
- deployed volumes — *LOCATED_ON* — prod
- deployed volumes — *LOCATED_ON* — performance cluster

%% ai-graph-end %%