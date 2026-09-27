---
ai_hash: 3dd1763473e8223e
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-15
entities:
- LUZ-158230 transfer.zip import size limits
- 200 MB per binary document
- ePost ZIP import spec v1.0 (authoritative)
- ZIP file size
- 2 GB
- Metadata file
- 100 KB
- Password-protected / encrypted ZIP
- Standard ZIP
- file/folder names
- UTF-8
- file count
- uncompressed bytes
- beyond-spec hardening test case
- 'luz_docs_import dedup: folders via view-controller API, documents via import job
  history'
- LUZ-158230 Post Health ZIP import v1 scope decisions
- LUZ-158230 QA edge-case decisions (ZIP import)
source: Testing-Agent refine session 2026-09-14
status: seedling
tags:
- luz-docs
- luz_docs_import
- earchive
- zip-import
- LUZ-158230
- boundary
title: LUZ-158230 transfer.zip import size limits (2GB zip, 200MB file)
type: lesson
---

# LUZ-158230 transfer.zip import size limits (2GB zip, 200MB file)

> [!warning] Corrected against the confirmed spec
> The "200 MB per binary file" value in this note's title was an interrogation guess, **not** in the confirmed spec. Spec truth below. See [[LUZ-158230 ePost ZIP import spec v1.0 (authoritative)]].

**Spec-confirmed limits (ePost ZIP import spec v1.0):**
- **ZIP file size < 2 GB** — the zip file's own size; exactly 2 GB is rejected.
- **Metadata file ≤ 100 KB** per metadata file.
- **Password-protected / encrypted ZIP → rejected.**
- Standard ZIP; file/folder names **UTF-8**.

There is **no spec limit of 200 MB per binary document** and **no cap on file count**; the 200 MB figure survives only as a **beyond-spec hardening** test case (verify current behaviour, not an acceptance criterion). The limit is on the **zip file size**, not uncompressed bytes.

## Related

- [[LUZ-158230 ePost ZIP import spec v1.0 (authoritative)]]
- [[luz_docs_import dedup folders via view-controller API, documents via import job history|luz_docs_import dedup: folders via view-controller API, documents via import job history]]
- [[LUZ-158230 Post Health ZIP import v1 scope decisions]]
- [[LUZ-158230 QA edge-case decisions (ZIP import)]]

%% ai-graph-start %%

**Related notes:**
- [[LUZ-158230 QA edge-case decisions (ZIP import)]]
- [[LUZ-158230 ePost ZIP import spec v1.0 (authoritative)]]
- [[LUZ-158230 ePost ZIP import - test scope decisions]]
- [[LUZ-158230 Post Health ZIP import v1 scope decisions]]
- [[ePost eArchive ZIP import transfer.zip shape and luz_docs_import]]

**Relations:**
- LUZ-158230 transfer.zip import size limits — *had initial guess* — 200 MB per binary document
- 200 MB per binary document — *is not in* — ePost ZIP import spec v1.0 (authoritative)
- ePost ZIP import spec v1.0 (authoritative) — *defines limit for* — ZIP file size
- ZIP file size — *must be less than* — 2 GB
- ePost ZIP import spec v1.0 (authoritative) — *defines limit for* — Metadata file
- Metadata file — *must be less than or equal to* — 100 KB
- ePost ZIP import spec v1.0 (authoritative) — *rejects* — Password-protected / encrypted ZIP
- ePost ZIP import spec v1.0 (authoritative) — *requires* — Standard ZIP
- ePost ZIP import spec v1.0 (authoritative) — *requires encoding for* — file/folder names
- file/folder names — *must be* — UTF-8
- ePost ZIP import spec v1.0 (authoritative) — *has no limit for* — 200 MB per binary document
- ePost ZIP import spec v1.0 (authoritative) — *has no cap on* — file count
- limit — *is on* — ZIP file size
- limit — *is not on* — uncompressed bytes
- 200 MB per binary document — *is a* — beyond-spec hardening test case
- LUZ-158230 transfer.zip import size limits — *is related to* — ePost ZIP import spec v1.0 (authoritative)
- LUZ-158230 transfer.zip import size limits — *is related to* — luz_docs_import dedup: folders via view-controller API, documents via import job history
- LUZ-158230 transfer.zip import size limits — *is related to* — LUZ-158230 Post Health ZIP import v1 scope decisions
- LUZ-158230 transfer.zip import size limits — *is related to* — LUZ-158230 QA edge-case decisions (ZIP import)

%% ai-graph-end %%