---
ai_hash: f07fa27052623333
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-15
entities:
- luz_docs_import
- import job history
- Document-dedup mechanism
- Spec
- LUZ-158230 ePost ZIP import spec v1.0 (authoritative)
- Post Health ZIP import
- Duplicate detection
- Document dedup
- Folder handling
- ePost
- luz_docs_view_controller folder API
- LUZ-158230 Post Health ZIP import v1 scope decisions
- LUZ-158230 QA edge-case decisions (ZIP import)
- target folder
- document
- folders
- tester
- recipient/folder auto-provisioning
- document name
- document size
- idempotent skip
- existing kept unchanged
source: Testing-Agent refine session 2026-09-14
status: seedling
tags:
- luz-docs
- luz_docs_import
- luz_docs_view_controller
- dedup
- zip-import
- LUZ-158230
title: 'luz_docs_import dedup: folders via view-controller API, documents via import
  job history'
type: lesson
---

# luz_docs_import dedup: folders via view-controller API, documents via import job history

> [!warning] Document-dedup mechanism corrected against the confirmed spec
> The "document dedup via import job history / filename" claim was an interrogation guess. **Spec truth:** a document already present in the **target folder with the SAME NAME AND SAME SIZE is SKIPPED**, existing kept unchanged (an idempotent skip). See [[LUZ-158230 ePost ZIP import spec v1.0 (authoritative)]].

In the Post Health ZIP import (luz_docs_import), duplicate detection:
- **Document dedup** (spec) = **same name + same size in the target folder → skipped**, existing document kept unchanged.
- **Folder handling** (spec) = folders (incl. nested) recreated **1:1** in ePost; existing folders matched, not duplicated. Implementation detail (plausible, not spec-stated): folder resolve/create goes through the **luz_docs_view_controller folder API** before uploading docs.

Why it matters: a tester exercises document dedup by re-sending an identical name+size file (expect skip) and folder handling by re-importing an existing folder tree (expect 1:1 match, no duplicate folders). No recipient/folder auto-provisioning.

## Related

- [[LUZ-158230 ePost ZIP import spec v1.0 (authoritative)]]
- [[LUZ-158230 Post Health ZIP import v1 scope decisions]]
- [[LUZ-158230 QA edge-case decisions (ZIP import)]]

## Related

- [[LUZ-158230 Post Health ZIP import v1 scope decisions]]

%% ai-graph-start %%

**Related notes:**
- [[ePost ZIP import dedup documents by job-success path, folders via view-controller]]
- [[luz_docs_import ZIP import is path-based idempotent per importZipName]]
- [[LUZ-158230 QA edge-case decisions (ZIP import)]]
- [[ePost ZIP import (LUZ-158230) behavior rules confirmed by domain expert]]
- [[luz-docs-import importZipName comes from the uploaded multipart filename, not the on-disk zip]]

**Relations:**
- luz_docs_import — *is also known as* — Post Health ZIP import
- Post Health ZIP import — *performs* — Duplicate detection
- Duplicate detection — *covers* — Document dedup
- Duplicate detection — *covers* — Folder handling
- Document dedup — *is defined by* — Spec
- Spec — *is* — LUZ-158230 ePost ZIP import spec v1.0 (authoritative)
- Document dedup — *requires matching* — document name
- Document dedup — *requires matching* — document size
- Document dedup — *occurs in* — target folder
- Document dedup — *results in* — idempotent skip
- Document dedup — *also results in* — existing kept unchanged
- Folder handling — *recreates* — folders
- Folder handling — *recreates in* — ePost
- Folder handling — *matches* — existing folders
- Folder handling — *uses* — luz_docs_view_controller folder API
- luz_docs_view_controller folder API — *is used for* — folder resolve/create
- luz_docs_view_controller folder API — *is used before* — uploading docs
- tester — *exercises* — Document dedup
- tester — *exercises* — Folder handling
- Document-dedup mechanism — *was guessed to use* — import job history
- LUZ-158230 ePost ZIP import spec v1.0 (authoritative) — *is related to* — luz_docs_import
- LUZ-158230 Post Health ZIP import v1 scope decisions — *is related to* — luz_docs_import
- LUZ-158230 QA edge-case decisions (ZIP import) — *is related to* — luz_docs_import
- luz_docs_import — *does not include* — recipient/folder auto-provisioning

%% ai-graph-end %%