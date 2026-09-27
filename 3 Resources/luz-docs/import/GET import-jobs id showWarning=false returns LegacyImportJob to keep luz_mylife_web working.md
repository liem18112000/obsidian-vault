---
ai_hash: fa3529d2df5a95e8
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-10
entities: []
source: session 2026-08-10 import-jobs usage investigation
status: seedling
tags:
- luz-docs-import
- api-versioning
- backward-compat
- gotcha
title: GET import-jobs id showWarning=false returns LegacyImportJob to keep luz_mylife_web
  working
type: lesson
---

# GET import-jobs id showWarning=false returns LegacyImportJob to keep luz_mylife_web working

The `GET {tenant}/import-jobs/{id}` endpoint in `luz_docs_import` (`ImportJobResource.getById`) has a `showWarning` query param defaulting to **false**. On the default path it returns `LegacyImportJob.from(job)` — the **master-compatible shape** — instead of the enriched `ImportJob` (successfulFiles/skippedFiles details, rejectedFiles).

**Why:** the sole consumer, `luz_mylife_web`'s `DashboardController.checkJobStatus()`, polls via `LuzDocsImportContainer.getJob()` which calls `.getAs(ImportJob.class)` **without** sending `showWarning`. So the default branch exists specifically to keep that dashboard deserializing into `ch.klara.luz.mylife.model.ImportJob` unchanged.

**Gotcha / decision:** do NOT change the `showWarning=false` default response shape without updating the mylife consumer's model + wrapper. The enriched shape (`?showWarning=true`) is currently requested by nobody. If mylife ever needs the richer data, `getJob()` must append `?showWarning=true`.

## Related
[[luz_mylife_web is the only consumer of luz_docs_import import-jobs endpoints]]

## Related

- [[luz_mylife_web is the only consumer of luz_docs_import import-jobs endpoints]]

%% ai-graph-start %%

**Related notes:**
- [[luz_mylife_web is the only consumer of luz_docs_import import-jobs endpoints]]
- [[DELETE import-jobs id has no live consumer]]
- [[luz-docs-import JobProgressWriter checkpoints are for crash-durability + heartbeat, not UI progress]]
- [[Gate enriched REST response behind a boolean query param with a legacy-shaped DTO]]
- [[luz-jsonstore GET-by-id masks read exceptions as empty-body 400; 10MB max-post-size caps whole-doc $set writes]]

%% ai-graph-end %%