---
ai_hash: 6ef327fc1833112a
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-07-09
entities:
- luz_docs
- Kepler
- count>N UI badge
- HyperLogLog
- fuzzy-zone fallback
- 500ms target
- 128k+ docs
- cosmetic threshold badge
- predicate
- precomputed dimensions
- folder id
- tag
- security-class code
- _isPublic
- HLL sketch
- write-path pattern
- _effectiveSecurityClassCodes
- _shard
- materialize pipeline
- arbitrary/ad hoc/free-text filter
- index
- $text
- trigram field
- estimate-vs-N comparison
- fuzzy zone
- raw point comparison
- exact capped count
- 'limit: N+1'
- luz_docs codebase
- docs/bitmap-count-investigation.md
- docs/perf-LUZ-154613-count-scaling-findings-and-solution.md
- security-facing visible-document count
- ~1-2% error
- access-leak-shaped
- cosmetic ">999" display badge
- security count
- HyperLogLog error in the small-range (linear-counting) regime
- luz_docs documentscount is scan-bound and cannot reach sub-second at 128k
source: luz_docs count-estimate research, 2026-07-09, branch kepler/sprint-159/LUZ-154613-shard-adapt-migraiton
status: seedling
tags:
- luz-docs
- hyperloglog
- kepler
- count-optimization
- design-decision
title: luz_docs count>N badge can use HyperLogLog with a fuzzy-zone fallback
type: observation
---

# luz_docs count>N badge can use HyperLogLog with a fuzzy-zone fallback

For luz_docs (Kepler), the question was whether a `count(query) > N` UI badge (e.g. flipping to "999+") could use a HyperLogLog sketch to stay under a 500ms target even at 128k+ docs.

Decision: HLL is a legitimate fit for this *cosmetic* threshold badge, but only under two conditions:

1. **The predicate must decompose into a small, fixed set of precomputed dimensions** (folder id, tag, security-class code, `_isPublic`) — each maintained as its own HLL sketch, updated O(1) per write (same write-path pattern already used for `_isPublic`/`_effectiveSecurityClassCodes`/`_shard` in the materialize pipeline). A merged HLL only answers "how many docs are in the union of these precomputed sketches" — it cannot handle an arbitrary/ad hoc/free-text filter, because you can't precompute a sketch per every string a user might type, and building one at query time costs the same scan you were trying to avoid. Ad hoc full-text counts need an index ($text or trigram field), not a sketch.

2. **The estimate-vs-N comparison must use a fuzzy zone**, not a raw point comparison — see [[HyperLogLog error in the small-range (linear-counting) regime]] for why a single estimate can't safely decide a close boundary. E.g.: estimate < 0.95N -> show as-is; estimate > 1.05N -> show "N+"; in between -> fall back to an exact capped count (`limit: N+1`).

This is a narrowing of an existing, already-decided rule in the luz_docs codebase (see docs/bitmap-count-investigation.md and docs/perf-LUZ-154613-count-scaling-findings-and-solution.md): HLL is explicitly *rejected* for the security-facing visible-document count, because its ~1-2% error is access-leak-shaped (could under-count and hide a document a user should see, or the reverse). A cosmetic ">999" display badge is a different, lower-stakes question than "can this user see this document" — that's what makes HLL acceptable here when it wasn't for the security count.

## Related

- [[HyperLogLog error in the small-range (linear-counting) regime]]
- [[luz_docs documentscount is scan-bound and cannot reach sub-second at 128k]]

%% ai-graph-start %%

**Related notes:**
- [[luz_docs documentscount is scan-bound and cannot reach sub-second at 128k]]
- [[HyperLogLog error in the small-range (linear-counting) regime]]
- [[Visible-document count as cardinality of a bitmap union]]
- [[Count-scaling path fan-out first, Roaring next, HyperLogLog for approximate]]
- [[Frozen JsonStore gateway makes _id-range count fan-out a dead end — pivot to bitmapHLL]]

**Relations:**
- luz_docs — *is also known as* — Kepler
- count>N UI badge — *is for* — luz_docs
- count>N UI badge — *can use* — HyperLogLog
- count>N UI badge — *can use* — fuzzy-zone fallback
- HyperLogLog — *aims for* — 500ms target
- HyperLogLog — *operates at* — 128k+ docs
- count>N UI badge — *is a type of* — cosmetic threshold badge
- HyperLogLog — *is a fit for* — cosmetic threshold badge
- predicate — *decomposes into* — precomputed dimensions
- precomputed dimensions — *includes* — folder id
- precomputed dimensions — *includes* — tag
- precomputed dimensions — *includes* — security-class code
- precomputed dimensions — *includes* — _isPublic
- precomputed dimensions — *maintained as* — HLL sketch
- HLL sketch — *updated by* — write-path pattern
- write-path pattern — *used for* — _isPublic
- write-path pattern — *used for* — _effectiveSecurityClassCodes
- write-path pattern — *used for* — _shard
- _isPublic — *is part of* — materialize pipeline
- _effectiveSecurityClassCodes — *is part of* — materialize pipeline
- _shard — *is part of* — materialize pipeline
- HyperLogLog — *cannot handle* — arbitrary/ad hoc/free-text filter
- arbitrary/ad hoc/free-text filter — *needs* — index
- index — *can be* — $text
- index — *can be* — trigram field
- estimate-vs-N comparison — *uses* — fuzzy zone
- fuzzy zone — *is preferred over* — raw point comparison
- fuzzy zone — *is explained by* — HyperLogLog error in the small-range (linear-counting) regime
- fuzzy zone — *can trigger* — exact capped count
- exact capped count — *uses* — limit: N+1
- luz_docs codebase — *contains document* — docs/bitmap-count-investigation.md
- luz_docs codebase — *contains document* — docs/perf-LUZ-154613-count-scaling-findings-and-solution.md
- HyperLogLog — *rejected for* — security-facing visible-document count
- HyperLogLog — *has* — ~1-2% error
- ~1-2% error — *is* — access-leak-shaped
- count>N UI badge — *is different from* — security-facing visible-document count
- HyperLogLog — *acceptable for* — count>N UI badge
- security-facing visible-document count — *is also known as* — security count
- count>N UI badge — *is a type of* — cosmetic ">999" display badge

%% ai-graph-end %%