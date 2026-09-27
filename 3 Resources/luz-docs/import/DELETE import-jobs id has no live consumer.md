---
ai_hash: eb18ccff27796381
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-10
entities: []
source: session 2026-08-10 import-jobs usage investigation
status: seedling
tags:
- luz-docs-import
- dead-code
- gotcha
title: DELETE import-jobs id has no live consumer
type: observation
---

# DELETE import-jobs id has no live consumer

The `DELETE {tenant}/import-jobs/{id}` endpoint in `luz_docs_import` (`ImportJobResource.delete`) has **no live consumer** anywhere in the Kepler workspace.

A client wrapper exists — `LuzDocsImportContainer.deleteJob(jobId)` (`.onPath(jobId).executeDelete()`) — and it is correct, but it is **never called**. The only trace of intent is a dangling log line in `DashboardController.logWhenJobFinish()` (~line 176): `Ivy.log().info("[uploadZipFile] DELETE jobId: "+ ...)` that prints the word DELETE without actually invoking `deleteJob`.

**Consequence:** import-job records are never purged by the dashboard — they accumulate. Looks like a planned cleanup step (wrapper + log written) that was never wired up.

**Search gotcha:** grepping `deleteJob` across the workspace produces false positives — `luz_docs`/`liem_luz_docs` have an unrelated `AnalyzeService.deleteJob` (GCP analyze jobs), and `luz_docs_import`'s own `DocsImportService.deleteJob`/`JsonStoreService.deleteJob` are the server-side implementation, not consumers. Only the `LuzDocsImportContainer` call chain counts as a consumer.

## Related
[[luz_mylife_web is the only consumer of luz_docs_import import-jobs endpoints]]

## Related

- [[luz_mylife_web is the only consumer of luz_docs_import import-jobs endpoints]]

%% ai-graph-start %%

**Related notes:**
- [[luz_mylife_web is the only consumer of luz_docs_import import-jobs endpoints]]
- [[GET import-jobs id showWarning=false returns LegacyImportJob to keep luz_mylife_web working]]
- [[luz-docs-import bug rejected files not removed from unprocessedFiles]]
- [[luz_docs_import detects dead async-worker pods via kubectl during status polling]]
- [[luz-docs-import JobProgressWriter checkpoints are for crash-durability + heartbeat, not UI progress]]

%% ai-graph-end %%