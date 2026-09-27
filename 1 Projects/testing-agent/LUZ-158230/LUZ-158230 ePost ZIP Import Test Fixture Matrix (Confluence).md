---
ai_hash: 1bba10a29ef9d774
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-18
entities:
- LUZ-158230
- ePost ZIP Import Test Fixture Matrix
- Confluence
- ePost health-document ZIP import
- '49665769474'
- child pages
- run results
- dev 13/08
- dev 18/08
- staging 19/08 2026
- 40 fixtures
- happy path
- sidecar metadata
- antivirus
- file-type allow-list
- per-document size
- duplicates/idempotency
- ignored entries
- encoding/names
- archive-level rejection
- request-shape rejection
- concurrency/flush
- stale-job timeout
- structure/security
- rejection precedence
- suggested run order
- 01 smoke
- sync rejections
- security 25
- volume 08
- job doc
- GET {tenant-id}/import-jobs/{id}
- status
- failureCode
- successfulFiles
- skippedFiles
- rejectedFiles
- failedFiles
- unprocessedFiles
- BUFFER_SIZE
- 100 KiB
- MAX_DOCUMENT_FILE_SIZE
- 200 MiB
- MAX_ZIP_FILE_SIZE
- 2 GiB
- IMPORT_CONCURRENCY_MAX
- '64'
- 3600 s
- FileTypeValidator
- IdempotentImportService
- AntiviusScanningService
- failJobIfStale
- MetadataSidecarResolver
- extension-only file-type check
- spoof `payload.exe.pdf` imports
- document bytes
- .metadata.json sidecar
- content-blind path idempotency
- ticket family
- fixture IDs
- luz_docs_import ZIP import is path-based idempotent per importZipName
source: session 2026-09-18 run-655b14d9; Confluence pageId 49665769474
status: seedling
tags:
- testing-agent
- zip-import
- fixtures
- LUZ-158230
- ePost
- eArchive
title: LUZ-158230 ePost ZIP Import Test Fixture Matrix (Confluence)
type: reference
---

# LUZ-158230 ePost ZIP Import Test Fixture Matrix (Confluence)

The authoritative test suite for ePost health-document ZIP import (LUZ-158230) is the Confluence page **"ePost ZIP Import Test Fixture Matrix"** (pageId 49665769474), with three child pages of actual run results (dev 13/08, dev 18/08, staging 19/08 2026).

Contents worth reusing:
- **40 fixtures** (01–40) grouped by concern: happy path, sidecar metadata, antivirus, file-type allow-list, per-document size, duplicates/idempotency, ignored entries, encoding/names, archive-level rejection, request-shape rejection, concurrency/flush, stale-job timeout, structure/security, rejection precedence.
- A **suggested run order** (01 smoke first; sync rejections next; security 25 before ship; volume 08 last).
- Per-run oracle: assert on the job doc `GET {tenant-id}/import-jobs/{id}` — final `status`, `failureCode`, and the five buckets `successfulFiles / skippedFiles / rejectedFiles / failedFiles / unprocessedFiles`.
- Constants: metadata `BUFFER_SIZE`=100 KiB, `MAX_DOCUMENT_FILE_SIZE`=200 MiB, `MAX_ZIP_FILE_SIZE`=2 GiB, `IMPORT_CONCURRENCY_MAX`=64, stale timeout 3600 s.
- New components under test: `FileTypeValidator`, `IdempotentImportService`, `AntiviusScanningService`, `failJobIfStale`, `MetadataSidecarResolver`-style pairing.
- **Known limitations flagged**: extension-only file-type check (spoof `payload.exe.pdf` imports), document bytes not AV-scanned (only the `.metadata.json` sidecar is), content-blind path idempotency.

When (re)testing this ticket family, align generated scenarios 1:1 to these fixture IDs rather than re-deriving a drifting suite.

## Related

- [[luz_docs_import ZIP import is path-based idempotent per importZipName]]

%% ai-graph-start %%

**Related notes:**
- [[LUZ-158230 eArchive Health ZIP import - golden test fixture matrix location]]
- [[luz_docs_import ZIP import behavior — limits, allow-list, idempotency, AV scope]]
- [[LUZ-158230 ePost ZIP import - test scope decisions]]
- [[LUZ-158230 QA edge-case decisions (ZIP import)]]
- [[LUZ-158230 test approach full-chain real-deps integration with fully-materialized done]]

**Relations:**
- ePost ZIP Import Test Fixture Matrix — *is_associated_with* — LUZ-158230
- ePost ZIP Import Test Fixture Matrix — *is_a_page_on* — Confluence
- ePost ZIP Import Test Fixture Matrix — *is_authoritative_test_suite_for* — ePost health-document ZIP import
- ePost health-document ZIP import — *is_associated_with* — LUZ-158230
- ePost ZIP Import Test Fixture Matrix — *has_page_id* — 49665769474
- ePost ZIP Import Test Fixture Matrix — *has* — child pages
- child pages — *contain* — run results
- run results — *include* — dev 13/08
- run results — *include* — dev 18/08
- run results — *include* — staging 19/08 2026
- ePost ZIP Import Test Fixture Matrix — *defines* — 40 fixtures
- 40 fixtures — *grouped_by_concern* — happy path
- 40 fixtures — *grouped_by_concern* — sidecar metadata
- 40 fixtures — *grouped_by_concern* — antivirus
- 40 fixtures — *grouped_by_concern* — file-type allow-list
- 40 fixtures — *grouped_by_concern* — per-document size
- 40 fixtures — *grouped_by_concern* — duplicates/idempotency
- 40 fixtures — *grouped_by_concern* — ignored entries
- 40 fixtures — *grouped_by_concern* — encoding/names
- 40 fixtures — *grouped_by_concern* — archive-level rejection
- 40 fixtures — *grouped_by_concern* — request-shape rejection
- 40 fixtures — *grouped_by_concern* — concurrency/flush
- 40 fixtures — *grouped_by_concern* — stale-job timeout
- 40 fixtures — *grouped_by_concern* — structure/security
- 40 fixtures — *grouped_by_concern* — rejection precedence
- ePost ZIP Import Test Fixture Matrix — *suggests* — suggested run order
- suggested run order — *includes* — 01 smoke
- suggested run order — *includes* — sync rejections
- suggested run order — *includes* — security 25
- suggested run order — *includes* — volume 08
- job doc — *is_oracle_for* — run results
- job doc — *accessed_via* — GET {tenant-id}/import-jobs/{id}
- job doc — *has_field* — status
- job doc — *has_field* — failureCode
- job doc — *has_field* — successfulFiles
- job doc — *has_field* — skippedFiles
- job doc — *has_field* — rejectedFiles
- job doc — *has_field* — failedFiles
- job doc — *has_field* — unprocessedFiles
- BUFFER_SIZE — *has_value* — 100 KiB
- MAX_DOCUMENT_FILE_SIZE — *has_value* — 200 MiB
- MAX_ZIP_FILE_SIZE — *has_value* — 2 GiB
- IMPORT_CONCURRENCY_MAX — *has_value* — 64
- stale timeout — *has_value* — 3600 s
- FileTypeValidator — *is_component_under_test* — true
- IdempotentImportService — *is_component_under_test* — true
- AntiviusScanningService — *is_component_under_test* — true
- failJobIfStale — *is_component_under_test* — true
- MetadataSidecarResolver — *is_component_under_test* — true
- extension-only file-type check — *is_known_limitation* — true
- document bytes — *is_not_AV_scanned* — true
- content-blind path idempotency — *is_known_limitation* — true
- extension-only file-type check — *allows* — spoof `payload.exe.pdf` imports
- .metadata.json sidecar — *is_AV_scanned* — true
- ticket family — *aligns_scenarios_to* — fixture IDs
- luz_docs_import ZIP import is path-based idempotent per importZipName — *is_related_to* — ePost ZIP Import Test Fixture Matrix

%% ai-graph-end %%