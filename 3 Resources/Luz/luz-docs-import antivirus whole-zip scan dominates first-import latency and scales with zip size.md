---
ai_hash: 75bb0a53dc3ae273
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-10
entities: []
source: session 2026-08-10 (lam_transfer.zip test)
status: seedling
tags:
- luz
- luz-docs-import
- antivirus
- performance
- latency
title: 'luz-docs-import: antivirus whole-zip scan dominates first-import latency and
  scales with zip size'
type: observation
---

# luz-docs-import: antivirus whole-zip scan dominates first-import latency and scales with zip size

In luz-docs-import, the FIRST step of the import flow is a whole-zip antivirus scan (`AntiviusScanningService.scanUploadFile`, called before unzip/dedup in `DocsImportAsyncService.importZipAndCleanFile`). Its latency scales with archive size and is the single largest contributor to first-import wall-clock:

- 9 KB `transfer.zip` → `/scanner` call ≈ 0.7 s
- 3.3 MB `lam_transfer.zip` → `/scanner` call ≈ 42 s (job sat in `UPLOADED`/`SCANNING` ~28 s before `DOCUMENT_CREATING`)

Measured as luz-docs-import`s outbound `time-consuming=` on `…/luz_antivirus/api/scanner` (see [[Trace Luz per-service latency via the time-consuming= log marker]]).

**Observation (unconfirmed):** re-uploading the byte-identical zip (dedup run) showed NO comparable ~40 s scanner cost — luz-docs-import made only ~24 short calls. Antivirus may short-circuit/cache identical content. The dedup skip itself happens AFTER the scan (idempotency is checked per-file post-unzip), so a re-import is not free of scan cost in general. Related: [[Luz docs-import zip flow upload-zip returns job-id, poll GET until DONE|Luz docs-import zip flow: upload-zip returns job-id, poll GET until DONE]].

## Related

- [[Trace Luz per-service latency via the time-consuming= log marker]]
- [[Luz docs-import zip flow upload-zip returns job-id, poll GET until DONE]]

%% ai-graph-start %%

**Related notes:**
- [[luz-docs-import ZIP import timing fresh 100-doc ~40s vs deduped sub-second]]
- [[luz_docs_import ZIP uploads are not virus-scanned (AntiviusScanningService is never wired in)]]
- [[luz-docs-import ZIP import call chain]]
- [[luz-docs-import scans metadata sidecars per-file, not the whole ZIP]]
- [[Per-file AV scan rejects one file; whole-job scan fails the whole import]]

%% ai-graph-end %%