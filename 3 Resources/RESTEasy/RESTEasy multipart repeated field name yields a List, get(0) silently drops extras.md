---
ai_hash: 5ddcebe6276a6268
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-13
entities: []
source: session 2026-08-13
status: seedling
tags:
- resteasy
- jax-rs
- multipart
- luz-docs
- gotcha
title: 'RESTEasy multipart: repeated field name yields a List, get(0) silently drops
  extras'
type: gotcha
---

# RESTEasy multipart: repeated field name yields a List, get(0) silently drops extras

In RESTEasy, `MultipartFormDataInput.getFormDataMap().get(fieldName)` returns a `List<InputPart>`, not a single part. When a client repeats the same multipart field name (e.g. several parts all named `file`), every one of them lands in that list.

**The gotcha:** code that reads only `docs.get(0)` will silently drop the extra parts — no error, no warning. From the caller's side this is silent data loss (they sent 3 files, 1 was processed).

**Where it bit us:** `luz_docs_import` `DocsImportService.createTempFile` did exactly this — multiple uploaded zips meant only the first was imported and the rest were discarded (their RESTEasy-buffered temp files got cleaned up by `documents.close()` but were never read).

**Guard applied:** reject `docs.size() > 1` with HTTP 400 (`DocsImportException("Only one zip file can be imported per request", SC_BAD_REQUEST)`), so callers get a clear error instead of silent loss. Alternative, if multi-file is desired: loop over `docs` instead of taking `get(0)`.

**General lesson:** when consuming a multipart map, always decide explicitly what `size() != 1` means — reject, loop, or merge. Never let `.get(0)` be an implicit "take the first, ignore the rest".

%% ai-graph-start %%

**Related notes:**
- [[Declare a variable before the try so the catch block can log it]]
- [[RESTEasy MultipartFormDataInput buffers the whole upload to tmp before the app reads it]]
- [[Read RESTEasy multipart parts eagerly in-request; never pass InputPart around]]
- [[luz-docs-import importZipName comes from the uploaded multipart filename, not the on-disk zip]]
- [[luz_docs_import upload-zip is slow for large files due to a synchronous double-write]]

%% ai-graph-end %%