---
ai_hash: 789c4c195971bf31
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-11
entities: []
source: luz_docs_import LUZ-158230 · 2026-08-11
status: seedling
tags:
- luz-docs-import
- error-handling
- exceptions
- java
title: Fail an import on a corrupt ZIP by translating the extraction exception to
  a domain FailureCode
type: lesson
---

# Fail an import on a corrupt ZIP by translating the extraction exception to a domain FailureCode

A background job that extracts a ZIP and swallows the extraction exception can proceed to report **DONE** on a partial/empty import — silent data loss. Fix: stop swallowing it; let the low-level `ZipException` (a subtype of `IOException`) propagate, then **translate it in the service layer** into a domain exception carrying a meaningful failure code (e.g. `DocsImportBackgroundException(FailureCode.INVALID)`).

**Why translate rather than let it bubble raw:** the jobs handler has a two-tier catch — a specific `catch (DomainException)` that records `e.getFailureCode()`, and a generic `catch (Exception)` that records `INTERNAL_SERVICE_ERROR`. Translating at the call site means the client sees the precise, actionable code (**INVALID** = bad input ZIP) instead of a generic internal error. Keep the low-level util free of domain types; do the mapping one layer up.

Related: [[zip4j 2.8.0 ZipFile is not AutoCloseable so try-with-resources won't compile]]

## Related

- [[zip4j 2.8.0 ZipFile is not AutoCloseable so try-with-resources won't compile]]

%% ai-graph-start %%

**Related notes:**
- [[zip4j 2.8.0 ZipFile is not AutoCloseable so try-with-resources won't compile]]
- [[zip4j 2.8.0 ZipException is-a IOException, and extractFile keeps Zip-Slip protection (per-entry extraction)]]
- [[luz-docs-import 'Permission denied' failures trace to zip4j applying archive file permissions]]
- [[luz-docs-import bug rejected files not removed from unprocessedFiles]]
- [[luz_docs_import health ZIP import uses a two-layer failure model]]

%% ai-graph-end %%