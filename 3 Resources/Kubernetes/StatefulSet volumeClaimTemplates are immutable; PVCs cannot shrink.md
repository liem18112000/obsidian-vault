---
ai_hash: bd90a3ea1db40e2b
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-03
entities: []
source: session 2026-08-03
status: seedling
tags:
- kubernetes
- statefulset
- pvc
- kustomize
- immutable
- gotcha
title: StatefulSet volumeClaimTemplates are immutable; PVCs cannot shrink
type: lesson
---

# StatefulSet volumeClaimTemplates are immutable; PVCs cannot shrink

A StatefulSet's `volumeClaimTemplates` (and most spec fields except `replicas`, `template`, `updateStrategy`, `ordinals`, `persistentVolumeClaimRetentionPolicy`, `minReadySeconds`) are **immutable after creation**. So a Kustomize/kubectl patch that changes PVC storage size inside `volumeClaimTemplates` is rejected — and because the apply is atomic, it takes the rest of your patch (e.g. resource limits added in the same StatefulSet doc) down with it, silently leaving the workload un-updated.

Separately, an existing PVC's `spec.resources.requests.storage` can only be **grown, never shrunk** (`Forbidden: field can not be less than status.capacity`).

Practical rules: keep resource/limit and env patches in the `template` (mutable); do NOT try to resize volumes on an existing StatefulSet — to change volume size, recreate the StatefulSet + PVCs. PVC size is a weak "minimise resources" lever anyway; CPU/memory limits are the real one.

## Related
- [[Right-sizing k8s resource limits on the Customer360 stack]]

%% ai-graph-start %%

**Related notes:**
- [[GKE Immediate-binding StorageClass deadlocks a single-replica StatefulSet across zones]]
- [[Right-sizing k8s resource limits on the Customer360 stack]]
- [[Rollout restart uses the LIVE spec - a manifest edited only in git changes nothing]]
- [[vngcloud Terraform accepts root_disk_size change but does not resize the boot volume in-place]]
- [[luz-docs-import k8s is a StatefulSet with a 300Gi block-disk temp VCT for upload and tmp]]

%% ai-graph-end %%