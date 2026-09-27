---
ai_hash: b87ce35720fca2f2
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-27
entities: []
source: session 2026-09-27 Confluence export
status: seedling
tags:
- classification
- keywords
- ranking
- gotcha
- heuristics
title: Keyword classifiers assign topics by lexicon size unless you normalize
type: gotcha
---

# Keyword classifiers assign topics by lexicon size unless you normalize

When a keyword classifier picks a category by `argmax(match count)`, the category with the **longest term list wins** — not the category that actually fits. More terms means more chances to match, so lexicon size silently becomes the dominant feature.

I hit this ranking 17,373 Confluence pages into six topics. My `infra` list had ~60 terms (kubernetes, gcp, docker, mongodb, deployment…) while `architecture` had ~20. The result was absurd on its face:

```
topic mix: {infra: 454, programming: 304, testing: 285, security: 100, ai_ml: 12, architecture: 11}
```

Eleven architecture pages in a corpus full of design documents. The pages were being *found* — they just lost the argmax to `infra`, because any page mentioning a database or a deployment matched more infra terms than architecture terms.

**The fix is one division:** score each topic against the size of its own lexicon before comparing.

```js
const rawT = titleHits * 3 + Math.min(excerptHits, 6);
perTopic[topic] = rawT / Math.sqrt(termList.length);   // <- normalise, then argmax
```

`sqrt` rather than plain length: dividing by the full count over-corrects and hands every tie to the shortest list. After normalising, architecture went 11 → 60 and the ordering matched what the pages actually were.

**Two related traps in the same scorer:**

- **Absolute scores are not comparable across categories either.** If you threshold on the raw score ("keep anything above 8"), long-lexicon categories clear the bar more easily. Normalise before thresholding, not just before argmax.
- **Term frequency in the corpus matters as much as list length.** In a corpus where every page says "deployment", that term carries almost no information. This is exactly what IDF weighting solves; if you are hand-rolling, at least keep the lists roughly equal in size and strip terms that appear nearly everywhere.

**The general rule:** any time you compare scores computed from differently-sized feature sets, you are comparing set sizes unless you explicitly divide them out. The symptom is a category that is implausibly rare or implausibly dominant — check the lexicon lengths before you start tuning weights.

## Related

- [[Confluence CQL search paginates by opaque cursor, not start offset]]

%% ai-graph-start %%

**Related notes:**
- [[LLM query enrichment for a substring-OR matcher must contract, not expand, the token set]]
- [[OCR body text dominates a full-text trigram index]]
- [[LLM-as-reranker JSON truncation budget max_tokens for pretty-printed output, not just element count]]

%% ai-graph-end %%