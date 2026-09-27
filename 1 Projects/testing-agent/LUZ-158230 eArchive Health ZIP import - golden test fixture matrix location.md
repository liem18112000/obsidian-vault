---
title: "LUZ-158230 eArchive Health ZIP import - golden test fixture matrix location"
created: 2026-09-22
type: howto
status: seedling
source: "session 2026-09-22"
tags: [luz-158230, earchive, zip-import, testing-agent, jira]
---

# LUZ-158230 eArchive Health ZIP import - golden test fixture matrix location

LUZ-158230 "[eArchive] Health documents ZIP import in ePost" is an Epic (status Resolved, labels `kepler`/`mt-receive`) implemented in the `luz_docs_import` service. An external sender (e.g. Post Health) delivers a `transfer.zip` — folders + a per-document `<filename>.metadata.json` sidecar — into a recipient eArchive. It introduces a `HEALTH` document type carrying structured `healthData`: `author` plus SNOMED-coded `documentCategory` / `facility` / `practiceSetting`, each with de/fr/it/en labels.

The authoritative test spec is the Confluence page **"ePost ZIP Import Test Fixture Matrix"**, pageId `49665769474`, space `TK` (Team Kepler), authored by Liem Doan. It is a ~40-case golden matrix (happy path, sidecar metadata, antivirus, file-type allow-list, per-document size, duplicates/idempotency, encoding/names, archive- and request-level synchronous rejections, concurrency/flush tuning, stale-job timeout, path-traversal security, rejection precedence). Each case asserts on the job document `GET {tenant-id}/import-jobs/{id}`: final `status`, `failureCode`, and the `successfulFiles`/`skippedFiles`/`rejectedFiles`/`failedFiles`/`unprocessedFiles` buckets.

Two attached PDFs are the source of truth: "ePost - ZIP document import specification.pdf" and "ePost - Health documents development handoff.pdf".

For an actual E2E run of the upload flow use the Claude Code skill `luz-docs-import-api-test` (tracks the job two ways; has a Gap-3 Unicode entry-name sub-mode). Code behaviour details are in the linked note.

## Related

- [[luz_docs_import ZIP import behavior — limits]]
- [[allow-list]]
- [[idempotency]]
- [[AV scope]]
