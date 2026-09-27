---
ai_hash: 923ec63ff441a8dc
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-06-11
entities:
- luz_docs_statistic
- EJB timer
- Pub/Sub
- $facet aggregation
- document statistics
- UpdateDocumentStatisticTimer
- UPDATE_DOCUMENT_STATISTIC_MAX_RECORD
- Google Pub/Sub messages
- luz.docs.document.statistic.sub
- tenantId
- documents collection
- jsonstore
- archived facet
- deleted facet
- total facet
- isBeingCreated
- documentstatistics collection
- service tenant
- totalDocuments
- Document sizes
- luz_docs_statistic two-token model service-tenant vs per-tenant cache token
source: luz_docs_statistic repo analysis, session 2026-06-11
status: seedling
tags:
- luz
- pubsub
- mongodb
- aggregation
- ejb-timer
- luz-docs-statistic
title: luz_docs_statistic updates stats via 1-minute EJB timer over Pub/Sub and $facet
  aggregation
type: concept
---

# luz_docs_statistic updates stats via 1-minute EJB timer over Pub/Sub and $facet aggregation

luz_docs_statistic recomputes per-tenant document statistics on a pull-based cycle rather than reacting per event:

1. An EJB `@Schedule` timer fires every minute (`UpdateDocumentStatisticTimer`, non-persistent).
2. It synchronously pulls + acks up to `UPDATE_DOCUMENT_STATISTIC_MAX_RECORD` (default 100) Google Pub/Sub messages from subscription `luz.docs.document.statistic.sub`; each message just names a `tenantId` whose documents changed.
3. Tenant IDs are de-duplicated, then per tenant a single $facet aggregation runs over the `documents` collection (via jsonstore) computing three facets — archived, deleted, total — each as count + summed size. Docs with `isBeingCreated=true` are $match-excluded first.
4. The result is upserted into `documentstatistics` under the service tenant; an insert only happens when `totalDocuments != 0`.

Quirks worth remembering:
- Document sizes are stored as **strings with a `Byte` suffix** (e.g. "3028Byte"), not numbers — anything consuming the entity must parse that.
- Pub/Sub messages are only triggers; the aggregation always recomputes from scratch, so duplicates/lost increments are harmless (eventually-consistent by design).

See [[luz_docs_statistic two-token model service-tenant vs per-tenant cache token]] for which token each step uses.

## Related

- [[luz_docs_statistic two-token model service-tenant vs per-tenant cache token]]

%% ai-graph-start %%

**Related notes:**
- [[totalFolders needs a second aggregate because a $facet pipeline is bound to one collection]]
- [[luz_docs_statistic two-token model service-tenant vs per-tenant cache token]]
- [[luz-docs-statistic-get-latest-endpoint]]
- [[New architecture for documentStatistic]]
- [[luz_docs_statistic computes per-tenant unmaterializedDocuments count]]

**Relations:**
- luz_docs_statistic — *updates* — document statistics
- luz_docs_statistic — *uses* — EJB timer
- luz_docs_statistic — *uses* — Pub/Sub
- luz_docs_statistic — *uses* — $facet aggregation
- EJB timer — *fires every* — minute
- EJB timer — *is named* — UpdateDocumentStatisticTimer
- UpdateDocumentStatisticTimer — *pulls and acks* — Google Pub/Sub messages
- Google Pub/Sub messages — *from* — luz.docs.document.statistic.sub
- Google Pub/Sub messages — *contain* — tenantId
- UpdateDocumentStatisticTimer — *pulls max* — UPDATE_DOCUMENT_STATISTIC_MAX_RECORD
- UPDATE_DOCUMENT_STATISTIC_MAX_RECORD — *has default value* — 100
- $facet aggregation — *operates on* — documents collection
- $facet aggregation — *uses* — jsonstore
- $facet aggregation — *computes* — archived facet
- $facet aggregation — *computes* — deleted facet
- $facet aggregation — *computes* — total facet
- archived facet — *calculates* — count and summed size
- deleted facet — *calculates* — count and summed size
- total facet — *calculates* — count and summed size
- $facet aggregation — *excludes documents with property* — isBeingCreated=true
- aggregation result — *upserts into* — documentstatistics collection
- documentstatistics collection — *managed by* — service tenant
- insert into — *documentstatistics collection requires condition* — totalDocuments != 0
- Document sizes — *stored as string with suffix* — Byte
- Pub/Sub messages — *serves as trigger for* — $facet aggregation
- $facet aggregation — *is* — eventually-consistent
- luz_docs_statistic — *related to* — luz_docs_statistic two-token model service-tenant vs per-tenant cache token

%% ai-graph-end %%