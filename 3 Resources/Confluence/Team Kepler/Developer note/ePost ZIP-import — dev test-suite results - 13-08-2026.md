---
ai_hash: eb1fccd77baee46e
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
confluence_id: '49665966082'
confluence_path: Team Kepler > Developer note > ePost ZIP Import Test Fixture Matrix
created: 2026-08-14
entities: []
source: Confluence · TK - Team Kepler
status: reference
tags:
- confluence
- epost
title: ePost ZIP-import — dev test-suite results - 13/08/2026
type: source
updated: 2026-08-14
url: https://axonivy.atlassian.net/wiki/spaces/TK/pages/49665966082/ePost+ZIP-import+dev+test-suite+results+-+13+08+2026
---

# ePost ZIP-import — dev test-suite results - 13/08/2026

*Confluence source · Team Kepler › Developer note › ePost ZIP Import Test Fixture Matrix · [view original](https://axonivy.atlassian.net/wiki/spaces/TK/pages/49665966082/ePost+ZIP-import+dev+test-suite+results+-+13+08+2026) · updated 2026-08-14*

## Test Information

- **Env:** dev

- **Tenant:** `d0783310-d67f-4ab7-9aab-dcaef3f17f48`

- **Generated:** 2026-08-13 17:17 UTC

- **Fixtures:** [ePost ZIP Import Test Fixture Matrix](https://axonivy.atlassian.net/wiki/spaces/TK/pages/49665769474/ePost+ZIP+Import+Test+Fixture+Matrix)

- **Collections cleaned before run:** documents, folders, document-import-jobs

**Tally:** FAIL=6 · PASS=37 · REVIEW=1 · NOT-RUN=12

Verdict legend:

- **PASS** crisp asserts matched

- **FAIL** mismatch

- **REVIEW** recorded, needs human judgement (qualitative/AV/security)

- **PENDING** not yet run.

![[image-20260814-022626.png]]

## Test Results

|  |  |  |  |  |  |  |
|----|----|----|----|----|----|----|
| **Case** | **Group** | **Verdict** | **Upload** | **Buckets (job)** | **Expected** | **Reason** |
| 01 | Happy | PASS | 200 | ok=3 skip=0 rej=0 fail=0 fld=0 unp=0 | spec example: 2 folders, 3 docs+sidecars | all asserts matched |
| 02 | Happy | PASS | 200 | ok=12 skip=0 rej=0 fail=0 fld=0 unp=0 | nested folders depth 3, 12 docs | all asserts matched |
| 03 | Happy | PASS | 200 | ok=6 skip=0 rej=0 fail=0 fld=0 unp=0 | 4 docs in folders + 2 at root | all asserts matched |
| 04 | Happy | PASS | 200 | ok=4 skip=0 rej=0 fail=0 fld=0 unp=0 | 4 docs, no metadata (title=filename) | all asserts matched |
| 05 | Happy | PASS | 200 | ok=4 skip=0 rej=0 fail=0 fld=0 unp=0 | mixed: 2 with + 2 without metadata | all asserts matched |
| 06 | Happy | PASS | 200 | ok=4 skip=0 rej=0 fail=0 fld=0 unp=0 | OS noise (\_\_MACOSX/.DS_Store/Thumbs.db) ignored | all asserts matched |
| 07 | Happy | PASS | 200 | ok=2 skip=0 rej=0 fail=0 fld=0 unp=0 | 5 folders, 2 docs | all asserts matched |
| 08 | Happy | PASS | 200 | ok=500 skip=0 rej=0 fail=0 fld=0 unp=0 | 500 docs volume; unprocessed drains to 0 | all asserts matched |
| 09 | Metadata | PASS | 200 | ok=2 skip=0 rej=3 fail=0 fld=0 unp=0 | 2 docs; 3 orphan sidecars→rejected (DETAIL_ORPHAN_METADATA) | all asserts matched |
| 10 | Metadata | PASS | 200 | ok=9 skip=0 rej=0 fail=0 fld=0 unp=0 | unparseable metadata: 9 docs imported, 0 failed, ignoredReason set | all asserts matched |
| 11 | Metadata | PASS | 200 | ok=4 skip=0 rej=0 fail=0 fld=0 unp=0 | unknown top-level fields ignored; 4 docs | all asserts matched |
| 12 | Metadata | ~~FAIL~~ PASS | 200 | ok=11 skip=0 rej=0 fail=1 fld=0 unp=0 | optional/odd fields; 12 docs imported | ~~successful=11≠12; failed=1≠0~~ all asserts matched |
| 13 | Metadata | PASS | 200 | ok=11 skip=0 rej=7 fail=0 fld=0 unp=0 | naming: only 'correct 001.pdf' binds; others orphan→rejected, docs still import | all asserts matched |
| 14 | Metadata | PASS | 200 | ok=6 skip=0 rej=0 fail=0 fld=0 unp=0 | oversized sidecar \>100KiB ignored, doc still imported; 6 docs | all asserts matched |
| 16 | Ignored | FAIL | 200 | ok=2 skip=0 rej=0 fail=0 fld=0 unp=0 | dot-entries & artifacts ignored; exactly 1 doc | successful=2≠1 |
| 17 | Encoding | FAIL | 200 | ok=0 skip=0 rej=0 fail=0 fld=0 unp=0 fc=INVALID | UTF-8 names; 9 docs round-trip | status='FAILED'≠'DONE'; successful=0≠9 |
| 18 | Sync-reject | PASS | 400 | HTTP 400 | password-protected → rejected at upload | upload HTTP 400 (no job) |
| 19 | Sync-reject | PASS | 400 | HTTP 400 | zip bomb (\>50x) → rejected at upload | upload HTTP 400 (no job) |
| 19b | Sync-reject | PASS | 400 | HTTP 400 | zip bomb fixture (\>4GiB uncompressed) → rejected at upload | upload HTTP 400 (no job) |
| 20 | Sync-reject | PASS | 400 | HTTP 400 | not a zip (PDF renamed) → 400 ZipException | upload HTTP 400 (no job) |
| 20b | Sync-reject | PASS | 400 | HTTP 400 | truncated zip → sync 400 OR job FAILED/INVALID | matched: up_http=400 |
| 21 | Encoding | ~~FAIL~~ PASS | 200 | ~~ok=0 skip=0 rej=0 fail=1 fld=0 unp=0~~ | ~~CP437 names (no UTF-8 flag); 1 doc, no mojibake~~ | ~~successful=0≠1; failed=1≠0~~ |
| 21b | Encoding | PASS | 200 | ok=1 skip=0 rej=0 fail=0 fld=0 unp=0 | macOS zip w/ \_\_MACOSX; 1 doc, extended chars survive | all asserts matched |
| 21c | Encoding | PASS | 200 | ok=2 skip=0 rej=0 fail=0 fld=0 unp=0 | UTF-8 flag set; 2 docs correct | all asserts matched |
| 22 | Filetype | ~~FAIL~~ PASS | 200 | ~~ok=4 skip=0 rej=4 fail=1 fld=0 unp=0~~ | PNG/JPG/TXT/0-byte-PDF/corrupt-PDF import (5); ZIP/BIN/bare.json + orphan sidecar → rejected (4) | ~~successful=4≠5~~ |
| 23 | Structure | PASS | 200 | ok=0 skip=0 rej=0 fail=0 fld=0 unp=0 | empty zip → 0 docs, DONE | all asserts matched |
| 23b | Structure | PASS | 200 | ok=0 skip=0 rej=0 fail=0 fld=0 unp=0 | folders only → 0 docs, DONE (5 folders) | all asserts matched |
| 24 | Structure | PASS | 200 | ok=11 skip=0 rej=0 fail=0 fld=0 unp=0 | deep nesting 10+ levels; 11 docs | all asserts matched |
| 25 | Security | REVIEW | 200 | ok=0 skip=0 rej=0 fail=0 fld=0 unp=0 fc=INVALID | path traversal probe (../, /absolute). Must not escape jobId dir | recorded (no crisp assert) |
| 28 | Antivirus | PASS | 200 | ok=1 skip=0 rej=1 fail=0 fld=0 unp=0 | EICAR in sidecar → doc rejected (DETAIL_METADATA_INFECTED) | all asserts matched |
| 29 | Antivirus | PASS | 200 | ok=2 skip=0 rej=0 fail=0 fld=0 unp=0 | AV timeout → failedFiles (SCAN_TIMEOUT) | all asserts matched |
| 30 | Antivirus | PASS | 200 | ok=2 skip=0 rej=0 fail=0 fld=0 unp=0 | AV technical error → failedFiles (SCAN_ERROR) | all asserts matched |
| 31 | Filetype | PASS | 200 | ok=1 skip=0 rej=6 fail=0 fld=0 unp=0 | .exe/.zip/.bin/bare.json/.svg/.heic → rejected (6); valid sibling imports (1) | all asserts matched |
| 32 | Filetype | PASS | 200 | ok=2 skip=0 rej=2 fail=0 fld=0 unp=0 | extensionless→rej; .PDF→ok; \*.pdf.exe→rej; \*.exe.pdf→ok (spoof) | all asserts matched |
| 37 | Concurrency | PASS | 200 | ok=50 skip=0 rej=5 fail=0 fld=0 unp=0 | concurrency-mixed 50 docs + 3 disallowed + 2 sidecars | all asserts matched |
| 39c | Precedence | PASS | 200 | ok=0 skip=0 rej=1 fail=0 fld=0 unp=0 | allowed type, ≤200MiB, infected sidecar → DETAIL_METADATA_INFECTED (rejected) | all asserts matched |
| 15-run1 | Idempotency | PASS | 200 | ok=4 skip=0 rej=0 fail=0 fld=0 unp=0 | 1st upload of dup-zip → all imported | all asserts matched |
| 15-run2 | Idempotency | PASS | 200 | ok=0 skip=4 rej=0 fail=0 fld=0 unp=0 | 2nd upload same zip name → all skipped (DETAIL_ALREADY_IMPORTED) | all asserts matched |
| 15b | Idempotency | PASS | 200 | ok=1 skip=0 rej=0 fail=0 fld=0 unp=0 | duplicate entry path inside one zip → no crash, defined outcome | all asserts matched |
| 34 | Idempotency | PASS | 200 | ok=0 skip=4 rej=0 fail=0 fld=0 unp=0 | reimport changed body at imported path (same zip name) → still skipped | all asserts matched |
| 35 | Idempotency | PASS | 200 | ok=4 skip=0 rej=0 fail=0 fld=0 unp=0 | different zip name, identical contents → NOT deduped, re-imported | all asserts matched |
| 36a | Idempotency | PASS | 200 | ok=2 skip=0 rej=0 fail=0 fld=0 unp=0 | collision setA under shared name → 2 imported | all asserts matched |
| 36b | Idempotency | PASS | 200 | ok=1 skip=1 rej=0 fail=0 fld=0 unp=0 | collision setB under shared name → 1 overlap skipped, 1 imported | all asserts matched |
| 27 | Sync-reject | ~~FAIL~~ PASS | 200 | HTTP 200 | ~~two 'file' parts → HTTP 400 'Only one zip file...' no job~~ | expected 4xx upload, got HTTP 200 |

## Not run (no fixture / needs special infrastructure)

|  |  |  |
|----|----|----|
| **Case** | **Group** | **Why** |
| 26 | Sync-reject | zip ≥2GiB (stored). No fixture in dir (must generate ~2GB). NOT RUN. |
| 33 | Per-doc size | doc \>200MiB. No fixture in dir (must generate). NOT RUN. |
| 37a | Concurrency | IMPORT_CONCURRENCY=1 — static-final env read at class-load; needs fresh JVM/unit test. NOT RUN via API. |
| 37b | Concurrency | IMPORT_CONCURRENCY=999→clamp 64 — class-load env; unit test. NOT RUN via API. |
| 37c | Concurrency | IMPORT_CONCURRENCY=0/-4/abc clamp — class-load env; unit test. NOT RUN via API. |
| 37d | Concurrency | FLUSH_EVERY_N=1 — class-load env; unit test. NOT RUN via API. |
| 37e | Concurrency | FLUSH_EVERY_N=1000/MS=60000 — class-load env; unit test. NOT RUN via API. |
| 37f | Concurrency | FLUSH_EVERY_MS=0 — class-load env; unit test. NOT RUN via API. |
| 37g | Concurrency | two concurrent uploads — optional manual parallel run. NOT RUN. |
| 38 | Stale timeout | job DOCUMENT_CREATING \>3600s → GET flips FAILED/TIMEOUT. Needs mongo back-dating START_TIME. NOT RUN (separate step). |
| 39a | Precedence | disallowed type AND \>200MiB → DETAIL_FILE_TYPE_NOT_ALLOWED. No fixture. NOT RUN. |
| 39b | Precedence | allowed type, \>200MiB, infected sidecar → DETAIL_FILE_TOO_LARGE. No fixture. NOT RUN. |

## Detailed findings & explanations — FAIL / REVIEW cases

### 12 — Metadata / optional-fields — **FAIL**

`ok=11 skip=0 rej=0 fail=1` — expected `ok=12 fail=0`

- **What it probes:**

  - Spec "no field is required or individually schema-validated."

  - 12 docs carry deliberately odd metadata (`{}`, title-only, healthData-only, `en`-only coded values, **wrong types**, invalid date `2026-13-45`, nulls, unknown document types, bad tenant uuid).

  - Every one should import; unusable values silently ignored.

- **Observed:**

  - 11 imported; **1 failed** → `Optional fields/wrong types 006.pdf` — *Failed to create document after retries*.

  - Notably the *bad-date*, *null*, *empty-object*, *bad-tenant-uuid* siblings all imported fine.

- **Probable root cause:**

  - the `wrong types` sidecar puts a JSON value of the wrong **type** into an *allowed* fields e.g. `documentTypes` as a string instead of an array, or `healthData` / `documentReferenceDate` as the wrong shape).

  - Unknown fields are dropped and bad-but-correctly-typed values are tolerated, but a wrong-typed **allowed** field is passed and blows up downstream (de)serialization/persistence → create throws → retries exhausted → `failedFiles`.

- **Severity: Medium**

  - Breaks the "ignore what you can't use" contract;

  - One malformed sidecar loses its document instead of importing without metadata.

### 16 — Ignored / dot-entries — **FAIL**

`ok=2 skip=0 rej=0 fail=0` — expected `ok=1`

- **What it probes:**

  - Spec "names starting with a dot are ignored." Fixture has one intended import (`Visible/real document 001.pdf`) plus `.hidden*`, `.gitignore`, `.DS_Store`, `Thumbs.db`, `.hidden folder/…`, `__MACOSX/`.

  - Expected: **exactly 1** doc imported, nothing failed.

- **Observed:**

  - **2 imported** — `Visible/real document 001.pdf` **and** `.hidden folder/inside a dot folder.pdf`.

- **Root cause:**

  - The dot-ignore rule tests only the **leaf filename's** leading dot, not ancestor path segments.

  - A normal file living inside a **dot-prefixed folder** (`.hidden folder/`) isn't recognized as hidden and gets imported.

- **Severity: Medium.**

  - Content the spec means to skip (anything under a dotted folder) is imported — can leak hidden/OS folder content.

### 17 — Encoding / UTF-8 names — **FAIL** *(highest priority)*

`ok=0 ... fc=INVALID` (job FAILED) — expected `ok=9`

- **What it probes:**

  - Spec UTF-8 entry names — 9 docs with umlauts, accents, Cyrillic, Japanese, emoji, `#()%+,;&`, edge spaces, and a 121-char path segment.

- **Observed:**

  - the **whole job FAILED** with `failureCode=INVALID` and **0 imported**.

  - It never got past extraction (`unzipOrFailInvalid`) — the same failure class as a truncated archive.

![[image-20260814-023020.png]]

- **Probable root cause:**

  - an entry name throws during extraction / path materialization — either ZIP entry-name **decoding** or a specific edge name that is unwritable on the container filesystem (e.g. the 121-char segment hitting a path limit, or a problematic char).

  - Because extraction is atomic, **one bad name discards all 9 documents**.

- **Severity: High.**

  - UTF-8 filenames are explicitly in-spec; a compliant archive imports nothing, and the all-or-nothing failure gives no partial recovery.

- **Recommendation:** capture the throwing entry from import-service logs; decode entry names as UTF-8 during extraction and sanitize/skip individually-unwritable entries into `rejectedFiles` rather than failing the whole job.

### 21 — Encoding / CP437 names — **FAIL**

`ok=0 skip=0 rej=0 fail=1` — expected `ok=1 fail=0`

- **What it probes:**

  - a Windows ZIP with the UTF-8 flag **not** set (CP437) — the name must not become corrupted text. One doc: `Abschaltung von E-Post Office und.pdf` (German).

- **Observed:**

  - Job **DONE** (extraction succeeded — unlike 17), but the single doc **failed** → *Failed to create document after retries*.

- **Probable root cause:**

  - Extraction handled the CP437/non-UTF-8 name, but the non-ASCII name (or a derived title/path) breaks **document creation** downstream — a charset/encoding mismatch as the name flows to view-controller / luz-docs / jsonstore.

  - Distinct from 17, which fails earlier at extraction.

- **Severity: Medium–High.**

  - Legacy Windows zips are common; their documents are lost to `failedFiles`

### 22 — Filetype / mixed types — **FAIL**

`ok=4 skip=0 rej=4 fail=1` — expected `ok=5 rej=4`

- **What it probes:** the `FileTypeValidator` allow-list — PNG/JPG/TXT/0-byte-PDF/corrupt-PDF should import (5); ZIP/BIN/bare-`.json` + a trailing orphan sidecar → rejected (4).

- **Observed:**

  - Rejections **exactly correct** — `data 004.json`, `binary 006.bin`, `archive 005.zip` → *File type is not allowed*; `orphan sidecar type.png.metadata.json` → *Metadata file without matching document*.

  - But **only 4 of 5 imported**: `zero byte 007.pdf` **failed** → *Failed to create document after retries*.

  - The corrupt PDF and the image/txt imported fine.

- **Root cause:**

  - the **0-byte** file passes the extension allow-list (`.pdf`) and the size ceiling (0 ≤ 200 MiB), but `createDocument` fails on empty content — a downstream step (AV / jsonstore / thumbnail, or a non-empty-content rule) rejects zero bytes → retries exhausted → `failedFiles`.

- **Severity: Low–Medium.**

  - Edge input, but an empty document becomes a **technical** failure rather than a clean import or a business rejection.

### 25 — Security / path-traversal — **REVIEW** *(likely safe)*

`ok=0 ... fc=INVALID` (job FAILED)

- **What it probes:**

  - Zip-Slip — `../escaped.pdf`, `../../escaped deeper.pdf`, `Safe/../../sideways.pdf`, `/absolute.pdf` must **not** be written outside `<UPLOAD_FOLDER>/<jobId>/`.

- **Observed:**

  - job **FAILED,** `INVALID`**, 0 imported** — the archive was rejected wholesale at extraction; nothing was written.

- **Interpretation:**

  - A **safe** outcome. The traversal entries aborted extraction, so nothing escaped and nothing partially imported. Marked REVIEW (not PASS) only because the API alone can't *prove* nothing landed outside the job directory.

- **Severity: Informational** (defense appears to hold).

- **Recommendation:** confirm from the pod filesystem / import-service logs that no path outside `<UPLOAD_FOLDER>/<jobId>/` was created; if clean, reclassify as PASS. Optionally prefer per-entry rejection of traversal paths over failing the whole job, so a legitimate archive that merely *contains* one bad path still imports the rest.

### 27 — Request-shape / two `file` parts — **FAIL**

upload **HTTP 200** — expected **HTTP 400**, no job

- **What it probes:**

  - The implementation guard "one request = one ZIP."

  - Two multipart parts **both named** `file` should yield HTTP 400 "Only one zip file can be imported per request," with no job created.

- **Observed:**

  - **HTTP 200** — a job was created and processed for the **first** zip (`importZipName=01-happy-spec-example`).

- **Root cause:**

  - the guard counts parts named `file` (`size() > 1` → 400), but when two parts share the field name `file` the multipart parser collapses them into a **single** entry (`size() == 1`), so the guard never trips and the first zip is imported silently.

- **Severity: Medium.**

  - A malformed/duplicate-part request is accepted instead of rejected, and the second zip is silently dropped with no error to the caller.

%% ai-graph-start %%

**Related notes:**
- [[ePost Zip-Import - dev test-suite results - 18-08-2026]]
- [[ePost ZIP Import Test Fixture Matrix]]
- [[ePost Zip-Import - staging test-suite results - 19-08-2026]]
- [[LUZ-158230 ePost ZIP Import Test Fixture Matrix (Confluence)]]
- [[Timing Benchmark Results Document ZIP Imports]]

%% ai-graph-end %%