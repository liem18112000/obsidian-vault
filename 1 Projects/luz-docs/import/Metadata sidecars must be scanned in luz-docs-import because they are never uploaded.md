---
ai_hash: 60063c18b5a853c6
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-10
entities:
- Metadata sidecars
- luz-docs-import
- Document payloads
- eArchive ZIP import
- antivirus responsibility
- luz-docs view-controller
- LuzDocsViewControllerService.createDocument
- HealthDocImporter.applyMetadata
- DocsImportAsyncService.importZipAndCleanFile
- LUZ-158230
- document metadata JSON
- old upfront whole-zip scan
- dedicated per-sidecar scan
- luz-docs-import scans metadata sidecars per-file, not the whole ZIP
- Per-file AV scan rejects one file; whole-job scan fails the whole import
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

- [[luz-docs-import scans metadata sidecars per-file, not the whole ZIP]]
- [[Per-file AV scan rejects one file; whole-job scan fails the whole import]]

%% ai-graph-start %%

**Related notes:**
- [[luz-docs-import scans metadata sidecars per-file, not the whole ZIP]]
- [[luz-docs-import AV scan covers only the metadata sidecar, never the document binary]]
- [[Per-file AV scan rejects one file; whole-job scan fails the whole import]]
- [[luz_docs_import adds document metadata only in DocsImportAsyncService.createDocument()]]
- [[luz-docs-import antivirus whole-zip scan dominates first-import latency and scales with zip size]]

**Relations:**
- Metadata sidecars — *must be scanned in* — luz-docs-import
- Metadata sidecars — *are never uploaded* — 
- eArchive ZIP import — *has ID* — LUZ-158230
- antivirus responsibility — *is split by* — who uploads the bytes
- Document payloads — *are scanned downstream by* — luz-docs view-controller
- LuzDocsViewControllerService.createDocument — *uploads* — Document payloads
- luz-docs-import — *does not scan* — Document payloads
- Metadata sidecars — *are never uploaded to* — luz-docs view-controller
- Metadata sidecars — *fields are folded into* — document metadata JSON
- HealthDocImporter.applyMetadata — *folds fields of* — Metadata sidecars
- luz-docs-import — *is the only place to scan* — Metadata sidecars
- DocsImportAsyncService.importZipAndCleanFile — *removed* — old upfront whole-zip scan
- luz-docs-import — *added* — dedicated per-sidecar scan
- luz-docs-import — *references* — luz-docs-import scans metadata sidecars per-file, not the whole ZIP
- luz-docs-import — *references* — Per-file AV scan rejects one file; whole-job scan fails the whole import

%% ai-graph-end %%