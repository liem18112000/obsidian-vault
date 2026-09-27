---
ai_hash: f5e79c562d1b8247
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-11
entities: []
source: session 2026-08-11 luz_docs_import apply-file-store
status: seedling
tags:
- kubernetes
- filestore
- luz
- nfs
- securitycontext
- reference
title: 'luz-store Filestore mount pattern: shared RWX PVC + fsGroup 2000 / runAsUser
  1000'
type: howto
---

# luz-store Filestore mount pattern: shared RWX PVC + fsGroup 2000 / runAsUser 1000

The reference way a Luz service consumes the shared Google Filestore (for `FilestoreUtils`/`SimpleTemporaryStorage`) is `luz-store` (`luz_kubernetes/kubernetes/luz-store/k8s.yaml`):

```yaml
spec:
  securityContext:                 # pod-level
    fsGroup: 2000
    fsGroupChangePolicy: "OnRootMismatch"
  containers:
  - name: luz-store
    env:
      - name: FILESTORE_MOUNT_LOCAL_PATH
        value: /mnt/individual
    volumeMounts:
    - mountPath: /mnt/individual
      name: luz-store-share-pvc
      subPathExpr: prod/luzstore   # each service gets its own subpath under the shared PVC
    securityContext:               # container-level
      runAsUser: 1000
  volumes:
  - name: luz-store-share-pvc
    persistentVolumeClaim:
      claimName: luz-filestore-share-pvc   # shared, ReadWriteMany, Filestore CSI (NFS v3)
```

**Why the securityContext matters:** `FilestoreUtils` sets POSIX `GROUP_WRITE` on the dirs/files it creates. That is only useful if the volume is group-owned by the same GID the process runs in — hence pod `fsGroup: 2000` (kubelet group-owns the mount) + container `runAsUser: 1000`. Omit it and a container running as a different user/root gives inconsistent ownership on the shared NFS. A service adding Filestore for the first time MUST add this securityContext (test carefully — it changes the container from root to uid 1000; the shared WildFly base image supports it, as luz-store proves).

The PVC `luz-filestore-share-pvc` is provisioned per-env in `kubernetes-overlays/env-<env>/luz-filestore/filestore-volume.yaml` (ReadWriteMany, `filestore.csi.storage.gke.io`, `nolock,nfsvers=3,tcp`, Retain; nonprod instance `luz-filestore-shared-temp-file-nonprod`). Related: [[Luz shared Filestore has an automated cleanup cronjob with per-env subPath prefixes]], [[FilestoreUtils temp-path scheme and mount config resolution]].

## Related

- [[FilestoreUtils temp-path scheme and mount config resolution]]
- [[Luz shared Filestore has an automated cleanup cronjob with per-env subPath prefixes]]

%% ai-graph-start %%

**Related notes:**
- [[Luz shared Filestore has an automated cleanup cronjob with per-env subPath prefixes]]
- [[FilestoreUtils temp-path scheme and mount config resolution]]
- [[luz-docs-import k8s is a StatefulSet with a 300Gi block-disk temp VCT for upload and tmp]]
- [[FilestoreUtils mount must pre-exist; SimpleTemporaryStorage defers the fail-fast check to first use]]
- [[Run local luz-jsonstore against a real tenant GKE Mongo via port-forwards]]

%% ai-graph-end %%