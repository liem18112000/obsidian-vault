---
title: "LUZ-158230 ePost ZIP Import Test Fixture Matrix (Confluence)"
created: 2026-09-18
type: reference
status: seedling
source: "session 2026-09-18 run-655b14d9; Confluence pageId 49665769474"
tags: [testing-agent, zip-import, fixtures, LUZ-158230, ePost, eArchive]
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
