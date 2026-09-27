---
ai_hash: 57e5ffb340a25144
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
confluence_id: '49674518529'
confluence_path: Team Kepler > Developer note > ePost ZIP Import Test Fixture Matrix
created: 2026-08-18
entities: []
source: Confluence · TK - Team Kepler
status: reference
tags:
- confluence
- epost
title: ePost Zip-Import - dev test-suite results - 18/08/2026
type: source
updated: 2026-08-18
url: https://axonivy.atlassian.net/wiki/spaces/TK/pages/49674518529/ePost+Zip-Import+-+dev+test-suite+results+-+18+08+2026
---

# ePost Zip-Import - dev test-suite results - 18/08/2026

*Confluence source · Team Kepler › Developer note › ePost ZIP Import Test Fixture Matrix · [view original](https://axonivy.atlassian.net/wiki/spaces/TK/pages/49674518529/ePost+Zip-Import+-+dev+test-suite+results+-+18+08+2026) · updated 2026-08-18*

## Test Information

- **Tenant:** `d0783310-d67f-4ab7-9aab-dcaef3f17f48` \* **env:** dev \* **api-forwarder:** localhost:18080 -\> dev

- **Zip source:** `data/Lam/import-test-zips/import-test-zips` (v2 matrix, 43 cases; `Book505Mb.zip` excluded)

- **Clean before run:** truncated `documents` (686), `folders` (70) and `document-import-jobs` (38) for the tenant, so every case re-imported fresh (no dedup against prior runs).

- **Method:** distinct zip names =\> no cross-case dedup; each case = one `POST /import-jobs/upload-zip` polled to terminal status; assertions per `TEST-MATRIX-v2.md`.

- **Artifacts:** per-case job JSON in `jobs/`, driver logs in `logs/`, raw `results.tsv`.

[[3 Resources/Confluence/Team Kepler/Developer note/attachments/epost-zip-import-dev-test-suite-results-18-08-2026/import-matrix-v2-20260818-1233.zip|import-matrix-v2-20260818-1233.zip]]

### Summary

- **Cases run:** 43

- **PASS:** 38 \* **NOTE (expected-given-setup):** 4 \* **DEVIATION (flag):** 1

- **Synchronous rejections (no job, correct):** 5 (18, 19, 19b, 20b -\> HTTP 400; 20 -\> client not-a-zip)

- **Async jobs:** all reached terminal `DONE`; no job hung, no `unprocessedFiles` left pending, no `FAILED`/`INTERNAL_SERVICE_ERROR`.

### Verdict legend

- **PASS** - observed buckets match the v2 matrix expectation. \*\*PASS\*\*\* - first-import half of a two-upload idempotency case; skip half not exercised in a single clean pass.

- **NOTE** - behavior is correct *for this harness setup* (AV not fault-injected; idempotency needs a prior job in the same run).

- **DEVIATION** - observed result differs from the matrix expectation; flagged below.

## Flagged deviation

#### 12 - one metadata-typed document lands in failedFiles

Matrix wording says "all 12 imported". Observed 11 imported + 1 failed:

```
fail  Optional fields/wrong types 006.pdf   [Document Types must be an array.]
```

- The `wrong types` fixture ships `documentTypes` as a non-array; the sidecar apply throws a validation error and the document is bucketed as **failed** rather than imported-with-ignored-metadata.

- Arguably stricter-than-spec (matrix section 12 says no field is individually schema-validated).

- Flag to the spec owner: should a bad `documentTypes` **fail** the document or be **ignored** like other unparseable metadata (case 10)?

## Notable NOTE cases

- **29 / 30 (AV timeout / technical-error):** both docs imported (2 ok, 0 failed). These fixtures are plain docs+sidecars with **no fault injected**; exercising the `failedFiles` path requires making luz-antivirus actually slow/down. Not a regression.

- **34 (re-import changed body) / 36b (collision setB):** on a clean run with per-file-distinct zip names these import fresh; the *skip* behaviour only appears when a prior job for the **same** `importZipName`**+path** already exists in the same run.

- **15 (duplicates-upload-twice):** single pass imports the 4 docs; the `skippedFiles`/`ALREADY_IMPORTED` half needs the same zip uploaded a second time.

### Results table

|  |  |  |  |  |  |  |  |  |  |
|----|----|----|----|----|----|----|----|----|----|
| **\#** | **Case** | **Status** | **ok** | **fail** | **skip** | **reject** | **http** | **Expected** | **Verdict** |
| 1 | 01-happy-spec-example | DONE | 3 | 0 | 0 | 0 | 200 | 3 docs imported w/ metadata, 2 folders | PASS |
| 2 | 02-happy-nested-folders | DONE | 12 | 0 | 0 | 0 | 200 | 12 docs, 6-folder tree depth 3 | PASS |
| 3 | 03-happy-root-and-folders | DONE | 6 | 0 | 0 | 0 | 200 | 6 docs (4 in folders + 2 at root) | PASS |
| 4 | 04-happy-no-metadata | DONE | 4 | 0 | 0 | 0 | 200 | 4 docs, title = file name | PASS |
| 5 | 05-happy-mixed-metadata | DONE | 4 | 0 | 0 | 0 | 200 | 4 docs (2 with sidecar, 2 without) | PASS |
| 6 | 06-happy-os-noise-ignored | DONE | 4 | 0 | 0 | 0 | 200 | 4 docs; \_\_MACOSX/.DS_Store/Thumbs.db ignored | PASS |
| 7 | 07-happy-empty-folders | DONE | 2 | 0 | 0 | 0 | 200 | 5 folders created, 2 docs | PASS |
| 8 | 08-happy-volume-500-docs | DONE | 500 | 0 | 0 | 0 | 200 | 500 docs, unprocessed drains to 0, DONE | PASS |
| 9 | 09-edge-metadata-orphan | DONE | 2 | 0 | 0 | 3 | 200 | 2 docs; 3 orphan sidecars -\> rejectedFiles (ORPHAN_METADATA) | PASS |
| 10 | 10-edge-metadata-unparseable | DONE | 9 | 0 | 0 | 0 | 200 | 9 docs imported, 0 failed; each carries ignoredReason | PASS |
| 11 | 11-edge-metadata-unknown-toplevel | DONE | 4 | 0 | 0 | 0 | 200 | 4 docs; only allowed fields applied | PASS |
| 12 | 12-edge-metadata-optional-fields | DONE | 11 | 1 | 0 | 0 | 200 | all 12 imported per matrix | DEVIATION |
| 13 | 13-edge-metadata-naming | DONE | 11 | 0 | 0 | 7 | 200 | only correct 001.pdf binds sidecar; others orphan/disallowed -\> rejected | PASS |
| 14 | 14-edge-metadata-oversized | DONE | 6 | 0 | 0 | 0 | 200 | docs imported; \>100 KiB sidecar ignored | PASS |
| 15 | 15b-edge-duplicate-entries-in-zip | DONE | 1 | 0 | 0 | 0 | 200 | no crash; defined outcome (1 doc) | PASS |
| 16 | 15-edge-duplicates-upload-twice | DONE | 4 | 0 | 0 | 0 | 200 | 1st import of 4 docs (skip semantics need 2nd upload) | PASS\* |
| 17 | 16-edge-dot-entries-and-artifacts | DONE | 2 | 0 | 0 | 0 | 200 | 2 docs; only dot-prefixed LEAF file names are ignored, dot-prefixed FOLDERS are kept (by design) | PASS |
| 18 | 17-edge-utf8-names | DONE | 9 | 1 | 0 | 0 | 200 | 9 docs round-trip UTF-8 names | PASS |
| 19 | 18-edge-password-protected | SYNC_REJECT | . | . | . | . | 400 | synchronous 400 at upload (password) | PASS |
| 20 | 19b-edge-zip-bomb-fixture | SYNC_REJECT | . | . | . | . | 400 | synchronous 400 (\>4 GiB / \>50x) | PASS |
| 21 | 19-edge-zip-bomb | SYNC_REJECT | . | . | . | . | 400 | synchronous 400 (\>50x ratio) | PASS |
| 22 | 20b-edge-truncated-zip | SYNC_REJECT | . | . | . | . | 400 | 400 OR job FAILED/INVALID | PASS |
| 23 | 20-edge-not-a-zip | CLIENT_NOT_A_ZIP | . | . | . | . | . | not-a-zip -\> 400 (PDF renamed .zip) | PASS |
| 24 | 21b-edge-macos-created | DONE | 1 | 0 | 0 | 0 | 200 | ext chars survive; \_\_MACOSX ignored (1 doc) | PASS |
| 25 | 21c-edge-utf8-flagged | DONE | 2 | 0 | 0 | 0 | 200 | UTF-8 flagged names correct (2 docs) | PASS |
| 26 | 21-edge-cp437-names | DONE | 1 | 0 | 0 | 0 | 200 | CP437 names not mojibake (1 doc) | PASS |
| 27 | 22-edge-mixed-file-types | DONE | 4 | 1 | 0 | 4 | 200 | png/jpg/txt/corrupt-pdf import; zip/bin/bare-json + orphan sidecar rejected; 0-byte pdf fails | PASS |
| 28 | 23b-edge-folders-only | DONE | 0 | 0 | 0 | 0 | 200 | 5 folders, 0 docs, DONE | PASS |
| 29 | 23-edge-empty | DONE | 0 | 0 | 0 | 0 | 200 | 0 docs, DONE | PASS |
| 30 | 24-edge-deep-nesting | DONE | 11 | 0 | 0 | 0 | 200 | 11 docs; deep path walk | PASS |
| 31 | 25-edge-path-traversal | DONE | 2 | 4 | 0 | 0 | 200 | Zip-Slip contained: traversal entries not written outside jobdir | PASS |
| 32 | 28-av-metadata-infected | DONE | 1 | 0 | 0 | 1 | 200 | infected sidecar -\> doc rejected (METADATA_INFECTED); clean doc imports | PASS |
| 33 | 29-av-scan-timeout | DONE | 2 | 0 | 0 | 0 | 200 | failedFiles on AV timeout - needs fault injection | NOTE |
| 34 | 30-av-scan-technical-error | DONE | 2 | 0 | 0 | 0 | 200 | failedFiles on AV error - needs fault injection | NOTE |
| 35 | 31-edge-filetype-disallowed | DONE | 1 | 0 | 0 | 6 | 200 | disallowed types -\> rejected; valid sibling imports | PASS |
| 36 | 32-edge-filetype-boundaries | DONE | 2 | 0 | 0 | 2 | 200 | .PDF & \*.exe.pdf import; extensionless & \*.pdf.exe rejected | PASS |
| 37 | 34-edge-reimport-changed-body | DONE | 4 | 0 | 0 | 0 | 200 | clean run -\> 4 imported (skip only visible after 15) | NOTE |
| 38 | 35-edge-reimport-renamed-zip | DONE | 4 | 0 | 0 | 0 | 200 | different zip name -\> not deduped, imported again (4) | PASS |
| 39 | 36a-edge-collision-setA | DONE | 2 | 0 | 0 | 0 | 200 | setA imports (2) | PASS |
| 40 | 36b-edge-collision-setB | DONE | 2 | 0 | 0 | 0 | 200 | distinct zip name -\> not deduped (2); same-name collision needs rename | NOTE |
| 41 | 37-concurrency-mixed | DONE | 50 | 0 | 0 | 5 | 200 | 50 imported + 5 rejected under concurrency; buckets consistent | PASS |
| 42 | 39c-precedence-av-infected | DONE | 0 | 0 | 0 | 1 | 200 | infected sidecar wins -\> doc rejected (METADATA_INFECTED) | PASS |
| 43 | 40-happy-root-only-file | DONE | 1 | 0 | 0 | 0 | 200 | 1 root doc | PASS |

%% ai-graph-start %%

**Related notes:**
- [[ePost ZIP-import — dev test-suite results - 13-08-2026]]
- [[ePost ZIP Import Test Fixture Matrix]]
- [[ePost Zip-Import - staging test-suite results - 19-08-2026]]
- [[LUZ-158230 ePost ZIP Import Test Fixture Matrix (Confluence)]]
- [[LUZ-158230 eArchive Health ZIP import - golden test fixture matrix location]]

%% ai-graph-end %%