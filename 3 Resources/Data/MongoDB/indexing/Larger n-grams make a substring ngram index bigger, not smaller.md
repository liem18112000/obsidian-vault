---
ai_hash: e84592c288e98da0
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-06-30
entities: []
source: luz_docs S2-index-size-options.md, 2026-06-30
status: seedling
tags:
- ngram
- trigram
- index
- gotcha
- luz-docs
title: Larger n-grams make a substring ngram index bigger, not smaller
type: counter-argument
---

# Larger n-grams make a substring ngram index bigger, not smaller

Reaching for **larger n-grams (4-grams instead of trigrams) to shrink** a substring index is **backwards**. Index entries = distinct n-grams per doc ([[Multikey ngram index size is driven by distinct-entry count, not bytes per entry]]). The number of *windows* in a length-L string is roughly the same for any n (L−n+1), but the **distinct n-gram space explodes**: ~37⁴ for 4-grams vs ~37³ for trigrams. Over a large body that approaches saturation, more of that larger space gets touched → **more distinct entries → bigger index**.

Larger n also raises the minimum query length (`|q|≥4` instead of ≥3), shrinking what's searchable.

So: larger n = bigger + less flexible. To shrink, go the other way (bucketing/capping/tokenizing), not bigger n.

From luz_docs S2-index-size-options.md, rejected option (Kepler eArchive).

## Related

- [[Multikey ngram index size is driven by distinct-entry count, not bytes per entry]]

%% ai-graph-start %%

**Related notes:**
- [[Multikey ngram index size is driven by distinct-entry count, not bytes per entry]]
- [[Bounded bucketed hashing caps trigram index entries per document]]
- [[A large secondary index hurts via working-set vs cache, not disk bytes]]
- [[OCR body text dominates a full-text trigram index]]
- [[luz-docs ngram search shipped code indexes the OCR body and prefilters fail-open]]

%% ai-graph-end %%