---
ai_hash: 1023490663ec14a1
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
- cronjob
- retention
- gotcha
title: Luz shared Filestore has an automated cleanup cronjob with per-env subPath
  prefixes
type: observation
---

# Luz shared Filestore has an automated cleanup cronjob with per-env subPath prefixes

The shared Luz Filestore (`luz-filestore-share-pvc`) is garbage-collected by cron, so services should treat it as ephemeral temp storage, NOT durable:

- **`filestore-cleanup-cronjob`** (`luz_kubernetes/kubernetes/cronjob/luz-filestore/filestore-cleanup-cronjob.yaml`) — dedicated image, every 15 min (`0/15 * * * *`), mounts the PVC `subPathExpr: prod`, `FILESTORE_MOUNT_LOCAL_PATH=/mnt/data`.
- **`remove-expired-files-in-filestore-instance-cronjob`** (prod overlay) — busybox, weekly (`0 3 * * 6`): `find /mnt/data -type f -mtime +7 -delete`, `subPathExpr: production/`.

**Gotcha — the subPath prefix is per-env and inconsistent.** The base 15-min sweeper mounts `prod` but is JSON-patched per env: dev uses `dev` (`env-dev/cronjob/luz-filestore/patch-volume-mount-sub-path.json`), the weekly one uses `production`. Meanwhile `luz-store` writes to a FIXED `prod/luzstore` in every env. So a service`s temp files are only swept if they live under whatever prefix that env`s sweeper actually cleans — do not assume the sweeper covers your subPath. The reliable design is for the app to delete its own working dir per operation (self-cleanup), treating the cronjob as a best-effort backstop for orphans left by crashes/OOM.

Discovered planning the luz_docs_import zip-temp migration. Related: [[luz-store Filestore mount pattern shared RWX PVC + fsGroup 2000 runAsUser 1000|luz-store Filestore mount pattern: shared RWX PVC + fsGroup 2000 / runAsUser 1000]].

## Related

- [[luz-store Filestore mount pattern shared RWX PVC + fsGroup 2000 runAsUser 1000|luz-store Filestore mount pattern: shared RWX PVC + fsGroup 2000 / runAsUser 1000]]

%% ai-graph-start %%

**Related notes:**
- [[luz-store Filestore mount pattern shared RWX PVC + fsGroup 2000 runAsUser 1000]]
- [[FilestoreUtils temp-path scheme and mount config resolution]]
- [[luz-docs-import k8s is a StatefulSet with a 300Gi block-disk temp VCT for upload and tmp]]
- [[luz-docs-import prod scratch peaks under 1GB — 100Gi tmp-scratch (LUZ-158230) is over-provisioned]]
- [[FilestoreUtils mount must pre-exist; SimpleTemporaryStorage defers the fail-fast check to first use]]

%% ai-graph-end %%