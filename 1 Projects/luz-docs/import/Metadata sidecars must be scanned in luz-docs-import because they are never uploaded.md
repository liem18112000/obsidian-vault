---
ai_hash: 8847651f9cea0fe7
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-10
entities:
- Metadata sidecars
- luz-docs-import
- eArchive ZIP import
- LUZ-158230
- antivirus responsibility
- Document payloads
- luz-docs view-controller
- LuzDocsViewControllerService.createDocument
- HealthDocImporter.applyMetadata
- DocsImportAsyncService.importZipAndCleanFile
- document metadata JSON
- old upfront whole-zip scan
- dedicated per-sidecar scan
- downstream view-controller
- who uploads the bytes
- luz-docs-import scans metadata sidecars per-file, not the whole ZIP
- Per-file AV scan rejects one file; whole-job scan fails the whole import
- luz-docs-import scans metadata sidecars per-file
- not the whole ZIP
source: session 2026-08-10, LUZ-158230
status: seedling
tags:
- luz-docs-import
- antivirus
- earchive
- LUZ-158230
- design-decision
title: Metadata sidecars must be scanned in luz-docs-import because they are never
  uploaded
type: argument
---

# Metadata sidecars must be scanned in luz-docs-import because they are never uploaded

In the eArchive ZIP import (LUZ-158230), the antivirus responsibility is split by *who uploads the bytes*.

- **Document payloads** (pdf/docx/images) are scanned **downstream** by the luz-docs view-controller when `LuzDocsViewControllerService.createDocument` uploads them. So this import service does **not** scan them itself.
- **Metadata sidecars** (`<doc>.metadata.json`) are **never uploaded** to the view-controller — their fields are folded locally into the document metadata JSON via `HealthDocImporter.applyMetadata`. Therefore `luz-docs-import` is the *only* place a sidecar can be scanned before its contents are trusted.

Consequence: the old upfront whole-zip scan in `DocsImportAsyncService.importZipAndCleanFile` was removed as redundant for documents, and a dedicated per-sidecar scan was added instead. If the downstream view-controller ever stops scanning on upload, document payloads would go unscanned — that assumption is load-bearing.

See [[luz-docs-import scans metadata sidecars per-file, not the whole ZIP]] and [[Per-file AV scan rejects one file; whole-job scan fails the whole import]].

## Related

- [[luz-docs-import scans metadata sidecars per-file]]
- [[not the whole ZIP]]
- [[Per-file AV scan rejects one file; whole-job scan fails the whole import]]

%% ai-graph-start %%

**Related notes:**
- [[luz-docs-import scans metadata sidecars per-file, not the whole ZIP]]
- [[luz-docs-import AV scan covers only the metadata sidecar, never the document binary]]
- [[Per-file AV scan rejects one file; whole-job scan fails the whole import]]
- [[luz_docs_import adds document metadata only in DocsImportAsyncService.createDocument()]]
- [[luz_docs_import ZIP import behavior — limits, allow-list, idempotency, AV scope]]

**Relations:**
- Metadata sidecars — *must be scanned in* — luz-docs-import
- Metadata sidecars — *are never uploaded* — 
- eArchive ZIP import — *is identified by* — LUZ-158230
- antivirus responsibility — *is split by* — who uploads the bytes
- Document payloads — *are scanned* — downstream
- Document payloads — *are scanned by* — luz-docs view-controller
- LuzDocsViewControllerService.createDocument — *uploads* — Document payloads
- luz-docs-import — *does not scan* — Document payloads
- Metadata sidecars — *are never uploaded to* — luz-docs view-controller
- Metadata sidecars — *fields are folded into* — document metadata JSON
- HealthDocImporter.applyMetadata — *folds* — Metadata sidecars
- HealthDocImporter.applyMetadata — *folds into* — document metadata JSON
- luz-docs-import — *is the only place to scan* — Metadata sidecars
- old upfront whole-zip scan — *was removed from* — DocsImportAsyncService.importZipAndCleanFile
- old upfront whole-zip scan — *was redundant for* — Document payloads
- dedicated per-sidecar scan — *was added to* — luz-docs-import
- downstream view-controller — *scans* — Document payloads
- downstream view-controller — *scans on* — upload
- Document payloads — *would go unscanned if* — downstream view-controller stops scanning
- luz-docs-import — *refers to* — luz-docs-import scans metadata sidecars per-file, not the whole ZIP
- luz-docs-import — *refers to* — Per-file AV scan rejects one file; whole-job scan fails the whole import
- luz-docs-import — *is related to* — luz-docs-import scans metadata sidecars per-file
- luz-docs-import scans metadata sidecars per-file — *is related to* — not the whole ZIP
- luz-docs-import scans metadata sidecars per-file — *is related to* — Per-file AV scan rejects one file; whole-job scan fails the whole import

%% ai-graph-end %%