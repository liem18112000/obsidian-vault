---
ai_hash: f47ed3651cdea803
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-10
entities:
- luz-docs-import
- metadata sidecar
- ZIP
- LUZ-158230
- DocsImportAsyncService
- Antivirus Scanning Service
- importZipAndCleanFile
- processDocumentFile
- metadata
- JsonUtils
- MetadataResult
- JSON
- Virus
- Metadata sidecars must be scanned in luz-docs-import because they are never uploaded
- Per-file AV scan rejects one file; whole-job scan fails the whole import
- document
- scanUploadFile
source: session 2026-08-10, LUZ-158230
status: seedling
tags:
- luz-docs-import
- antivirus
- earchive
- LUZ-158230
title: luz-docs-import scans metadata sidecars per-file, not the whole ZIP
type: howto
---

# luz-docs-import scans metadata sidecars per-file, not the whole ZIP

As of LUZ-158230, `DocsImportAsyncService` no longer scans the uploaded ZIP as a whole. The `antiviusScanningService.scanUploadFile(zipFile, token)` call was removed from `importZipAndCleanFile`.

Instead, in `processDocumentFile`, right before a documents metadata is read, the sidecar is scanned once:

```java
File metadataFile = JsonUtils.getMetadataJsonFile(file);
if (metadataFile.isFile() && !antiviusScanningService.isFileClean(metadataFile, token)) {
    progress.recordRejected(file, "Virus found in the metadata file");
    return;
}
```

The scan happens **once, before** `MetadataResult.parseJsonMetadataFromFile` reads/parses the JSON — so nothing derived from an unscanned sidecar is ever trusted. Each document has exactly one sidecar and is processed once, so "scan once before reading" holds naturally.

Why not the whole-zip scan anymore: see [[Metadata sidecars must be scanned in luz-docs-import because they are never uploaded]]. Why a bad scan only rejects that one document: see [[Per-file AV scan rejects one file; whole-job scan fails the whole import]].

## Related

- [[Metadata sidecars must be scanned in luz-docs-import because they are never uploaded]]
- [[Per-file AV scan rejects one file; whole-job scan fails the whole import]]

%% ai-graph-start %%

**Related notes:**
- [[Per-file AV scan rejects one file; whole-job scan fails the whole import]]
- [[Metadata sidecars must be scanned in luz-docs-import because they are never uploaded]]
- [[luz-docs-import AV scan covers only the metadata sidecar, never the document binary]]
- [[luz_docs_import ZIP uploads are not virus-scanned (AntiviusScanningService is never wired in)]]
- [[luz-docs-import antivirus whole-zip scan dominates first-import latency and scales with zip size]]

**Relations:**
- luz-docs-import — *scans* — metadata sidecar
- luz-docs-import — *does not scan* — ZIP
- LUZ-158230 — *caused change in* — DocsImportAsyncService
- DocsImportAsyncService — *no longer scans* — ZIP
- scanUploadFile — *removed from* — importZipAndCleanFile
- processDocumentFile — *performs scan of* — metadata sidecar
- processDocumentFile — *scans before reading* — metadata
- JsonUtils — *provides* — metadata sidecar
- metadata sidecar — *contains* — JSON
- MetadataResult — *parses* — JSON
- Antivirus Scanning Service — *scans* — metadata sidecar
- Antivirus Scanning Service — *previously scanned* — ZIP
- Virus — *in* — metadata sidecar
- Virus — *rejects* — document
- Metadata sidecars must be scanned in luz-docs-import because they are never uploaded — *explains removal of* — ZIP scan
- Per-file AV scan rejects one file; whole-job scan fails the whole import — *explains* — per-file rejection
- document — *has* — metadata sidecar

%% ai-graph-end %%