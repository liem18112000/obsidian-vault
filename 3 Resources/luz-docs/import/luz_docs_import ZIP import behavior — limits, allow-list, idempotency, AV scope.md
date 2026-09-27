---
ai_hash: a5e773abfd292c8a
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-22
entities: []
source: LUZ-158230 fixture matrix, session 2026-09-22
status: seedling
tags:
- luz-docs-import
- earchive
- zip-import
- testing
- gotcha
title: luz_docs_import ZIP import behavior — limits, allow-list, idempotency, AV scope
type: lesson
---

# luz_docs_import ZIP import behavior — limits, allow-list, idempotency, AV scope

The `luz_docs_import` service imports an eArchive `transfer.zip` (folders + per-document `<filename>.metadata.json` sidecars). Its validation behaviour has several load-bearing, non-obvious rules worth remembering when testing or extending it.

- **Metadata allow-list is silent**: only `HealthDocImporter.ALLOWED_FIELDS` are applied — `documentReferenceDate`, `documentTypes`, `senderName`, `senderTenantId`, `senderCompanyId`, plus `healthData` and `documentTitle`. Every other top-level key is silently ignored (so `id`/`_id`/`status` cannot poison internal properties).
- **File-type check is extension-only** (`FileTypeValidator`, no magic-byte sniffing). Case-insensitive allow-list. Consequence: `payload.exe.pdf` **imports** and `report.pdf.exe` is rejected — a known spoof limitation.
- **Idempotency is path-based** (`IdempotentImportService`): dedupe key = `importZipName` (zip name minus extension) + each file's relative path, matched against prior jobs' `successfulFiles`+`skippedFiles`. **Content/size are never compared** — re-uploading a same-named zip with changed bytes at an already-imported path is silently skipped. A renamed zip with identical contents re-imports.
- **AV scans only the sidecar JSON** (`AntiviusScanningService`), before the document is created. The document binary is **never** AV-scanned, and a document without a sidecar is never scanned at all — an infected `*.pdf` with a clean/absent sidecar imports.
- **Rejection precedence** in `processDocumentFile` is fixed: metadata/orphan → file-type → size → AV(sidecar) → create. The first rule tripped wins the reported `detail`.
- **Constants**: metadata size cap `BUFFER_SIZE` = 100 KiB (102400 B); `MAX_DOCUMENT_FILE_SIZE` = 200 MiB; `Constants.MAX_ZIP_FILE_SIZE` = 2 GiB; zip-bomb guard = uncompressed > 50x compressed; `IMPORT_CONCURRENCY_MAX` = 64; stale-job timeout 3600 s (`failJobIfStale`, lazily flips a stuck `DOCUMENT_CREATING` job to `FAILED`/`TIMEOUT` on GET).
- **Testability gotcha**: `IMPORT_CONCURRENCY`, `FLUSH_EVERY_N`, `FLUSH_EVERY_MS` are `static final`, read once at class-load via `EnvConfig`. A `@BeforeEach setenv` runs too late — set the env before the class loads (fresh classloader / new JVM) or the frozen value is used.

Workspace/repo for the codegraph: `axonivy-prod/luz_docs_import`.

## Related

- [[LUZ-158230 eArchive Health ZIP import — golden test fixture matrix location]]

%% ai-graph-start %%

**Related notes:**
- [[LUZ-158230 ePost ZIP Import Test Fixture Matrix (Confluence)]]
- [[LUZ-158230 QA edge-case decisions (ZIP import)]]
- [[LUZ-158230 Post Health ZIP import v1 scope decisions]]
- [[LUZ-158230 ePost ZIP import - test scope decisions]]
- [[Metadata sidecars must be scanned in luz-docs-import because they are never uploaded]]

%% ai-graph-end %%