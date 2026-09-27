---
ai_hash: e4b9ff6fe404a0ca
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-15
entities:
- LUZ-158230
- QA edge-case decisions
- ZIP import
- confirmed spec
- earlier version of this note
- QA-interrogation guesses
- v1.0 spec
- SPEC TRUTH
- LUZ-158230 ePost ZIP import spec v1.0 (authoritative)
- Post Health ZIP import
- LUZ-158230 Post Health ZIP import v1 scope decisions
- ZIP file
- metadata file
- Password-protected/encrypted ZIP
- per-binary 200MB limit
- uncompressed measurement
- Document dedup
- document
- target folder
- Idempotent skip
- job-history/filename
- concurrent-duplicate risk
- Folder dedup
- ePost
- luz_docs_view_controller API
- Pairing
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
- '`en`'
- Empty zip
- dot-prefixed+OS artefacts
- dot-prefixed entry
- Beyond-spec hardening
- robustness/security test scenarios
- Zip-bomb / decompression-ratio guard
- total-uncompressed cap
- Path-traversal / absolute-path / symlink sanitisation
- Per-binary-file 200 MB size limit
- Sender/recipient notification
- luz_docs_import dedup
- import job history
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
- LUZ-158230 — *concerns* — QA edge-case decisions
- QA edge-case decisions — *for* — ZIP import
- QA edge-case decisions — *corrected_against* — confirmed spec
- earlier version of this note — *recorded* — QA-interrogation guesses
- v1.0 spec — *overrode* — QA-interrogation guesses
- v1.0 spec — *is* — SPEC TRUTH
- SPEC TRUTH — *is_detailed_in* — LUZ-158230 ePost ZIP import spec v1.0 (authoritative)
- v1.0 spec — *is* — LUZ-158230 ePost ZIP import spec v1.0 (authoritative)
- LUZ-158230 — *defines_edge_case_behaviour_for* — Post Health ZIP import
- Post Health ZIP import — *builds_on* — LUZ-158230 Post Health ZIP import v1 scope decisions
- ZIP file — *must_be_less_than* — 2 GB
- metadata file — *must_be_less_than_or_equal_to* — 100 KB
- Password-protected/encrypted ZIP — *is* — rejected
- confirmed spec — *does_not_include* — per-binary 200MB limit
- confirmed spec — *does_not_include* — uncompressed measurement
- document — *is_skipped_if_matches_existing_in* — target folder by name and size
- existing document — *remains* — unchanged
- Document dedup — *is_an* — Idempotent skip
- Idempotent skip — *does_not_rely_on* — job-history/filename
- Idempotent skip — *prevents* — concurrent-duplicate risk
- folders — *are_recreated_1:1_in* — ePost
- existing folders — *are* — matched
- existing folders — *are_not* — duplicated
- folder resolution — *uses* — luz_docs_view_controller API
- document — *is_paired_with_metadata_via* — naming convention `<file incl. ext>.metadata.json`
- orphan metadata.json — *is* — rejected
- orphan metadata.json — *is* — reported as failed
- document with no metadata — *is* — imported without health metadata
- Metadata validation — *is* — none
- Metadata validation — *excludes* — schema validation
- Metadata validation — *excludes* — required fields
- disallowed top-level element — *is* — silently ignored
- unparseable metadata — *results_in* — metadata ignored
- unparseable metadata — *allows* — document import
- `en` — *is_recommended_for* — Language labels
- `en` — *is_not_mandatory_for* — Language labels
- missing `en` — *is_not* — a failure
- Empty zip — *results_in* — nothing imported
- dot-prefixed+OS artefacts — *results_in* — nothing imported
- dot-prefixed entry — *is* — ignored
- Beyond-spec hardening — *are* — robustness/security test scenarios
- confirmed spec — *does_not_require* — Beyond-spec hardening
- Beyond-spec hardening — *includes* — Zip-bomb / decompression-ratio guard
- Zip-bomb / decompression-ratio guard — *has* — total-uncompressed cap
- Beyond-spec hardening — *includes* — Path-traversal / absolute-path / symlink sanitisation
- Beyond-spec hardening — *includes* — Per-binary-file 200 MB size limit
- Beyond-spec hardening — *includes* — Sender/recipient notification
- LUZ-158230 QA edge-case decisions — *references* — LUZ-158230 ePost ZIP import spec v1.0 (authoritative)
- LUZ-158230 QA edge-case decisions — *references* — LUZ-158230 Post Health ZIP import v1 scope decisions
- LUZ-158230 QA edge-case decisions — *references* — luz_docs_import dedup
- luz_docs_import dedup — *handles_folders_via* — luz_docs_view_controller API
- luz_docs_import dedup — *handles_documents_via* — import job history
- LUZ-158230 QA edge-case decisions — *references* — LUZ-158230 transfer.zip import size limits (2GB zip, 200MB file)

%% ai-graph-end %%