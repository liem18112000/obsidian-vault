---
title: "ePost ZIP Import Test Fixture Matrix"
created: 2026-08-14
updated: 2026-08-14
type: source
status: reference
source: "Confluence · TK - Team Kepler"
url: https://axonivy.atlassian.net/wiki/spaces/TK/pages/49665769474/ePost+ZIP+Import+Test+Fixture+Matrix
confluence_id: "49665769474"
confluence_path: "Team Kepler > Developer note"
tags: [confluence, epost]
---

# ePost ZIP Import Test Fixture Matrix

*Confluence source · Team Kepler › Developer note · [view original](https://axonivy.atlassian.net/wiki/spaces/TK/pages/49665769474/ePost+ZIP+Import+Test+Fixture+Matrix) · updated 2026-08-14*

### Suggested run order

1.  **01** — smoke test; if this fails nothing else is meaningful.

2.  **18, 19, 20, 20b, 26, 27** — synchronous rejections (validator + request shape), fastest feedback, no job created.

3.  **02, 03, 04, 06, 07, 16, 17, 24, 23, 23b** — structure & encoding against existing code.

4.  **05, 09, 10, 11, 12, 13, 14** — sidecar metadata (now live).

5.  **31, 32, 33, 22** — file-type allow-list & per-document size.

6.  **28, 29, 30** — antivirus (needs AV service/stub); note the document-not-scanned gap.

7.  **15, 15b, 34, 35, 36** — duplicates & idempotency (15 run twice).

8.  **39, 40/notifications** — precedence & notification bodies.

9.  **25** — security, before anything ships.

10. **37 (a–g), 38** — concurrency/flush tuning and stale timeout (env set before class-load).

11. **08** — volume last; watch `unprocessedFiles` draining and progress-flush behavior.

For each run assert on the job document (`GET {tenant-id}/import-jobs/{id}`): final `status`, `failureCode` where relevant, and the `successfulFiles` / `skippedFiles` / `rejectedFiles` / `failedFiles` / `unprocessedFiles` contents against the tables above — and the notification body per the variants section.

[[zip-data.zip|zip-data.zip]]

### Happy path

|  |  |  |  |
|----|----|----|----|
| \# | Fixture | Spec rule | Expected result |
| 01 | `01-happy-spec-example.zip` | §2 example verbatim — 2 folders, 3 docs, 1 sidecar each | 2 folders created, 3 documents imported **with metadata applied**, 0 failed |
| 02 | `02-happy-nested-folders.zip` | §2 nested folders recreated 1:1 (6 folders, depth 3) | full tree recreated, 12 documents imported |
| 03 | `03-happy-root-and-folders.zip` | documents at the ZIP root as well as in folders | 4 docs into folders, 2 into the tenant root |
| 04 | `04-happy-no-metadata.zip` | §6 document without metadata → imported without health metadata | 4 documents imported, title falls back to the file name |
| 05 | `05-happy-mixed-metadata.zip` | per-document optionality of the sidecar | 2 docs with metadata, 2 without, all imported (**now live, not expected-fail**) |
| 06 | `06-happy-os-noise-ignored.zip` | §2 `__MACOSX/`, `.DS_Store`, `Thumbs.db` ignored | 4 documents imported, noise neither imported nor failed |
| 07 | `07-happy-empty-folders.zip` | folders without documents | 5 folders created, 2 documents imported |
| 08 | `08-happy-volume-500-docs.zip` | throughput / job progress | 500 documents, `unprocessedFiles` drains to 0, job ends `DONE`. Also a de-facto concurrency smoke test (see 37) |

### Metadata handling (sidecar — now implemented)

Only these top-level fields are applied (`HealthDocImporter.ALLOWED_FIELDS`): `documentReferenceDate`, `documentTypes`, `senderName`, `senderTenantId`, `senderCompanyId`, plus `healthData` and `documentTitle`. Everything else is silently ignored.

|  |  |  |  |
|----|----|----|----|
| \# | Fixture | Spec rule | Expected result |
| 09 | `09-edge-metadata-orphan.zip` | §3 metadata without a matching document is an error; §2 must sit in the document's folder | 2 documents imported; 3 orphan sidecars → `rejectedFiles` (`DETAIL_ORPHAN_METADATA`) — *note the bucket change from v1* |
| 10 | `10-edge-metadata-unparseable.zip` | §6 unparseable metadata → document imported, metadata ignored | all 9 documents imported, **0 failed**; each carries `ignoredReason` "not valid JSON". Truncated JSON, plain text, empty file, JSON array, scalar, `null`, trailing comma, latin-1, UTF-8 BOM |
| 11 | `11-edge-metadata-unknown-toplevel.zip` | §4/§6 disallowed top-level element silently ignored | 4 documents imported; only the 5 allowed fields + `healthData`/`documentTitle` are applied. `id`/`tenantId`/`_id`/`status`/… must **not** reach internal properties |
| 12 | `12-edge-metadata-optional-fields.zip` | §4 no field is required / individually schema-validated | all 12 documents imported. `{}`, title-only, healthData-only, `en`-only coded values, wrong types, invalid date `2026-13-45`, nulls, unknown document types |
| 13 | `13-edge-metadata-naming.zip` | §3 `.metadata.json` appended to the **complete** file name, exact lowercase postfix | only `correct 001.pdf` binds its sidecar. `.METADATA.JSON`, `.Metadata.Json`, base-name-without-`.pdf`, `.meta.json`, `.metadata`, case-mismatched extension → **orphans →** `rejectedFiles`; their documents import without metadata |
| 14 | `14-edge-metadata-oversized.zip` | §5 metadata file size limit | boundary is `BUFFER_SIZE` **= 100 KiB (102 400 B)**: `<=` accepted, `>` → sidecar **ignored** (document still imported, `ignoredReason` set). Include 100 KiB exact, 100 KiB+1, 5 MB, and a deeply-nested object that must not blow the parser |

### Antivirus (JSON Metadata scan, `AntiviusScanningService`)

Each document that **has a sidecar** triggers an AV scan **of the sidecar JSON** before the document is created. Generate sidecars containing the EICAR test string `X5O!P%@AP[4\PZX54(P^)7CC)7}$EICAR-STANDARD-ANTIVIRUS-TEST-FILE!$H+H*`. Requires the AV service reachable (or a stub returning the given `ScanningResult` / throwing the given error).

|  |  |  |  |
|----|----|----|----|
| \# | Fixture | Trigger | Expected result |
| 28 | `28-av-metadata-infected.zip` | sidecar bytes contain EICAR → `ScanningResult != OK` | the **document** → `rejectedFiles` (`DETAIL_METADATA_INFECTED` "Virus found in the metadata file"); document **not** created |
| 29 | `29-av-scan-timeout.zip` | AV endpoint slow → `SocketTimeoutException`/`TimeoutException`/message "timed out" | document → `failedFiles` (`DETAIL_METADATA_SCAN_TIMEOUT` "…timed out after 30s") |
| 30 | `30-av-scan-technical-error.zip` | AV endpoint down / non-OK / any other exception | document → `failedFiles` (`DETAIL_METADATA_SCAN_ERROR` "Cannot scan virus for file metadata") |

> ⚠️ **Coverage gap to flag, not a fixture:** only the **sidecar** is scanned. The **document binary is never AV-scanned**, and a document **without a sidecar is never scanned at all**. An infected `*.pdf` with a clean/absent sidecar imports successfully. Confirm with the spec owner whether that is intended before relying on AV here.

### File-type allow-list (NEW — `FileTypeValidator`)

Allowed extensions (case-insensitive, **extension only — no magic-byte sniffing**): `pdf, doc, docx, odt, ods, odp, rtf, jfif, png, odf, xls, xlsx, gif, jpg, jpeg, jpe, bmp, txt` (plus any `*.metadata.json`). Anything else → `rejectedFiles` (`DETAIL_FILE_TYPE_NOT_ALLOWED`).

|  |  |  |  |
|----|----|----|----|
| \# | Fixture | Case | Expected result |
| 31 | `31-edge-filetype-disallowed.zip` | `.exe`, `.zip`, `.bin`, a **bare** `.json` document, `.svg`, `.heic` | each → `rejectedFiles` (type not allowed); any valid-type sibling still imports |
| 32 | `32-edge-filetype-boundaries.zip` | extensionless file; UPPERCASE `.PDF`; double-extension `report.pdf.exe` and `payload.exe.pdf` | extensionless → rejected; `.PDF` → **imported** (lower-cased); `*.pdf.exe` → rejected; `*.exe.pdf` **→ imported** — spoof risk, extension-only check. Document the spoof as a known limitation |

### Per-document size (NEW)

|  |  |  |  |
|----|----|----|----|
| \# | Fixture | Rule | Expected result |
| 33 | `33-edge-document-oversize.zip` | `length() > MAX_DOCUMENT_FILE_SIZE` = **200 MiB (209 715 200 B)** | exactly 200 MiB → imported; 200 MiB+1 → `rejectedFiles` (`DETAIL_FILE_TOO_LARGE` "…maximum allowed size of 200 MB"). Store the big entry uncompressed so the archive doesn't trip the zip-bomb / 2 GiB zip guards first |

### Duplicates & idempotency (`IdempotentImportService`)

Dedupe key = `importZipName` (zip file name minus extension) **+ each file's relative path**, looked up in prior jobs' `successfulFiles` + `skippedFiles`. **Content and size are not compared.**

|  |  |  |  |
|----|----|----|----|
| \# | Fixture | Case | Expected result |
| 15 | `15-edge-duplicates-upload-twice.zip` | **upload the same-named zip twice** | 1st run imports; 2nd run: every path already recorded → `skippedFiles` (`DETAIL_ALREADY_IMPORTED`). *v1's folder-*`isNewCreated` *note no longer applies* |
| 15b | `15b-edge-duplicate-entries-in-zip.zip` | same entry path twice inside one archive | must not crash; job completes with a defined outcome |
| 34 | reuse 15 with a **modified body** at an already-imported path | same zip name, same path, **different bytes** | still **skipped** — path-based idempotency ignores content. Assert this is intended (silent skip of changed content) |
| 35 | `35-edge-reimport-renamed-zip.zip` | **different zip name**, identical contents | **not** deduped → all documents imported again (idempotency is per `importZipName`) |
| 36 | two different document sets shipped as **same-named** zips | filename collision within one tenant | overlapping relative paths from set A are skipped when set B is imported — cross-contamination edge, assert expected behaviour |

### Ignored entries

|  |  |  |  |
|----|----|----|----|
| \# | Fixture | Spec rule | Expected result |
| 16 | `16-edge-dot-entries-and-artifacts.zip` | §2 names starting with a dot are ignored | exactly **1** document imported (`Visible/real document 001.pdf`); `.hidden*`, `.gitignore`, `.DS_Store`, `Thumbs.db`, `.hidden folder/`, `__MACOSX/` ignored, none failed |

### Encoding & names

|  |  |  |  |
|----|----|----|----|
| \# | Fixture | Spec rule | Expected result |
| 17 | `17-edge-utf8-names.zip` | §2/§5 UTF-8 names | 9 documents; umlauts, accents, Cyrillic, Japanese, emoji, `#()%+,;&`, edge spaces and a 121-char segment survive round-trip |
| 21 | `21-edge-cp437-names.zip` | Windows ZIP, UTF-8 flag not set | names must not be mojibake |
| 21b | `21b-edge-macos-created.zip` | macOS ZIP with `__MACOSX` (UTF-8 re-read path) | extended chars survive, `__MACOSX` ignored |
| 21c | `21c-edge-utf8-flagged.zip` | ZIP with UTF-8 name flag set | names correct |

### Archive-level rejection — synchronous (`DocsImportValidator`, expect HTTP 4xx, **no** job created)

|  |  |  |  |
|----|----|----|----|
| \# | Fixture | Spec rule | Expected result |
| 18 | `18-edge-password-protected.zip` | §5 password-protected rejected (pw `123456`) | rejected at upload. Note: a `storage_fail` notification is still pushed on a plain validation rejection |
| 19 | `19-edge-zip-bomb.zip` | bomb guard: uncompressed `> 50×` compressed | 120 MB from 120 KB ≈ 1000× → rejected |
| 19b | `19b-edge-zip-bomb-fixture.zip` | repo bomb fixture, ~5 GB uncompressed | rejected (also trips `> 4 GiB`) |
| 20 | `20-edge-not-a-zip.zip` | §5 standard ZIP | a PDF renamed `.zip` → `ZipException` → "must be zip" 400 |
| 20b | `20b-edge-truncated-zip.zip` | corrupt archive, central directory missing | **either** synchronous 400 (validator `ZipException`) **or** created job → `FAILED` / `failureCode=INVALID` at extraction. Record which; must not hang or half-import |
| 23 | `23-edge-empty.zip` | valid ZIP, zero entries | job completes with 0 documents, status `DONE` |
| 23b | `23b-edge-folders-only.zip` | folders but no documents | 5 folders created, 0 documents |
| 26 | `26-edge-over-2gb.zip` (generate) | §5 zip `>= 2 GiB` (`Constants.MAX_ZIP_FILE_SIZE`) | rejected at upload. Generate with stored (1:1) random data so it trips the size limit, not the bomb check |

```
python -c "
import zipfile, os
with zipfile.ZipFile('26-edge-over-2gb.zip','w',zipfile.ZIP_STORED) as z:
    with z.open('big.bin','w') as f:
        for _ in range(2100): f.write(os.urandom(1024*1024))
"
```

### Request-level rejection — multipart part count (synchronous, **no** job created)

Not a ZIP fixture — construct the multipart request by hand. Send two or more parts **named** `file` (reuse any existing fixtures, e.g. `01-happy-spec-example.zip` + `02-happy-nested-folders.zip`).

|  |  |  |  |
|----|----|----|----|
| \# | Fixture | Rule | Expected result |
| 27 | two `file` parts (e.g. `01` + `02`) | implementation guard — not in the spec (one request = one ZIP) | **HTTP 400** "Only one zip file can be imported per request"; no job, nothing written to the upload folder. Note: `importZipFile`'s catch still fires `pushNotificationAfterImportHasException`, so a notification is pushed on this pure request-shape rejection. **Field-name caveat:** the guard counts only parts named `file`; extra parts under a *different* name are invisible (`size()==1`) and silently dropped |

### Concurrency & flush tuning ( `ImportDocumentExecutor`, `JobProgressWriter`, `EnvConfig`)

Documents are processed on `Executors.newFixedThreadPool(IMPORT_CONCURRENCY)`; job progress is persisted by `JobProgressWriter`, throttled by `FLUSH_EVERY_N` docs / `FLUSH_EVERY_MS` ms, with a final `flushNow()`.

> **Testability gotcha:** `IMPORT_CONCURRENCY`, `FLUSH_EVERY_N`, `FLUSH_EVERY_MS` are `static final`, read **once at class-load** via `EnvConfig`. Set the env var **before** the class loads (fresh classloader / new JVM) — a `@BeforeEach` `setenv` runs too late and silently uses the already-frozen value.

|  |  |  |  |
|----|----|----|----|
| \# | Case | Env | Expected result |
| 37a | serialized | `IMPORT_CONCURRENCY=1` | all docs processed on one thread; results identical to concurrent run, no lost/dup entries |
| 37b | clamp high | `IMPORT_CONCURRENCY=999` | clamped to **64** (`IMPORT_CONCURRENCY_MAX`) |
| 37c | clamp low / invalid | `=0`, `=-4`, `=abc` | `0`/`-4` clamped to **1**; `abc` → default **16** (logged), then clamped |
| 37d | flush every doc | `FLUSH_EVERY_N=1` | job doc persists after each file; a mid-import `GET` reflects near-real-time `unprocessedFiles` |
| 37e | rare flush | `FLUSH_EVERY_N=1000`, `FLUSH_EVERY_MS=60000` | intermediate progress may lag; **final** `flushNow()` **must leave** `unprocessedFiles=0` and the final bucket counts correct |
| 37f | always flush | `FLUSH_EVERY_MS=0` | flush on every record; correctness unchanged, higher write volume |
| 37g | two concurrent uploads (same tenant) | default | each `run()` gets its own pool (≤64 threads each); both jobs finish with correct, non-interleaved buckets. Watch idempotency: both may read "already-imported" before either writes |

Use the 500-doc fixture (08) — with a deliberate mix of success/reject/fail (combine 22/28/31/33 entries) — to stress the shared `job` lists under concurrency (all mutations are serialized on the `JobProgressWriter` monitor; this test guards that invariant).

### Stale-job timeout (NEW — `failJobIfStale`)

|  |  |  |  |
|----|----|----|----|
| \# | Case | Trigger | Expected result |
| 38 | stuck job | a job left in `DOCUMENT_CREATING` older than **3600 s** (`isStale`), then `GET /import-jobs/{id}` | lazily flipped to `status=FAILED`, `failureCode=TIMEOUT`. Simulate by back-dating `START_TIME` in the stored job |

### Structure & security

|  |  |  |  |
|----|----|----|----|
| \# | Fixture | Spec rule | Expected result |
| 22 | `22-edge-mixed-file-types.zip` | the spec doesn't restrict document type, **but** `FileTypeValidator` **does** | PNG, JPG, TXT, a 0-byte PDF, a corrupt PDF → imported; **ZIP, BIN, and a bare** `.json` **document →** `rejectedFiles` (type not allowed); trailing orphan sidecar → `rejectedFiles`. *Corrected from v1's "all imported"* |
| 24 | `24-edge-deep-nesting.zip` | nested folders recreated 1:1 | 10 levels + ~8-segment long path; 11 documents. Watch the `File.separator` path walk (Windows vs Linux container) |
| 25 | `25-edge-path-traversal.zip` | **security probe**, not in the spec | `../escaped.pdf`, `../../escaped deeper.pdf`, `Safe/../../sideways.pdf`, `/absolute.pdf` must **not** be written outside `<UPLOAD_FOLDER>/<jobId>/`. Anything escaping is a Zip Slip finding |

### Rejection precedence (NEW — order-dependent detail)

`processDocumentFile` checks in a fixed order: **metadata/orphan → file-type → size → AV(sidecar) → create**. When a file trips more than one rule, the **first** one wins the reported detail.

|  |  |  |
|----|----|----|
| \# | Case | Expected `detail` |
| 39a | disallowed type **and** \> 200 MiB | `DETAIL_FILE_TYPE_NOT_ALLOWED` (type checked before size) |
| 39b | allowed type, \> 200 MiB, **and** infected sidecar | `DETAIL_FILE_TOO_LARGE` (size checked before AV) — document rejected before the sidecar is ever scanned |
| 39c | allowed type, ≤ 200 MiB, infected sidecar | `DETAIL_METADATA_INFECTED` → `rejectedFiles` |
