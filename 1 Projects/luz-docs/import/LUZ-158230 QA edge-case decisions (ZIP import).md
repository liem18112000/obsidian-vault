---
ai_hash: 42d39245a908d048
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-15
entities:
- LUZ-158230 QA edge-case decisions (ZIP import)
- LUZ-158230
- ZIP import
- QA-interrogation guesses
- confirmed v1.0 spec
- SPEC TRUTH
- LUZ-158230 ePost ZIP import spec v1.0 (authoritative)
- Post Health ZIP import
- LUZ-158230 Post Health ZIP import v1 scope decisions
- Size limits
- ZIP file
- 2 GB
- metadata file
- 100 KB
- Password-protected/encrypted ZIP
- per-binary 200MB limit (spec)
- uncompressed measurement (spec)
- Document dedup
- document
- target folder
- SAME NAME AND SAME SIZE
- job-history
- filename
- concurrent-duplicate risk
- Folder dedup
- folders
- existing folders
- ePost
- luz_docs_view_controller API
- Pairing
- doc
- metadata
- naming convention `<file incl. ext>.metadata.json`
- orphan metadata.json
- document with no metadata
- health metadata
- Metadata validation
- schema validation
- required fields
- disallowed top-level element
- unparseable metadata
- Language labels
- '`en` language label'
- missing `en` language label
- Empty zip
- dot-prefixed entry
- Beyond-spec hardening
- Zip-bomb / decompression-ratio guard
- Path-traversal / absolute-path / symlink sanitisation
- Per-binary-file 200 MB size limit (Beyond-spec hardening)
- Sender/recipient notification
- 'luz_docs_import dedup: folders via view-controller API, documents via import job
  history'
- LUZ-158230 transfer.zip import size limits (2GB zip, 200MB file)
source: Testing-Agent QA interrogation 2026-09-15
status: seedling
tags:
- luz-docs
- luz_docs_import
- earchive
- zip-import
- LUZ-158230
- qa
- edge-cases
- security
title: LUZ-158230 QA edge-case decisions (ZIP import)
type: argument
---

# LUZ-158230 QA edge-case decisions (ZIP import)

> [!warning] Corrected against the confirmed spec
> An earlier version of this note recorded QA-interrogation guesses that the **confirmed v1.0 spec later overrode**. The bullets below are the SPEC TRUTH. See [[LUZ-158230 ePost ZIP import spec v1.0 (authoritative)]].

Spec-confirmed edge-case behaviour for the Post Health ZIP import (LUZ-158230). These sit on top of the [[LUZ-158230 Post Health ZIP import v1 scope decisions]].

- **Size limits** = **ZIP file < 2 GB** (the zip file's own size; exactly 2GB rejected) and **metadata file ≤ 100 KB**. **Password-protected/encrypted ZIP → rejected.** (No spec per-binary 200MB limit; no uncompressed measurement.)
- **Document dedup** = a document already in the **target folder with the SAME NAME AND SAME SIZE** is **SKIPPED**, existing kept unchanged. Idempotent skip — NOT job-history/filename, NOT a concurrent-duplicate risk.
- **Folder dedup** = folders (incl. nested) recreated **1:1** in ePost; existing folders matched not duplicated. (Impl detail: folder resolve via luz_docs_view_controller API.)
- **Pairing**: doc↔metadata by naming convention `<file incl. ext>.metadata.json` in the same folder. An **orphan metadata.json** (no matching document) is **REJECTED / reported as failed**. A **document with no metadata** is **IMPORTED without health metadata** (not an error).
- **Metadata validation** = **NONE**. No schema validation, no required fields. A **disallowed top-level element is silently ignored**; **unparseable metadata → document still imported, metadata ignored** (not a failure).
- **Language labels**: at least **`en` is RECOMMENDED, not mandatory**; missing `en` is not a failure.
- **Empty zip / only dot-prefixed+OS artefacts** → nothing imported (per-file processing; no-op). Any dot-prefixed entry is ignored.

## Beyond-spec hardening (verify current behaviour — NOT spec-required)
These were kept as robustness/security test scenarios even though the confirmed spec does not require them:
- **Zip-bomb / decompression-ratio guard** (total-uncompressed cap).
- **Path-traversal / absolute-path / symlink** sanitisation.
- **Per-binary-file 200 MB** size limit.
- **Sender/recipient notification** on success/partial/failure (fire-and-forget).

## Related

- [[LUZ-158230 ePost ZIP import spec v1.0 (authoritative)]]
- [[LUZ-158230 Post Health ZIP import v1 scope decisions]]
- [[luz_docs_import dedup folders via view-controller API, documents via import job history|luz_docs_import dedup: folders via view-controller API, documents via import job history]]
- [[LUZ-158230 transfer.zip import size limits (2GB zip, 200MB file)]]

%% ai-graph-start %%

**Related notes:**
- [[LUZ-158230 transfer.zip import size limits (2GB zip, 200MB file)]]
- [[LUZ-158230 ePost ZIP import spec v1.0 (authoritative)]]
- [[LUZ-158230 ePost ZIP import - test scope decisions]]
- [[ePost ZIP import (LUZ-158230) behavior rules confirmed by domain expert]]
- [[LUZ-158230 Post Health ZIP import v1 scope decisions]]

**Relations:**
- LUZ-158230 QA edge-case decisions (ZIP import) — *is about* — LUZ-158230
- LUZ-158230 QA edge-case decisions (ZIP import) — *is about* — ZIP import
- LUZ-158230 QA edge-case decisions (ZIP import) — *corrected against* — confirmed v1.0 spec
- LUZ-158230 QA edge-case decisions (ZIP import) — *overrode* — QA-interrogation guesses
- confirmed v1.0 spec — *overrode* — QA-interrogation guesses
- SPEC TRUTH — *is* — confirmed v1.0 spec
- LUZ-158230 QA edge-case decisions (ZIP import) — *references* — LUZ-158230 ePost ZIP import spec v1.0 (authoritative)
- LUZ-158230 QA edge-case decisions (ZIP import) — *defines behaviour for* — Post Health ZIP import
- LUZ-158230 QA edge-case decisions (ZIP import) — *builds upon* — LUZ-158230 Post Health ZIP import v1 scope decisions
- Size limits — *applies to* — ZIP file
- ZIP file — *has size limit* — 2 GB
- Size limits — *applies to* — metadata file
- metadata file — *has size limit* — 100 KB
- Password-protected/encrypted ZIP — *is* — rejected
- per-binary 200MB limit (spec) — *is not part of* — confirmed v1.0 spec
- uncompressed measurement (spec) — *is not part of* — confirmed v1.0 spec
- Document dedup — *skips* — document
- document — *has property* — SAME NAME AND SAME SIZE
- document — *located in* — target folder
- Document dedup — *is not based on* — job-history
- Document dedup — *is not based on* — filename
- Document dedup — *is not* — concurrent-duplicate risk
- Folder dedup — *recreates* — folders
- folders — *recreated in* — ePost
- existing folders — *are* — matched not duplicated
- Folder dedup — *resolves via* — luz_docs_view_controller API
- Pairing — *links* — doc
- Pairing — *links* — metadata
- Pairing — *uses* — naming convention `<file incl. ext>.metadata.json`
- orphan metadata.json — *is* — REJECTED / reported as failed
- document with no metadata — *is* — IMPORTED without health metadata
- Metadata validation — *is* — NONE
- Metadata validation — *does not include* — schema validation
- Metadata validation — *does not include* — required fields
- disallowed top-level element — *is* — silently ignored
- unparseable metadata — *results in* — document still imported
- unparseable metadata — *results in* — metadata ignored
- Language labels — *recommends* — `en` language label
- `en` language label — *is not* — mandatory
- missing `en` language label — *is not* — a failure
- Empty zip — *results in* — nothing imported
- dot-prefixed entry — *is* — ignored
- Beyond-spec hardening — *includes* — Zip-bomb / decompression-ratio guard
- Beyond-spec hardening — *includes* — Path-traversal / absolute-path / symlink sanitisation
- Beyond-spec hardening — *includes* — Per-binary-file 200 MB size limit (Beyond-spec hardening)
- Beyond-spec hardening — *includes* — Sender/recipient notification
- Beyond-spec hardening — *is not* — spec-required
- LUZ-158230 — *related to* — LUZ-158230 ePost ZIP import spec v1.0 (authoritative)
- LUZ-158230 — *related to* — LUZ-158230 Post Health ZIP import v1 scope decisions
- LUZ-158230 — *related to* — luz_docs_import dedup: folders via view-controller API, documents via import job history
- LUZ-158230 — *related to* — LUZ-158230 transfer.zip import size limits (2GB zip, 200MB file)

%% ai-graph-end %%