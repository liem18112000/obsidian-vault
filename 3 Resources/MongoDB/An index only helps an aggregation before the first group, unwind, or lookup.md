---
ai_hash: 83caaaef37360544
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-27
entities: []
source: 'Confluence: Facet Count Fan-out Techniques in MongoDB (2026-06-26)'
status: seedling
tags:
- mongodb
- aggregation
- indexing
- performance
- query-planning
- luz-docs
title: An index only helps an aggregation before the first group, unwind, or lookup
type: concept
---

# An index only helps an aggregation before the first group, unwind, or lookup

In a MongoDB aggregation, an index can only be used for `$match` and `$sort` stages that appear **before the first `$group`, `$unwind`, or `$lookup`**. After that boundary the documents are synthetic — they no longer correspond to anything in an index — so every later stage is executed in memory.

Three consequences that explain most "why doesn't my index help?" puzzles:

- **A `$group` count is O(n) and unindexable.** `{count: {$sum: 1}}` has to touch every surviving document. If the `$match` selects nearly the whole tenant, an `IXSCAN` over almost all keys is no better than a `COLLSCAN` — you have optimised the wrong half of the pipeline.
- **A post-`$group` `$sort` is always in-memory**, and therefore subject to the 32 MB sort limit.
- **`$unwind` ends index usability for everything downstream**, which is why array facets cannot be index-accelerated past that point.

So the useful question is not "is there an index on this field?" but **"how much does the leading `$match` prefix eliminate before the first blocking stage?"** A selective leading match is a big win; a covered `IXSCAN` feeding `$group` without a `FETCH` is a constant-factor win; a count over the whole tenant is neither.

Corollary that justified materialising security fields in `luz-docs`: a per-document **`$lookup` cannot be pruned** — it is a join executed per surviving document. Removing the `$lookup` by denormalising the joined field onto the document is worth more than any index on it.

When the scan itself is unavoidable, the remaining lever is parallelism — splitting the base query into K disjoint `_shard` ranges and summing the partial counts.

## Related

- [[A MongoDB text index matches stemmed words, not substrings]]
- [[A string index is silently skipped when its collation differs from the query's]]

## Related

- [[A string index is silently skipped when its collation differs from the query's]]

%% ai-graph-start %%

**Related notes:**
- [[Mongo facet $group count index only helps the $match prefix, not the count]]
- [[A string index is silently skipped when its collation differs from the query's]]
- [[Index Impact on MongoDB searchByFacets]]
- [[Facet Count Fan-out Techniques in MongoDB]]
- [[A MongoDB text index matches stemmed words, not substrings]]

%% ai-graph-end %%