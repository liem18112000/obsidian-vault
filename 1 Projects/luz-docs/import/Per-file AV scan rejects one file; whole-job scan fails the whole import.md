---
ai_hash: 2ee1c3febdba98ea
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-10
entities:
- AntivirusScanningService
- scanUploadFile(...)
- DocsImportBackgroundException
- INFECTED
- INTERNAL_SERVICE_ERROR
- import job
- isFileClean(file, token)
- ScanningResult.OK
- timeout
- transport errors
- single document
- job.rejectedFiles
- JobProgressWriter.recordRejected(file, detail)
- LUZ-158230
- abnormal result
- job-level scanning
- file-level scanning
- luz-docs-import scans metadata sidecars per-file, not the whole ZIP
- Metadata sidecars must be scanned in luz-docs-import because they are never uploaded
- luz-docs-import
- metadata sidecars
- whole ZIP
- per-file scan
- whole-job scan
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

- [[luz-docs-import scans metadata sidecars per-file]]
- [[not the whole ZIP]]
- [[Metadata sidecars must be scanned in luz-docs-import because they are never uploaded]]

%% ai-graph-start %%

**Related notes:**
- [[luz-docs-import scans metadata sidecars per-file, not the whole ZIP]]
- [[luz-docs-import AV scan covers only the metadata sidecar, never the document binary]]
- [[Metadata sidecars must be scanned in luz-docs-import because they are never uploaded]]
- [[luz_docs_import ZIP uploads are not virus-scanned (AntiviusScanningService is never wired in)]]
- [[luz-docs-import antivirus whole-zip scan dominates first-import latency and scales with zip size]]

**Relations:**
- AntivirusScanningService — *has_entry_point* — scanUploadFile(...)
- AntivirusScanningService — *has_entry_point* — isFileClean(file, token)
- scanUploadFile(...) — *is_type_of* — whole-job scan
- isFileClean(file, token) — *is_type_of* — per-file scan
- scanUploadFile(...) — *throws* — DocsImportBackgroundException
- DocsImportBackgroundException — *is_status_INFECTED_on* — NOT_OK
- DocsImportBackgroundException — *is_status_INTERNAL_SERVICE_ERROR_on* — any exception
- scanUploadFile(...) — *fails_entire* — import job
- isFileClean(file, token) — *swallows* — timeout
- isFileClean(file, token) — *swallows* — transport errors
- isFileClean(file, token) — *returns_false_on* — timeout
- isFileClean(file, token) — *returns_false_on* — transport errors
- isFileClean(file, token) — *returns_true_only_on* — ScanningResult.OK
- isFileClean(file, token) — *rejects_on_NOT_OK* — single document
- isFileClean(file, token) — *rejects_on_timeout* — single document
- isFileClean(file, token) — *rejects_on_any_error* — single document
- per-file scan — *rejects* — single document
- single document — *added_to* — job.rejectedFiles
- JobProgressWriter.recordRejected(file, detail) — *records* — job.rejectedFiles
- per-file scan — *uses* — JobProgressWriter.recordRejected(file, detail)
- per-file scan — *satisfies_requirement* — LUZ-158230
- LUZ-158230 — *requires_rejection_if* — timeout
- LUZ-158230 — *requires_rejection_if* — abnormal result
- timeout — *is_treated_as* — abnormal result
- per-file scan — *is_move_from* — job-level scanning
- per-file scan — *is_move_to* — file-level scanning
- luz-docs-import scans metadata sidecars per-file, not the whole ZIP — *is_related_to* — per-file scan
- Metadata sidecars must be scanned in luz-docs-import because they are never uploaded — *is_related_to* — per-file scan
- luz-docs-import — *scans* — metadata sidecars
- metadata sidecars — *are_not* — whole ZIP
- metadata sidecars — *are_never* — uploaded

%% ai-graph-end %%