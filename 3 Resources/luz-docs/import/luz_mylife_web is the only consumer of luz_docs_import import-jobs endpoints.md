---
ai_hash: 4b0272036e973a6c
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-10
entities: []
source: session 2026-08-10 import-jobs usage investigation
status: seedling
tags:
- luz-docs
- luz-docs-import
- luz-mylife-web
- rest-consumer
title: luz_mylife_web is the only consumer of luz_docs_import import-jobs endpoints
type: observation
---

# luz_mylife_web is the only consumer of luz_docs_import import-jobs endpoints

`luz_mylife_web` (the Ivy MyLife / ePost dashboard) is the **only** consumer of the `import-jobs` REST endpoints exposed by the `luz_docs_import` microservice. No other service, frontend (luz_webclient, luz_docs_view_controller), or integration-test repo calls them.

Its client wrapper is `ch.klara.luz.mylife.container.LuzDocsImportContainer` with three methods:
- `uploadZipFile()` → `POST {tenant}/import-jobs/upload-zip`
- `getJob(jobId)` → `GET {tenant}/import-jobs/{id}`
- `deleteJob(jobId)` → `DELETE {tenant}/import-jobs/{id}`

The base path is not hard-coded in the wrapper; it comes from `luz_components` `RestResource.LUZ_DOCS_IMPORT = "{company-tenant-id}/import-jobs"`, and `.onPath(x)` appends the segment. So when hunting for consumers of a service, grep the `RestResource` enum value + wrapper class, not just the literal URL path.

Verified 2026-08-10 via a workspace-wide sweep of C:\Users\dvtliem\Kepler.

## Related
[[GET import-jobs id showWarning=false returns LegacyImportJob to keep luz_mylife_web working]]
[[DELETE import-jobs id has no live consumer]]

## Related

- [[GET import-jobs id showWarning=false returns LegacyImportJob to keep luz_mylife_web working]]
- [[DELETE import-jobs id has no live consumer]]

%% ai-graph-start %%

**Related notes:**
- [[DELETE import-jobs id has no live consumer]]
- [[GET import-jobs id showWarning=false returns LegacyImportJob to keep luz_mylife_web working]]
- [[luz_epost_business_web to luz_docs_view_controller integration goes through one REST client package]]
- [[luz-docs-import ZIP import call chain]]
- [[Luz docs-import zip flow upload-zip returns job-id, poll GET until DONE]]

%% ai-graph-end %%