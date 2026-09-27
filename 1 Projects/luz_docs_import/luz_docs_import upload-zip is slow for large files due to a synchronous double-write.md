---
ai_hash: e2b3cb190d8dbd43
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-12
entities:
- luz_docs_import
- upload-zip
- large files
- synchronous double-write
- POST .../import-jobs/upload-zip
- ZIPs > 100 MB
- import work
- virus scan
- unzip
- folder/document creation
- '@Asynchronous EJB'
- DocsImportAsyncService.importZipAndCleanFile
- synchronous ingest
- request thread
- resteasy-multipart
- /tmp
- DocsImportService.java:112
- saveReferenceFileToTemp
- /luz_docs_import/upload/...
- DocsImportService.java:129
- per-pod pd-standard 300 GiB VCT
- throughput
- disk I/O
- network-transfer floor
- Validation
- ZIP central directory
- single-pass write
- in-flight Filestore temp-storage plan
- NFS
- ingress
- 'proxy-request-buffering: off'
- long-term pre-signed direct-to-GCS upload
- GCS
- docs/large-zip-upload-latency-investigation.md
- RESTEasy MultipartFormDataInput
- GKE pd-standard disk
- pre-signed direct-to-object-storage uploads
- nginx-ingress
source: docs/large-zip-upload-latency-investigation.md 2026-08-12
status: seedling
tags:
- luz-docs-import
- performance
- upload
- LUZ-158230
title: luz_docs_import upload-zip is slow for large files due to a synchronous double-write
type: observation
---

# luz_docs_import upload-zip is slow for large files due to a synchronous double-write

In `luz_docs_import`, `POST .../import-jobs/upload-zip` takes minutes to respond for ZIPs > 100 MB — but **not** because of import work. The import (virus scan, unzip, folder/document creation) is genuinely offloaded to an `@Asynchronous` EJB (`DocsImportAsyncService.importZipAndCleanFile`), so the slow part is the **synchronous ingest** on the request thread.

**Root cause:** the ZIP is written to disk **twice** before the 200 — resteasy-multipart buffers it to `/tmp` (`DocsImportService.java:112`), then `saveReferenceFileToTemp` copies it again to `/luz_docs_import/upload/...` (`:129`). Both are subPaths of the **same per-pod `pd-standard` 300 GiB VCT**, whose throughput is low — so ~1 GB of ZIP means ~a minute of serialized disk I/O plus the network-transfer floor. Validation only reads the ZIP central directory, so it is not the cost.

**Fix direction:** single-pass write (fold into the in-flight Filestore temp-storage plan, which currently keeps the double-write and moves the 2nd write onto slower NFS); ingress `proxy-request-buffering: off`; long-term pre-signed direct-to-GCS upload. Full write-up: `docs/large-zip-upload-latency-investigation.md` (docs/ is gitignored).

Related: [[RESTEasy MultipartFormDataInput buffers the whole upload to tmp before the app reads it]], [[GKE pd-standard disk throughput is low and scales with provisioned volume size]], [[Decouple upload API latency from file size with pre-signed direct-to-object-storage uploads]], [[nginx-ingress proxy-request-buffering stages the whole request body before forwarding to the pod]].

## Related

- [[RESTEasy MultipartFormDataInput buffers the whole upload to tmp before the app reads it]]
- [[GKE pd-standard disk throughput is low and scales with provisioned volume size]]
- [[Decouple upload API latency from file size with pre-signed direct-to-object-storage uploads]]
- [[nginx-ingress proxy-request-buffering stages the whole request body before forwarding to the pod]]

%% ai-graph-start %%

**Related notes:**
- [[RESTEasy MultipartFormDataInput buffers the whole upload to tmp before the app reads it]]
- [[Decouple upload API latency from file size with pre-signed direct-to-object-storage uploads]]
- [[nginx-ingress proxy-request-buffering stages the whole request body before forwarding to the pod]]
- [[GKE pd-standard disk throughput is low and scales with provisioned volume size]]
- [[luz-docs-import cold first-import slowness is JIT plus downstream re-warm on a CPU-limited pod]]

**Relations:**
- luz_docs_import upload-zip — *is slow for* — large files
- luz_docs_import upload-zip — *is slow due to* — synchronous double-write
- POST .../import-jobs/upload-zip — *takes minutes to respond for* — ZIPs > 100 MB
- slow response — *is not due to* — import work
- import work — *is offloaded to* — DocsImportAsyncService.importZipAndCleanFile
- DocsImportAsyncService.importZipAndCleanFile — *is an* — @Asynchronous EJB
- import work — *includes* — virus scan
- import work — *includes* — unzip
- import work — *includes* — folder/document creation
- slow part — *is* — synchronous ingest
- synchronous ingest — *occurs on* — request thread
- synchronous double-write — *is root cause of* — slow part
- ZIP — *is written to disk* — twice
- resteasy-multipart — *buffers ZIP to* — /tmp
- /tmp — *at* — DocsImportService.java:112
- saveReferenceFileToTemp — *copies ZIP to* — /luz_docs_import/upload/...
- /luz_docs_import/upload/... — *at* — DocsImportService.java:129
- /tmp — *is subPath of* — per-pod pd-standard 300 GiB VCT
- /luz_docs_import/upload/... — *is subPath of* — per-pod pd-standard 300 GiB VCT
- per-pod pd-standard 300 GiB VCT — *has* — low throughput
- low throughput — *causes* — ~a minute of serialized disk I/O
- Validation — *reads* — ZIP central directory
- Fix direction — *is* — single-pass write
- single-pass write — *should fold into* — in-flight Filestore temp-storage plan
- in-flight Filestore temp-storage plan — *moves 2nd write onto* — NFS
- Fix direction — *includes* — ingress proxy-request-buffering: off
- Fix direction — *includes* — long-term pre-signed direct-to-GCS upload
- long-term pre-signed direct-to-GCS upload — *uses* — GCS
- Full write-up — *is in* — docs/large-zip-upload-latency-investigation.md
- RESTEasy MultipartFormDataInput — *buffers* — whole upload to tmp
- GKE pd-standard disk — *has* — low throughput
- pre-signed direct-to-object-storage uploads — *decouples latency from* — file size
- nginx-ingress — *stages whole request body before forwarding* — to the pod

%% ai-graph-end %%