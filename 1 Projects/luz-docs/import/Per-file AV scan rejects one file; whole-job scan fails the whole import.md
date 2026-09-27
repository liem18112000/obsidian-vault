---
ai_hash: 507c3ad5dc2ec445
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-10
entities:
- AntivirusScanningService
- scanUploadFile(...)
- DocsImportBackgroundException
- INFECTED
- INTERNAL_SERVICE_ERROR
- isFileClean(file, token)
- ScanningResult.OK
- job.rejectedFiles
- JobProgressWriter.recordRejected(file, detail)
- LUZ-158230
- luz-docs-import scans metadata sidecars per-file, not the whole ZIP
- Metadata sidecars must be scanned in luz-docs-import because they are never uploaded
- whole-job scan
- per-file scan
- import job
- document
- timeout
- transport errors
- abnormal scan
- failure semantics
- design distinction
- file-level scanning
- job-level scanning
source: session 2026-08-10, LUZ-158230
status: seedling
tags:
- luz-docs-import
- antivirus
- earchive
- LUZ-158230
- fail-closed
- gotcha
title: Per-file AV scan rejects one file; whole-job scan fails the whole import
type: lesson
---

# Per-file AV scan rejects one file; whole-job scan fails the whole import

The two antivirus entry points in `AntiviusScanningService` differ deliberately in their failure semantics — this is the key design distinction:

- `scanUploadFile(...)` (the old whole-zip / whole-job scan) **throws** `DocsImportBackgroundException` (`INFECTED` on NOT_OK, `INTERNAL_SERVICE_ERROR` on any exception). A throw here fails the **entire import job**.
- `isFileClean(file, token)` (the new per-file scan) **swallows** timeouts and transport errors and returns `false`. It returns `true` **only** on an explicit `ScanningResult.OK`. So NOT_OK, a timeout, or any error all mean "reject this one file."

Effect: a bad per-file scan rejects only that single document — added to `job.rejectedFiles` with detail `"Virus found in the metadata file"` via `JobProgressWriter.recordRejected(file, detail)` — and the rest of the import continues. This is the whole point of moving from job-level to file-level scanning.

Requirement it satisfies (LUZ-158230): "if the antivirus times out OR the scan is abnormal => reject the file." Treating timeout the same as an abnormal result is intentional — fail closed.

Related: [[luz-docs-import scans metadata sidecars per-file, not the whole ZIP]], [[Metadata sidecars must be scanned in luz-docs-import because they are never uploaded]].

## Related

- [[luz-docs-import scans metadata sidecars per-file, not the whole ZIP]]
- [[Metadata sidecars must be scanned in luz-docs-import because they are never uploaded]]

%% ai-graph-start %%

**Related notes:**
- [[luz-docs-import scans metadata sidecars per-file, not the whole ZIP]]
- [[luz-docs-import AV scan covers only the metadata sidecar, never the document binary]]
- [[Metadata sidecars must be scanned in luz-docs-import because they are never uploaded]]
- [[luz_docs_import ZIP uploads are not virus-scanned (AntiviusScanningService is never wired in)]]
- [[luz-docs-import antivirus whole-zip scan dominates first-import latency and scales with zip size]]

**Relations:**
- AntivirusScanningService — *has entry point* — scanUploadFile(...)
- AntivirusScanningService — *has entry point* — isFileClean(file, token)
- scanUploadFile(...) — *is a type of* — whole-job scan
- isFileClean(file, token) — *is a type of* — per-file scan
- scanUploadFile(...) — *throws* — DocsImportBackgroundException
- DocsImportBackgroundException — *indicates* — INFECTED
- DocsImportBackgroundException — *indicates* — INTERNAL_SERVICE_ERROR
- whole-job scan — *fails* — import job
- per-file scan — *swallows* — timeout
- per-file scan — *swallows* — transport errors
- isFileClean(file, token) — *returns true only on* — ScanningResult.OK
- per-file scan — *rejects* — document
- document — *is added to* — job.rejectedFiles
- JobProgressWriter.recordRejected(file, detail) — *records rejected* — document
- per-file scan — *uses* — JobProgressWriter.recordRejected(file, detail)
- document — *rejected with detail* — "Virus found in the metadata file"
- per-file scan — *moves from* — job-level scanning
- per-file scan — *moves to* — file-level scanning
- LUZ-158230 — *is satisfied by* — per-file scan
- LUZ-158230 — *requires* — reject file on timeout
- LUZ-158230 — *requires* — reject file on abnormal scan
- timeout — *is treated as* — abnormal scan
- per-file scan — *is related to* — luz-docs-import scans metadata sidecars per-file, not the whole ZIP
- per-file scan — *is related to* — Metadata sidecars must be scanned in luz-docs-import because they are never uploaded
- scanUploadFile(...) — *has* — failure semantics
- isFileClean(file, token) — *has* — failure semantics
- failure semantics — *is a* — design distinction

%% ai-graph-end %%