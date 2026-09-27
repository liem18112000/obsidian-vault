---
ai_hash: fbe3fa4e3e5a70f7
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-15
entities:
- LUZ-158230
- ZIP import size limits
- 2GB zip limit (initial guess)
- 200MB file limit (initial guess)
- ePost ZIP import spec v1.0
- ZIP file size
- 2 GB
- Metadata file
- 100 KB
- Password-protected / encrypted ZIP
- Standard ZIP
- UTF-8
- binary document size
- file count
- beyond-spec hardening test case
- current behaviour
- acceptance criterion
- uncompressed bytes
- luz_docs_import dedup
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
- LUZ-158230 — *concerns* — ZIP import size limits
- ZIP import size limits — *initially mentioned* — 2GB zip limit (initial guess)
- ZIP import size limits — *initially mentioned* — 200MB file limit (initial guess)
- 200MB file limit (initial guess) — *was an* — interrogation guess
- 200MB file limit (initial guess) — *is not in* — ePost ZIP import spec v1.0
- ePost ZIP import spec v1.0 — *specifies* — ZIP file size
- ZIP file size — *must be less than* — 2 GB
- ePost ZIP import spec v1.0 — *specifies* — Metadata file
- Metadata file — *limit is* — 100 KB
- ePost ZIP import spec v1.0 — *rejects* — Password-protected / encrypted ZIP
- ePost ZIP import spec v1.0 — *requires* — Standard ZIP
- Standard ZIP — *uses* — UTF-8
- ePost ZIP import spec v1.0 — *has no limit for* — binary document size
- ePost ZIP import spec v1.0 — *has no limit for* — file count
- 200MB file limit (initial guess) — *is a* — beyond-spec hardening test case
- beyond-spec hardening test case — *verifies* — current behaviour
- beyond-spec hardening test case — *is not an* — acceptance criterion
- ZIP import size limits — *are based on* — ZIP file size
- ZIP import size limits — *are not based on* — uncompressed bytes
- LUZ-158230 — *is related to* — ePost ZIP import spec v1.0
- LUZ-158230 — *is related to* — luz_docs_import dedup
- LUZ-158230 — *is related to* — LUZ-158230 Post Health ZIP import v1 scope decisions
- LUZ-158230 — *is related to* — LUZ-158230 QA edge-case decisions (ZIP import)

%% ai-graph-end %%