---
ai_hash: 45bf14ca89782bcc
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
confluence_id: '49681301505'
confluence_path: Team Kepler > Developer note > ePost ZIP Import Test Fixture Matrix
created: 2026-08-20
entities: []
source: Confluence · TK - Team Kepler
status: reference
tags:
- confluence
- epost
title: ePost Zip-Import - staging test-suite results - 19/08/2026
type: source
updated: 2026-08-20
url: https://axonivy.atlassian.net/wiki/spaces/TK/pages/49681301505/ePost+Zip-Import+-+staging+test-suite+results+-+19+08+2026
---

# ePost Zip-Import - staging test-suite results - 19/08/2026

*Confluence source · Team Kepler › Developer note › ePost ZIP Import Test Fixture Matrix · [view original](https://axonivy.atlassian.net/wiki/spaces/TK/pages/49681301505/ePost+Zip-Import+-+staging+test-suite+results+-+19+08+2026) · updated 2026-08-20*

## Test Information

- **Tenant:** `6873c725-bf58-4c9e-b9bf-173059b692cd` ·

- **env:** `dev-staging`

- **Run:** 2026-08-19 15:35–15:56 (local) · artifacts in this folder (`jobs/`, `logs/`, `results.tsv`, `run_matrix.sh`)

- **Zip source:** `data/Lam/import-test-zips/import-test-zips` — every fixture once (44 cases; `Book505Mb.zip` excluded), plus 3 idempotency second-uploads.

- **Deployed build under test:** `luz-docs-import` image `d4c18dc2…` = branch `origin/hotfix/0.00.14.01` ("Change hotfix version", 2026-08-18). **This build does NOT contain the LUZ-158230 folder-consistency fix**.

[[run_matrix.zip|run_matrix.zip]]

### Summary

- **Cases run:** 47 (44 fixtures + 3 idempotency re-uploads)

- **v2 suite:** all behaviors match the established dev baseline — **PASS** and the expected **NOTES** (AV needs fault injection; single-pass idempotency).

- **v3 suite (cases 41 & 16):** ❌ **root-orphan bug is PRESENT on dev-staging** — dot-prefixed folders are dropped and their children land at the archive root (`folderIds:[]`). **This is the pre-fix behaviour and is expected here because the deployed build lacks LUZ-158230** — it is a *deployment gap*, not a regression. On `dev` (where the fix is deployed) the same cases pass.

- **Synchronous rejections (no job, correct):** 5 (18, 19, 19b, 20b → HTTP 400; 20 → client not-a-zip).

- **Idempotency (second uploads):** all 3 confirm path-based dedup (skip on re-import; content ignored; per-`importZipName` scoping).

- **Async jobs:** every job reached terminal `DONE`; no hung job, no `unprocessedFiles` left pending, no `FAILED`/`INTERNAL_SERVICE_ERROR`.

## ⚠️ Orphan documents at the archive root

The v3 matrix asserts one **load-bearing invariant**: *a document whose folder cannot be created is never orphaned at the archive root — it is either placed inside its folder or recorded in* `failedFiles`*.* On dev-staging this invariant is **violated**, because the deployed build (`hotfix/0.00.14.01`, `d4c18dc2`) predates LUZ-158230.

Verified directly in MongoDB (`documents.folderIds`; an empty array = archive root):

|  |  |  |  |  |
|----|----|----|----|----|
| Case | Document (zip path) | folderIds | Folder created? | Result |
| 41 | `.rejected folder/blocked 001.pdf` | `[]` | `.rejected folder` **absent** | ❌ **ROOT ORPHAN** |
| 41 | `.rejected folder/Nested/blocked 002.pdf` | `[Nested]` | `Nested` created **root-level** (dot parent dropped) | ⚠️ mis-placed |
| 41 | `Clean folder/ok 003.pdf` | `[Clean folder]` | `Clean folder` created | ✅ correct |
| 16 | `.hidden folder/inside a dot folder.pdf` | `[]` | `.hidden folder` **absent** | ❌ **ROOT ORPHAN** |
| 16 | `Visible/real document 001.pdf` | `[Visible]` | `Visible` created | ✅ correct |

The job document is **silent** about the bug: it lists `.rejected folder/blocked 001.pdf` and `.hidden folder/inside a dot folder.pdf` in `successfulFiles` (with `failedFolders: []`), keeping the zip-relative path even though the folder was never created and the file went to root. This is exactly the failure mode LUZ-158230 addresses (create dot-prefixed folders; cascade a folder-creation failure to `failedFiles` instead of orphaning at root).

**Interpretation:** this is **not** a regression — it confirms the pre-fix bug is still live on dev-staging and that **the fix branch has not been deployed there**. To validate v3 on dev-staging, ship `mt-receive/LUZ-158230/fix-folder-issues` to that env and re-run cases 41 & 16.

### Results table

|  |  |  |  |  |  |  |  |  |  |
|----|----|----|----|----|----|----|----|----|----|
| **\#** | **Case** | **Status** | **ok** | **fail** | **skip** | **reject** | **http** | **Expected** | **Verdict** |
| 1 | 01-happy-spec-example | DONE | 3 | 0 | 0 | 0 | 200 | 3 docs w/ metadata, 2 folders | PASS |
| 2 | 02-happy-nested-folders | DONE | 12 | 0 | 0 | 0 | 200 | 12 docs, 6-folder tree depth 3 | PASS |
| 3 | 03-happy-root-and-folders | DONE | 6 | 0 | 0 | 0 | 200 | 6 docs (4 in folders + 2 root) | PASS |
| 4 | 04-happy-no-metadata | DONE | 4 | 0 | 0 | 0 | 200 | 4 docs, title = file name | PASS |
| 5 | 05-happy-mixed-metadata | DONE | 4 | 0 | 0 | 0 | 200 | 4 docs (2 w/ sidecar, 2 without) | PASS |
| 6 | 06-happy-os-noise-ignored | DONE | 4 | 0 | 0 | 0 | 200 | 4 docs; `__MACOSX`/`.DS_Store`/`Thumbs.db` ignored | PASS |
| 7 | 07-happy-empty-folders | DONE | 2 | 0 | 0 | 0 | 200 | 5 folders created, 2 docs | PASS |
| 8 | 08-happy-volume-500-docs | DONE | 500 | 0 | 0 | 0 | 200 | 500 docs, unprocessed drains to 0 | PASS |
| 9 | 09-edge-metadata-orphan | DONE | 2 | 0 | 0 | 3 | 200 | 2 docs; 3 orphan sidecars → rejected (ORPHAN_METADATA) | PASS |
| 10 | 10-edge-metadata-unparseable | DONE | 9 | 0 | 0 | 0 | 200 | 9 docs, 0 failed; each carries ignoredReason | PASS |
| 11 | 11-edge-metadata-unknown-toplevel | DONE | 4 | 0 | 0 | 0 | 200 | 4 docs; only allowed fields applied | PASS |
| 12 | 12-edge-metadata-optional-fields | DONE | 11 | 1 | 0 | 0 | 200 | matrix: "all 12 imported" | PASS |
| 13 | 13-edge-metadata-naming | DONE | 11 | 0 | 0 | 7 | 200 | only `correct 001.pdf` binds sidecar; others orphan/disallowed → rejected | PASS |
| 14 | 14-edge-metadata-oversized | DONE | 6 | 0 | 0 | 0 | 200 | docs imported; \>100 KiB sidecar ignored | PASS |
| 15b | 15b-edge-duplicate-entries-in-zip | DONE | 1 | 0 | 0 | 0 | 200 | no crash; defined outcome (1 doc) | PASS |
| 15 | 15-edge-duplicates-upload-twice | DONE | 4 | 0 | 0 | 0 | 200 | 1st import of 4 docs (skip half → 15R) | PASS\* |
| 16 | 16-edge-dot-entries-and-artifacts | DONE | 2 | 0 | 0 | 0 | 200 | v3: 2 docs, **no root orphan** (`.hidden folder/…` inside its folder) | **FAIL AS EXPECTED** |
| 17 | 17-edge-utf8-names | DONE | 9 | 1 | 0 | 0 | 200 | 9 docs round-trip UTF-8; the 1 fail is a 121-char-path *sidecar* (name-too-long), not a document | PASS |
| 18 | 18-edge-password-protected | SYNC_REJECT | · | · | · | · | 400 | synchronous 400 (password) | PASS |
| 19b | 19b-edge-zip-bomb-fixture | SYNC_REJECT | · | · | · | · | 400 | synchronous 400 (\>4 GiB / \>50×) | PASS |
| 19 | 19-edge-zip-bomb | SYNC_REJECT | · | · | · | · | 400 | synchronous 400 (\>50× ratio) | PASS |
| 20b | 20b-edge-truncated-zip | SYNC_REJECT | · | · | · | · | 400 | 400 OR job FAILED/INVALID | PASS |
| 20 | 20-edge-not-a-zip | CLIENT_NOT_A_ZIP | · | · | · | · | · | not-a-zip → rejected (PDF renamed `.zip`) | PASS |
| 21b | 21b-edge-macos-created | DONE | 1 | 0 | 0 | 0 | 200 | ext chars survive; `__MACOSX` ignored | PASS |
| 21c | 21c-edge-utf8-flagged | DONE | 2 | 0 | 0 | 0 | 200 | UTF-8-flagged names correct | PASS |
| 21 | 21-edge-cp437-names | DONE | 1 | 0 | 0 | 0 | 200 | CP437 names not mojibake | PASS |
| 22 | 22-edge-mixed-file-types | DONE | 4 | 1 | 0 | 4 | 200 | png/jpg/txt/corrupt-pdf import; zip/bin/bare-json + orphan sidecar rejected; 0-byte pdf fails | PASS |
| 23b | 23b-edge-folders-only | DONE | 0 | 0 | 0 | 0 | 200 | 5 folders, 0 docs | PASS |
| 23 | 23-edge-empty | DONE | 0 | 0 | 0 | 0 | 200 | 0 docs, DONE | PASS |
| 24 | 24-edge-deep-nesting | DONE | 11 | 0 | 0 | 0 | 200 | 11 docs; deep path walk | PASS |
| 25 | 25-edge-path-traversal | DONE | 2 | 4 | 0 | 0 | 200 | Zip-Slip contained: traversal entries not written outside jobdir | PASS |
| 28 | 28-av-metadata-infected | DONE | 1 | 0 | 0 | 1 | 200 | infected sidecar → doc rejected; clean doc imports | PASS |
| 29 | 29-av-scan-timeout | DONE | 2 | 0 | 0 | 0 | 200 | failedFiles on AV timeout — needs fault injection | NOTE |
| 30 | 30-av-scan-technical-error | DONE | 2 | 0 | 0 | 0 | 200 | failedFiles on AV error — needs fault injection | NOTE |
| 31 | 31-edge-filetype-disallowed | DONE | 1 | 0 | 0 | 6 | 200 | disallowed types → rejected; valid sibling imports | PASS |
| 32 | 32-edge-filetype-boundaries | DONE | 2 | 0 | 0 | 2 | 200 | `.PDF` & `*.exe.pdf` import; extensionless & `*.pdf.exe` rejected | PASS |
| 34 | 34-edge-reimport-changed-body | DONE | 4 | 0 | 0 | 0 | 200 | own name → imports fresh (skip half → 34R) | NOTE |
| 35 | 35-edge-reimport-renamed-zip | DONE | 4 | 0 | 0 | 0 | 200 | different zip name → not deduped, imported again | PASS |
| 36a | 36a-edge-collision-setA | DONE | 2 | 0 | 0 | 0 | 200 | setA imports (2) | PASS |
| 36b | 36b-edge-collision-setB | DONE | 2 | 0 | 0 | 0 | 200 | distinct name → not deduped (collision half → 36R) | NOTE |
| 37 | 37-concurrency-mixed | DONE | 50 | 0 | 0 | 5 | 200 | 50 imported + 5 rejected under concurrency; buckets consistent | PASS |
| 39c | 39c-precedence-av-infected | DONE | 0 | 0 | 0 | 1 | 200 | infected sidecar wins → doc rejected | PASS |
| 40 | 40-happy-root-only-file | DONE | 1 | 0 | 0 | 0 | 200 | 1 root doc | PASS |
| 41 | 41-edge-folder-create-failure | DONE | 3 | 0 | 0 | 0 | 200 | v3: **no root orphan** (docs in their folders, or `failedFiles`) | **FAIL AS EXPECTED** |
| — | 15R-reupload-same-name | DONE | 0 | 0 | 4 | 0 | 200 | re-upload same name → all 4 SKIPPED (ALREADY_IMPORTED) | PASS |
| — | 34R-as-15-changed-body | DONE | 0 | 0 | 4 | 0 | 200 | changed bytes, same name+paths → all 4 SKIPPED (content ignored) | PASS |
| — | 36R-b-as-a-collision | DONE | 1 | 0 | 1 | 0 | 200 | setB as setA's name → overlap `january 001` skipped, `march 003` imported | PASS |

%% ai-graph-start %%

**Related notes:**
- [[ePost Zip-Import - dev test-suite results - 18-08-2026]]
- [[ePost ZIP-import — dev test-suite results - 13-08-2026]]
- [[ePost ZIP Import Test Fixture Matrix]]
- [[LUZ-158230 ePost ZIP Import Test Fixture Matrix (Confluence)]]
- [[LUZ-158230 eArchive Health ZIP import - golden test fixture matrix location]]

%% ai-graph-end %%