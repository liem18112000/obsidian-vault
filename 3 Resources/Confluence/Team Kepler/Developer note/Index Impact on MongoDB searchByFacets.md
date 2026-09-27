---
ai_hash: 7b8e60d84e75ca80
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
confluence_id: '49538695172'
confluence_path: Team Kepler > Developer note > Facet Count Fan-out Techniques in
  MongoDB
created: 2026-06-26
entities: []
source: Confluence · TK - Team Kepler
status: reference
tags:
- confluence
- mongodb
- search
title: Index Impact on MongoDB searchByFacets
type: source
updated: 2026-06-26
url: https://axonivy.atlassian.net/wiki/spaces/TK/pages/49538695172/Index+Impact+on+MongoDB+searchByFacets
---

# Index Impact on MongoDB searchByFacets

*Confluence source · Team Kepler › Developer note › Facet Count Fan-out Techniques in MongoDB · [view original](https://axonivy.atlassian.net/wiki/spaces/TK/pages/49538695172/Index+Impact+on+MongoDB+searchByFacets) · updated 2026-06-26*

## Overview

**Question:** Can a correct/relevant MongoDB index significantly boost the facet-count path at API document search (`searchByFacets`)?

------------------------------------------------------------------------

### Actual aggregation pipeline `getFacets` builds (in order)

```
[ $match  securityClass / materialize match ]      ← leading, INDEXABLE
[ $match  skipBeingCreated ]                        ← leading, INDEXABLE
[ $match  credentialMappings ]                      ← leading, INDEXABLE
[ $match  field {$exists:true} + deletionStatus ]   ← INDEXABLE (weak selectivity)
[ $unwind ]                                          ← array facets only; kills downstream index
[ $group  _id:$field, count:{$sum:1} ]              ← THE COUNT. not index-served
[ $sort   _id.<field> | count ]                      ← POST-group, in-memory, NEVER indexed
[ $skip ]
[ $limit ]
```

------------------------------------------------------------------------

### MongoDB index rules that govern this pipeline

1.  **Index used only for stages BEFORE the first** `$group`**/**`$unwind`**/**`$lookup`**.**

    - Mongo uses an index for `$match`/`$sort` only at the *front* of a pipeline.

    - After a transforming stage, later stages run on in-memory intermediate docs.

2.  `$group` **count is NOT index-served.**

    - Every doc surviving the `$match` prefix is read and counted (`$sum:1`).

    - Cost = O(matched docs), independent of index.

    - No `$match` exists after `$group`, so nothing downstream prunes.

3.  **Post-group** `$sort` **(**`buildSortFacet`**) can never use an index**

    - Sorts grouped output in memory.

4.  **COLLATION KILLER (dominant factor).**

    1.  Pipeline runs with `COLLATION_DEFAULT = {locale:"en", caseFirst:"UPPER"}`

    2.  MongoDB rule: an index is usable for a collated string operation only if the index carries the *same* collation.

    3.  A default/binary index is **silently skipped → COLLSCAN**.

------------------------------------------------------------------------

### 4. Does an index boost `searchByFacets`?

|  |  |  |
|----|----|----|
| Scenario | Boost? | Why |
| Selective leading `$match` (narrow filter, security-class subset) | **Big yes** | Collated compound index avoids COLLSCAN, feeds a small set to `$group` |
| Compound index contains all match fields **+** facet field | **Yes (constant factor)** | Covered IXSCAN — skips FETCH of full BSON to feed `$group`, even at low selectivity |
| Facet over whole tenant (eArchive: `deletionStatus:false` ≈ all docs) | **Little / none** | `$group` traverses ~every doc; IXSCAN over ~all keys ≈ COLLSCAN cost. Index is the ceiling, not a fix |
| Non-materialized security path (`$lookup` folders) | **No** | Per-doc join, unprunable on documents side — *why* materialize (`_effectiveSecurityClassCodes`) was built to drop the `$lookup` |
| Array facet (`$unwind`) | **No downstream** | `$unwind` kills index for everything after it |

%% ai-graph-start %%

**Related notes:**
- [[Facet Count Fan-out Techniques in MongoDB]]
- [[Mongo facet $group count index only helps the $match prefix, not the count]]
- [[An index only helps an aggregation before the first group, unwind, or lookup]]
- [[MongoDB $facet buckets add no parallelism and defeat COUNT_SCAN]]
- [[07 Aggregation Pipeline]]

%% ai-graph-end %%