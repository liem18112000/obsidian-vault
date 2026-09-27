---
ai_hash: d7fd725bb52f92e9
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-06-11
entities:
- totalFolders
- $facet pipeline
- luz_docs_statistic
- documents collection
- folders collection
- averageFoldersPerDocument
- MongoDBService.aggregate
- DocumentStatisticUtils.extractCount
- EJB timer
- PubSub
- folderIds
- aggregate call
- tenant token
- sub-pipelines
- collection-parameterized overload
- old 3-arg method
- non-being-created documents
- deleted documents
- luz_docs_statistic updates stats via 1-minute EJB timer over PubSub and $facet aggregation
- documents $facet
- minimal aggregate pipeline
source: session 2026-06-11
status: seedling
tags:
- luz
- luz-docs-statistic
- mongodb
- aggregation
- facet
title: totalFolders needs a second aggregate because a $facet pipeline is bound to
  one collection
type: lesson
---

# totalFolders needs a second aggregate because a $facet pipeline is bound to one collection

When adding `totalFolders` to luz_docs_statistic (June 2026), the metric could not join the existing documents $facet: a $facet's sub-pipelines all run over the same input collection, so a per-tenant folder count requires a separate aggregate call against the `folders` collection. The job now runs two aggregates per tenant with the same cached tenant token: the documents $facet (counts + `averageFoldersPerDocument` via $avg of $size($ifNull(folderIds,[]))) and a minimal `[{$group:{_id:null,count:{$sum:1}}}]` on folders, extracted by `DocumentStatisticUtils.extractCount` (null result → 0, i.e. empty collection).

Enabler: `MongoDBService.aggregate` previously hardcoded the `documents` collection; it now has a collection-parameterized overload and the old 3-arg method delegates to it.

Semantics note: `averageFoldersPerDocument` averages over all non-being-created documents (deleted included), counting a missing `folderIds` as 0 folders.

Related: [[luz_docs_statistic updates stats via 1-minute EJB timer over PubSub and $facet aggregation]]

## Related

- [[luz_docs_statistic updates stats via 1-minute EJB timer over PubSub and $facet aggregation]]

%% ai-graph-start %%

**Related notes:**
- [[luz_docs_statistic updates stats via 1-minute EJB timer over PubSub and $facet aggregation]]
- [[luz_docs_statistic computes per-tenant unmaterializedDocuments count]]
- [[luz_docs_statistic unmaterializedDocuments metric counts docs missing any materialize sentinel field]]
- [[Stale-materialized detection recomputes MaterializeCompute state via $lookup inside the statistic $facet]]
- [[luz_docs_statistic two-token model service-tenant vs per-tenant cache token]]

**Relations:**
- totalFolders — *requires* — second aggregate call
- second aggregate call — *is needed because* — $facet pipeline
- $facet pipeline — *is bound to* — one collection
- totalFolders — *was added to* — luz_docs_statistic
- totalFolders — *could not join* — documents $facet
- $facet's sub-pipelines — *run over* — same input collection
- per-tenant folder count — *requires* — separate aggregate call
- separate aggregate call — *is against* — folders collection
- job — *runs* — two aggregates per tenant
- two aggregates — *use* — same cached tenant token
- one aggregate — *is* — documents $facet
- documents $facet — *calculates* — counts
- documents $facet — *calculates* — averageFoldersPerDocument
- averageFoldersPerDocument — *is calculated via* — $avg of $size($ifNull(folderIds,[]))
- second aggregate call — *is* — minimal aggregate pipeline
- minimal aggregate pipeline — *operates on* — folders
- minimal aggregate pipeline — *is extracted by* — DocumentStatisticUtils.extractCount
- DocumentStatisticUtils.extractCount — *handles* — null result
- null result — *implies* — 0 folders
- MongoDBService.aggregate — *previously hardcoded* — documents collection
- MongoDBService.aggregate — *now has* — collection-parameterized overload
- old 3-arg method — *delegates to* — collection-parameterized overload
- averageFoldersPerDocument — *averages over* — non-being-created documents
- non-being-created documents — *includes* — deleted documents
- missing folderIds — *is counted as* — 0 folders
- luz_docs_statistic — *updates stats via* — EJB timer
- luz_docs_statistic — *updates stats via* — PubSub
- luz_docs_statistic — *updates stats via* — $facet aggregation
- luz_docs_statistic — *is related to* — luz_docs_statistic updates stats via 1-minute EJB timer over PubSub and $facet aggregation

%% ai-graph-end %%