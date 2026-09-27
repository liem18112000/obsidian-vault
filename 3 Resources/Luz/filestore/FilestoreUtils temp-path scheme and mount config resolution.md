---
ai_hash: 8ebbb5eb7f4b6580
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-11
entities: []
source: session 2026-08-11 luz_docs_import apply-file-store
status: seedling
tags:
- filestore
- luz
- java
- gke
- nfs
- reference
title: FilestoreUtils temp-path scheme and mount config resolution
type: concept
---

# FilestoreUtils temp-path scheme and mount config resolution

`FilestoreUtils` (`ch.klara.luz:filestore_utils`, Java 17, JDK-only) writes each file to a generated, collision-free path:

```
{mount}/{yyyy_MM_ddTHH_mm}/{uuid}/{fileName}
```

- **Minute-resolution timestamp + random UUID** → human-browsable date grouping plus stateless uniqueness (no locking/coordination); safe for concurrent multi-pod writes.
- Sets POSIX **`GROUP_WRITE`** on created dirs/files so multiple containers sharing the Filestore PVC can read/write each other`s files (plain `0700` would block cross-container access).
- **Mount resolution order:** env `FILESTORE_MOUNT_LOCAL_PATH` -> sysprop `filestore.mount.local.path` -> MicroProfile Config (skipped gracefully if absent) -> default `/uploads`.
- API: `writeToFile(InputStream, fileName)` and `unzip(ZipInputStream, dest)`, both returning a `Path` and throwing `FileSystemException`.

`SimpleTemporaryStorage` (in `luz_store`) is a thin static wrapper: `writeFile(is, name)` -> `FS.writeToFile(...)` re-wrapping errors as `TemporaryStorageException`, plus `readFile(Path, opts)`. `DistributedTemporaryStorage` layers a `DualCache` `(token,tenantId,fileName)->absolutePath` on top for write-here / read-later-on-another-pod flows.

Note: `unzip` uses `java.util.zip.ZipInputStream` — it has **no** encryption/zip-bomb/explicit-UTF-8 handling, so services that need those (e.g. `luz_docs_import`, which uses zip4j) should keep their own extractor and use FilestoreUtils only for the *write*.

## Related

- [[FilestoreUtils mount must pre-exist; SimpleTemporaryStorage defers the fail-fast check to first use]]

%% ai-graph-start %%

**Related notes:**
- [[FilestoreUtils mount must pre-exist; SimpleTemporaryStorage defers the fail-fast check to first use]]
- [[luz-store Filestore mount pattern shared RWX PVC + fsGroup 2000 runAsUser 1000]]
- [[luz-docs-import k8s is a StatefulSet with a 300Gi block-disk temp VCT for upload and tmp]]
- [[Luz shared Filestore has an automated cleanup cronjob with per-env subPath prefixes]]
- [[RESTEasy MultipartFormDataInput buffers the whole upload to tmp before the app reads it]]

%% ai-graph-end %%