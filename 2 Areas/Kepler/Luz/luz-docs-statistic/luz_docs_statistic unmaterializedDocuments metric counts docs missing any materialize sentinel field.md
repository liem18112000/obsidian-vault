---
ai_hash: 90e374e535a7fdb8
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-06-11
entities:
- luz_docs_statistic
- unmaterializedDocuments
- metric
- document
- materialize sentinel field
- sprint 158
- LUZ-155460
- June 2026
- total
- archived
- deleted
- _isPublic
- _effectiveSecurityClassCodes
- _folderNames
- _folderSecurityClassCodes
- luz_docs
- materialize pass
- backfill progress
- tenant
- JsonStoreQueryUtil
- unmaterializedDocumentConditionsBuilder()
- DocumentStatisticUtils
- buildFacetArrays()
- PubSub
- $facet aggregation
- 1-minute EJB timer
source: LUZ-155460 implementation, session 2026-06-11
status: seedling
tags:
- luz
- luz-docs-statistic
- materialize
- earchive
- LUZ-155460
title: luz_docs_statistic unmaterializedDocuments metric counts docs missing any materialize
  sentinel field
type: concept
---

# luz_docs_statistic unmaterializedDocuments metric counts docs missing any materialize sentinel field

As of sprint 158 (LUZ-155460, June 2026) luz_docs_statistic tracks a fourth facet, `unmaterializedDocuments` (count only — the size variant was dropped), alongside total/archived/deleted. A document counts as **unmaterialized** when ANY of the four materialize sentinel fields is absent ($or of $exists:false):

- `_isPublic` — true iff doc has no own security codes AND at least one containing folder is code-free (or doc is in no folder)
- `_effectiveSecurityClassCodes` — union of doc's own codes + all folders' own∪inherited codes
- `_folderNames` — folder names parallel to folderIds order (missing folder → empty-string slot)
- `_folderSecurityClassCodes` — per-folder own∪inherited codes, parallel array to folderIds/_folderNames

These fields are written by luz_docs' materialize pass; the metric exists to monitor backfill progress — it should trend to 0 once the materialise rollout/backfill completes for a tenant. Filter lives in `JsonStoreQueryUtil.unmaterializedDocumentConditionsBuilder()`, facet wiring in `DocumentStatisticUtils.buildFacetArrays()`.

Related: [[luz_docs_statistic updates stats via 1-minute EJB timer over PubSub and $facet aggregation]]

## Related

- [[luz_docs_statistic updates stats via 1-minute EJB timer over PubSub and $facet aggregation]]

%% ai-graph-start %%

**Related notes:**
- [[Stale-materialized detection recomputes MaterializeCompute state via $lookup inside the statistic $facet]]
- [[luz_docs_statistic computes per-tenant unmaterializedDocuments count]]
- [[totalFolders needs a second aggregate because a $facet pipeline is bound to one collection]]
- [[luz_docs_statistic updates stats via 1-minute EJB timer over PubSub and $facet aggregation]]
- [[luz-docs parallelized count undercounts documents missing _shard]]

**Relations:**
- luz_docs_statistic — *tracks* — unmaterializedDocuments
- unmaterializedDocuments — *is a* — metric
- metric — *counts* — document
- document — *is unmaterialized if missing* — _isPublic
- document — *is unmaterialized if missing* — _effectiveSecurityClassCodes
- document — *is unmaterialized if missing* — _folderNames
- document — *is unmaterialized if missing* — _folderSecurityClassCodes
- _isPublic — *is a* — materialize sentinel field
- _effectiveSecurityClassCodes — *is a* — materialize sentinel field
- _folderNames — *is a* — materialize sentinel field
- _folderSecurityClassCodes — *is a* — materialize sentinel field
- luz_docs_statistic — *tracks facet* — total
- luz_docs_statistic — *tracks facet* — archived
- luz_docs_statistic — *tracks facet* — deleted
- unmaterializedDocuments — *introduced in* — sprint 158
- sprint 158 — *is identified by* — LUZ-155460
- sprint 158 — *occurred in* — June 2026
- luz_docs — *performs* — materialize pass
- materialize pass — *writes* — materialize sentinel field
- metric — *monitors* — backfill progress
- backfill progress — *completes for* — tenant
- JsonStoreQueryUtil — *contains method* — unmaterializedDocumentConditionsBuilder()
- unmaterializedDocumentConditionsBuilder() — *defines* — filter
- DocumentStatisticUtils — *contains method* — buildFacetArrays()
- buildFacetArrays() — *handles* — facet wiring
- luz_docs_statistic — *updates stats via* — 1-minute EJB timer
- luz_docs_statistic — *updates stats via* — PubSub
- luz_docs_statistic — *updates stats via* — $facet aggregation

%% ai-graph-end %%