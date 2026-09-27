---
ai_hash: 9f9be690b87cedd7
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-13
entities:
- luz-docs-import
- AV scan
- metadata sidecar
- document binary
- ZIP import
- DocsImportAsyncService.processDocumentFile
- antiviusScanningService.scanMetadataFile
- document
- rejectedFiles
- failedFiles
- proceed
- INFECTED
- TIMEOUT
- TECHNICAL_ERROR
- CLEAN
- storage
- security intent
- current design
- RESTEasy multipart repeated field name yields a List, get(0) silently drops extras
- zero AV scanning
- infected document body
- clean sidecar
- absent sidecar
- infected sidecar
source: session 2026-08-13
status: seedling
tags:
- luz-docs
- antivirus
- security
- import
- gap
title: luz-docs-import AV scan covers only the metadata sidecar, never the document
  binary
type: observation
---

# luz-docs-import AV scan covers only the metadata sidecar, never the document binary

In `luz_docs_import`, the antivirus scan during ZIP import only ever scans the `.metadata.json` **sidecar**, not the document binary itself.

`DocsImportAsyncService.processDocumentFile` calls `antiviusScanningService.scanMetadataFile(metadataFile, token)` **only when `getMetadataJsonFile(file).isFile()`** is true. The document file (`.pdf`, `.png`, etc.) is never passed to the AV service.

**Consequences:**
- A document with **no sidecar** is imported with **zero AV scanning**.
- An **infected document body** with a clean (or absent) sidecar is not caught.
- When the *sidecar* is INFECTED, the whole **document** is rejected (`recordRejected`, "Virus found in the metadata file"), even though the document binary was the thing never scanned.

Scan outcomes and where they route the document: `INFECTED` -> rejectedFiles; `TIMEOUT` (30s) / `TECHNICAL_ERROR` -> failedFiles; `CLEAN` -> proceed.

**Why it matters:** if the security intent is "no infected document reaches storage", this design does not achieve it — only sidecar JSON is scanned. Flag for the spec owner; likely a gap, not a deliberate decision. See [[RESTEasy multipart repeated field name yields a List, get(0) silently drops extras]] for the same repo.

%% ai-graph-start %%

**Related notes:**
- [[luz-docs-import scans metadata sidecars per-file, not the whole ZIP]]
- [[Metadata sidecars must be scanned in luz-docs-import because they are never uploaded]]
- [[Per-file AV scan rejects one file; whole-job scan fails the whole import]]
- [[luz_docs_import ZIP uploads are not virus-scanned (AntiviusScanningService is never wired in)]]
- [[luz-docs-import antivirus whole-zip scan dominates first-import latency and scales with zip size]]

**Relations:**
- luz-docs-import — *performs* — AV scan
- AV scan — *covers* — metadata sidecar
- AV scan — *excludes* — document binary
- AV scan — *occurs during* — ZIP import
- DocsImportAsyncService.processDocumentFile — *calls* — antiviusScanningService.scanMetadataFile
- antiviusScanningService.scanMetadataFile — *scans* — metadata sidecar
- antiviusScanningService.scanMetadataFile — *is conditional on existence of* — metadata sidecar
- document binary — *is not scanned by* — antiviusScanningService.scanMetadataFile
- document — *with no sidecar receives* — zero AV scanning
- infected document body — *with clean sidecar is not* — caught
- infected document body — *with absent sidecar is not* — caught
- infected sidecar — *causes rejection of* — document
- document — *rejected due to infected sidecar is routed to* — rejectedFiles
- INFECTED — *routes to* — rejectedFiles
- TIMEOUT — *routes to* — failedFiles
- TECHNICAL_ERROR — *routes to* — failedFiles
- CLEAN — *routes to* — proceed
- security intent — *is* — "no infected document reaches storage"
- current design — *does not achieve* — security intent
- current design — *scans only* — metadata sidecar
- luz-docs-import — *is in same repository as* — RESTEasy multipart repeated field name yields a List, get(0) silently drops extras
- AV scan — *has timeout of* — 30s

%% ai-graph-end %%