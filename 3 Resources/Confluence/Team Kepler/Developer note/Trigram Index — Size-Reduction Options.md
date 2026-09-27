---
ai_hash: 19641f98e685ab7c
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
confluence_id: '49550557203'
confluence_path: Team Kepler > Developer note > Full‑Text Document Search — Performance
  Analysis & Proposals > S2 — Materialized n‑gram / Trigram Field (native, self‑hosted,
  keeps substring semantics)
created: 2026-07-01
entities: []
source: Confluence · TK - Team Kepler
status: reference
tags:
- confluence
- performance
- search
title: Trigram Index — Size-Reduction Options
type: source
updated: 2026-07-01
url: https://axonivy.atlassian.net/wiki/spaces/TK/pages/49550557203/Trigram+Index+Size-Reduction+Options
---

# Trigram Index — Size-Reduction Options

*Confluence source · Team Kepler › Developer note › Full‑Text Document Search — Performance Analysis & Proposals › S2 — Materialized n‑gram / Trigram Field (native, self‑hosted, keeps substring semantics) · [view original](https://axonivy.atlassian.net/wiki/spaces/TK/pages/49550557203/Trigram+Index+Size-Reduction+Options) · updated 2026-07-01*

## Problem

- The trigram index works (see [Trigram Search — Performance-Env Benchmark](https://axonivy.atlassian.net/wiki/spaces/TK/pages/49545969673/Trigram+Search+Performance-Env+Benchmark) ), but index size is **~5.6 GB @ 512k docs — 98% of all index storage**, growing ~linearly (~11 MB / 1 000 docs).

- Root cause: full OCR body is in the trigram blob, and index entries = **Σ(distinct trigrams per doc)**.

- Baseline for all estimates: 512k docs, 100-paragraph lorem body, current index size is **5,714 MB**, ~2500 distinct trigrams/doc, ~4.7 B/entry compressed.

## Solutions

### Hashed / packed trigram keys

Store each trigram as a fixed-width integer hash instead of the 3-char string.

```
"tra" → hash32("tra") → 0x4F2A  (store the int, not the string)
query "traw" → ["tra","raw"] → [0x4F2A, 0x91BC] → $all:[0x4F2A,0x91BC]
```

#### What "hash" means here

- One trigram (the normalized 3-char window) → `java.lang.String.hashCode()` → a Java `int` (signed 32-bit).

- `String.hashCode()` is **specified by the JLS** (`s[0]*31^(n-1) + … + s[n-1]`), so it is **stable across JVMs and versions** — safe to persist long-term. (Unlike `Object.hashCode`, which is identity-based and must never be stored.)

- The hash space is 2³². The folded-ASCII trigram universe is tiny (~40³ ≈ 64 k possible trigrams), so collision probability across a document is negligible.

- **Collisions are harmless by design.** Two distinct trigrams mapping to the same int only add a *false positive* to the candidate set. The residual regex (already in the pipeline) rejects it. No recall loss

![[image-20260701-053950.png]]

### Bounded bucketed hashing

#### Mechanism

For each normalized trigram `t`:

```
h      = t.hashCode()            // signed int32  (Option B stopped here)
bucket = Math.floorMod(h, K)     // in [0, K)     (Option C folds into K buckets)
```

`_searchTrigrams` = the **distinct bucket ids** touched by the document's trigrams. A `LinkedHashSet<Integer>` dedups them, so a doc whose body touches every bucket stores exactly K entries.

> `Math.floorMod`**, not** `%`**.** Java `%` keeps the sign of the dividend, so `-7 % 1024 = -7` (negative bucket id). `String.hashCode()` is frequently negative, so `%` would produce out-of-range, sign-inconsistent buckets — and write vs query must agree on the exact value. `Math.floorMod(h, K)` always returns `[0, K)`. This is the single easy-to-miss correctness bug in C.

The search term decomposes identically: term → normalized trigrams → `floorMod(hash, K)` → **distinct buckets** → `$all: [b1, b2, …]`. Same K, same fold function on both sides (guaranteed because both call `TrigramGenerator`)

**Saturation is the whole point — and the whole risk**

- **Short strings** (metadata: title, fileName, sender…) touch few buckets → the doc's bucket set is small and *distinctive* → `$all` is selective.

- **Long bodies** touch *most* buckets → the bucket set saturates toward the full `[0,K)` → `$all` over a query's few buckets is *almost always satisfied* → the prefilter stops eliminating candidates and the residual regex is doing nearly all the work.

That is the fundamental tension: bucketing shrinks the index precisely *because* it throws away distinguishing information, and a saturated body has thrown away enough that the filter is weak. So K must be large enough that the *body* doesn't saturate it into uselessness, yet small enough to shrink the index. This is why C is benchmarked,

#### Correctness — no recall loss, more false positives

**Recall is preserved (necessary condition still holds).** If string `S` contains substring `q` (\|q\| ≥ 3), then `trigrams(q) ⊆ trigrams(S)`. Bucketing is a function, so `buckets(q) ⊆ buckets(S)`. `$all` over `buckets(q)` is therefore satisfied by any `S` that truly contains `q`. **No true match is ever filtered out.** The residual regex then confirms the exact substring.

**False positives increase (two independent sources):**

1.  **Hash collisions** — two distinct trigrams → same int (inherited from B; negligible).

2.  **Bucket collisions** — two distinct trigrams → same bucket (the dominant new source; grows as K shrinks). Many trigrams share a bucket, so `$all` over buckets is a *coarser* test than `$all` over exact trigrams.

Both only enlarge the **candidate set**; neither drops a real hit. Cost is paid entirely at query time in the residual regex over a bigger candidate set — never in recall.

![[image-20260701-054044.png]]![[image-20260701-054103.png]]

%% ai-graph-start %%

**Related notes:**
- [[S2 — Materialized n‑gram - Trigram Field (native, self‑hosted, keeps substring semantics)]]
- [[Bounded bucketed hashing caps trigram index entries per document]]
- [[Multikey ngram index size is driven by distinct-entry count, not bytes per entry]]
- [[OCR body text dominates a full-text trigram index]]
- [[Trigram index makes substring search indexable filter by 3-grams, then verify by regex]]

%% ai-graph-end %%