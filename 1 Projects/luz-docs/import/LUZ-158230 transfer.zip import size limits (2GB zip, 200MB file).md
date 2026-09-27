---
title: "LUZ-158230 transfer.zip import size limits (2GB zip, 200MB file)"
created: 2026-09-15
type: lesson
status: seedling
source: "Testing-Agent refine session 2026-09-14"
tags: [luz-docs, luz_docs_import, earchive, zip-import, LUZ-158230, boundary]
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
- [[luz_docs_import dedup: folders via view-controller API, documents via import job history]]
- [[LUZ-158230 Post Health ZIP import v1 scope decisions]]
- [[LUZ-158230 QA edge-case decisions (ZIP import)]]
