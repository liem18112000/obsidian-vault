---
ai_hash: 96af26980dc25cd1
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-22
entities:
- LUZ-158230
- eArchive Health ZIP import
- Epic
- Resolved
- kepler
- mt-receive
- luz_docs_import
- ePost
- Post Health
- transfer.zip
- filename.metadata.json
- eArchive
- HEALTH document type
- healthData
- author
- SNOMED
- documentCategory
- facility
- practiceSetting
- ePost ZIP Import Test Fixture Matrix
- Confluence
- '49665769474'
- TK
- Team Kepler
- Liem Doan
- job document GET {tenant-id}/import-jobs/{id}
- status
- failureCode
- successfulFiles
- skippedFiles
- rejectedFiles
- failedFiles
- unprocessedFiles
- ePost - ZIP document import specification.pdf
- ePost - Health documents development handoff.pdf
- Claude Code
- luz-docs-import-api-test
- Gap-3 Unicode
- luz_docs_import ZIP import behavior — limits
- allow-list
- idempotency
- AV scope
source: session 2026-09-22
status: seedling
tags:
- luz-158230
- earchive
- zip-import
- testing-agent
- jira
title: LUZ-158230 eArchive Health ZIP import - golden test fixture matrix location
type: howto
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

%% ai-graph-start %%

**Related notes:**
- [[LUZ-158230 ePost ZIP Import Test Fixture Matrix (Confluence)]]
- [[LUZ-158230 test approach full-chain real-deps integration with fully-materialized done]]
- [[LUZ-158230 is the ePost eArchive transfer.zip document-import feature]]
- [[ePost eArchive ZIP import transfer.zip shape and luz_docs_import]]
- [[ePost ZIP import (LUZ-158230) behavior rules confirmed by domain expert]]

**Relations:**
- LUZ-158230 — *is an* — Epic
- LUZ-158230 — *has status* — Resolved
- LUZ-158230 — *has label* — kepler
- LUZ-158230 — *has label* — mt-receive
- LUZ-158230 — *implemented in* — luz_docs_import
- LUZ-158230 — *is about* — eArchive Health ZIP import
- ePost — *is an* — external sender
- Post Health — *is an example of* — external sender
- ePost — *delivers* — transfer.zip
- transfer.zip — *contains* — filename.metadata.json
- filename.metadata.json — *is a* — sidecar
- eArchive — *is a* — recipient
- HEALTH document type — *carries* — healthData
- healthData — *includes* — author
- healthData — *includes* — documentCategory
- healthData — *includes* — facility
- healthData — *includes* — practiceSetting
- documentCategory — *is coded by* — SNOMED
- facility — *is coded by* — SNOMED
- practiceSetting — *is coded by* — SNOMED
- ePost ZIP Import Test Fixture Matrix — *is an authoritative* — test spec
- ePost ZIP Import Test Fixture Matrix — *is on* — Confluence
- ePost ZIP Import Test Fixture Matrix — *has pageId* — 49665769474
- ePost ZIP Import Test Fixture Matrix — *is in space* — TK
- TK — *is* — Team Kepler
- ePost ZIP Import Test Fixture Matrix — *authored by* — Liem Doan
- ePost ZIP Import Test Fixture Matrix — *is a* — golden matrix
- ePost ZIP Import Test Fixture Matrix — *asserts on* — job document GET {tenant-id}/import-jobs/{id}
- job document GET {tenant-id}/import-jobs/{id} — *returns* — status
- job document GET {tenant-id}/import-jobs/{id} — *returns* — failureCode
- job document GET {tenant-id}/import-jobs/{id} — *returns* — successfulFiles
- job document GET {tenant-id}/import-jobs/{id} — *returns* — skippedFiles
- job document GET {tenant-id}/import-jobs/{id} — *returns* — rejectedFiles
- job document GET {tenant-id}/import-jobs/{id} — *returns* — failedFiles
- job document GET {tenant-id}/import-jobs/{id} — *returns* — unprocessedFiles
- ePost - ZIP document import specification.pdf — *is a* — source of truth
- ePost - Health documents development handoff.pdf — *is a* — source of truth
- luz-docs-import-api-test — *is a* — Claude Code skill
- luz-docs-import-api-test — *used for* — E2E run of upload flow
- luz-docs-import-api-test — *has* — Gap-3 Unicode
- luz_docs_import ZIP import behavior — limits — *is related to* — LUZ-158230
- allow-list — *is related to* — LUZ-158230
- idempotency — *is related to* — LUZ-158230
- AV scope — *is related to* — LUZ-158230

%% ai-graph-end %%