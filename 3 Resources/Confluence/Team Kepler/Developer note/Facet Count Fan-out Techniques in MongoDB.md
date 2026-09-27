---
title: "Facet Count Fan-out Techniques in MongoDB"
created: 2026-06-26
updated: 2026-06-26
type: source
status: reference
source: "Confluence · TK - Team Kepler"
url: https://axonivy.atlassian.net/wiki/spaces/TK/pages/49537843344/Facet+Count+Fan-out+Techniques+in+MongoDB
confluence_id: "49537843344"
confluence_path: "Team Kepler > Developer note"
tags: [confluence, mongodb]
---

# Facet Count Fan-out Techniques in MongoDB

*Confluence source · Team Kepler › Developer note · [view original](https://axonivy.atlassian.net/wiki/spaces/TK/pages/49537843344/Facet+Count+Fan-out+Techniques+in+MongoDB) · updated 2026-06-26*

## **Question**

1.  Where does `searchByFacets` get its `count`?

2.  Can an index significantly boost it?

3.  Can the count fan-out technique help?

## **TL;DR:**

- Facet `count` = `$group {count:{$sum:1}}` in a Mongo aggregation.

- An index boosts **only the leading** `$match` **prefix**

- The count fan-out parallelizes the unavoidable scan ~K× (cache-resident regime)

## What an index can touch

**MongoDB rules:**

1.  Index used only for `$match`/`$sort` **before** the first `$group`/`$unwind`/`$lookup`.

2.  The `$group` count reads every surviving doc — O(n), unindexable.

3.  Post-group `$sort` is in-memory, never indexed.

4.  **Collation killer:** pipeline runs with `COLLATION_DEFAULT = {locale:"en", caseFirst:"UPPER"}`

5.  A string index is used only if it carries the **same** collation — else silently skipped → COLLSCAN.

|  |  |  |
|----|----|----|
| Scenario | Index boost? | Why |
| Selective leading `$match` | **Big** | Collated compound index avoids COLLSCAN, feeds small set to `$group` |
| Index covers match fields + facet field | **Constant factor** | Covered IXSCAN feeds `$group` without FETCH |
| Facet over whole tenant (count-all) | **~None** | `$group` traverses ~every doc; IXSCAN over ~all keys ≈ COLLSCAN |
| Non-materialized security `$lookup` | **No** | Per-doc join, unprunable — why materialize path drops the `$lookup` |
| Array facet (`$unwind`) | **No downstream** | `$unwind` kills index after it |

## The count fan-out

`baseQuery` fanning into K disjoint `_shard` ranges (first open-below, last open-above), each a concurrent sub-count, converging to `Σ partial = total` — plus the red `_shard`-index precondition and the green cache-resident caveat.

|  |  |  |
|----|----|----|
|  | Index | Fan-out |
| Reduce docs read | only if `$match` selective | **No** — same O(n) total work |
| Parallelize the O(n) scan | No | **Yes** — ~K× wall-clock, cache-resident only |
| Help count-all (low selectivity) | ~No | **Yes**, until disk-bound wall (K scans blow cache → K stops helping) |

- Engine is **count-only**: `function` returns `int`. Facet buckets are a `JsonArray` → can't flow through.

- **To parallelize facets** you'd need: per-partition `$group`, then a **per-bucket merge** (sum `count` keyed by `key` across K results), then relocate `$sort`/`$skip`/`$limit` to the merge layer (top-N per partition ≠ global top-N).

<!-- -->

- 🔴 `_shard` **index required.** `shardRangeClause` = `_shard >= lo AND _shard < hi`.

- 🟠 **Gate probe** `{_shard:{$exists:false}}` **is non-indexable**

![[image-20260626-073533.png]]
