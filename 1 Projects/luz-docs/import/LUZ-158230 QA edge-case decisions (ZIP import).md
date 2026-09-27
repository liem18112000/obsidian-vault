---
title: "LUZ-158230 QA edge-case decisions (ZIP import)"
created: 2026-09-15
type: argument
status: seedling
source: "Testing-Agent QA interrogation 2026-09-15"
tags: [luz-docs, luz_docs_import, earchive, zip-import, LUZ-158230, qa, edge-cases, security]
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
- [[luz_docs_import dedup: folders via view-controller API, documents via import job history]]
- [[LUZ-158230 transfer.zip import size limits (2GB zip, 200MB file)]]
