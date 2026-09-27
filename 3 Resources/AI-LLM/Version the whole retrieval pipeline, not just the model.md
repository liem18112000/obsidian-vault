---
title: "Version the whole retrieval pipeline, not just the model"
created: 2026-09-27
type: lesson
status: seedling
source: "Confluence: IR - System Design (AI)"
tags: [rag, embeddings, reproducibility, evaluation, prompting, confluence-distilled]
---

# Version the whole retrieval pipeline, not just the model

A retrieval index is the output of a **pipeline**, not of a model. Chunking strategy, metadata-extraction prompts, enrichment steps and the embedding model all shape the vectors. Versioning only the model leaves you unable to answer the question that matters during any regression: *what produced this vector?*

The practice is to **version the entire pipeline definition** — chunking, metadata prompts, embedding model, and the rest — as one identifier.

**What that identifier buys:**

- **Reproducibility.** Given a stored vector and its pipeline version, you can regenerate it exactly. Without it, a vector is an artefact of whatever the code happened to be doing that week.
- **"What produced this?" becomes answerable.** When retrieval quality drops for documents indexed in a particular window, the pipeline version localises the change — and prompts are part of that, since a reworded summarisation prompt changes the indexed text as surely as a new model does.
- **Offline A/B evaluation of strategies.** You can build two indices under two pipeline versions and compare them on a fixed judged dataset, which is only meaningful if each side is a known, pinned configuration.

That last point pairs with **offline evaluation**: testing against a static, pre-collected dataset with human relevance judgements — repeatable, fast, and the right tool early in development. Repeatability is exactly what pipeline versioning provides; without it you cannot tell whether a score moved because the strategy improved or because something upstream drifted.

> [!tip] The prompt is part of the pipeline
> This is the piece teams most often leave unversioned. If an LLM writes the title, summary, context, keywords or entities that get indexed, then **the prompt is an input to the index** and deserves the same treatment as the model id. A prompt tweak that improves summaries subtly changes what is retrievable — silently, and only for documents indexed after the change.

> [!warning] Not every pipeline change costs the same
> A new **embedding model** forces a full rebuild, because the vector space changes. A new **chunking strategy** also forces one. A change to which metadata fields are *displayed* may need nothing. Record the version on everything, but know which changes are rebuild-triggering — see [[Changing the embedding model forces a full index rebuild]].

Source: [[IR - System Design]] (AI, Confluence).

## Related

- [[Changing the embedding model forces a full index rebuild]]
