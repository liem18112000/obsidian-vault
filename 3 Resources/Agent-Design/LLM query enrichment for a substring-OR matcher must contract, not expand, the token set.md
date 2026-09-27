---
ai_hash: b28413b3d9cd48db
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-03
entities:
- LLM query enrichment
- substring-OR matcher
- token set
- LLM
- query
- retriever
- whitespace tokens
- OR-clauses
- matches
- recall
- precision
- Fix pattern
- specific terms
- proper nouns
- code identifiers
- unique feature names
- broad generic words
- structural cap
- precision terms
- corpus
- class name
- code-graph nodes
- Jira titles
- selectivity
- index's domain distribution
- domain-saturated index
- eArchive-import content
- import token
- earchive token
- actual corpus
- phrase/AND/rarity-weighted matching
- test-agent KGA self-exploration
- G2 hypothesize step
- G0/G1
- 'Retrieval tiering: query knowledge sources cheapest and most-trusted first'
- External LLM output is a lead generator
- External LLM output is not a source of truth
- document
- system
- data
- service
- UI
- component
- validation
- structure
source: session 2026-09-03 — test-agent KGA G2
status: seedling
tags:
- agent-design
- rag
- retrieval
- llm
- search
- precision
title: LLM query enrichment for a substring-OR matcher must contract, not expand,
  the token set
type: lesson
---

# LLM query enrichment for a substring-OR matcher must contract, not expand, the token set

If a retriever matches a query by splitting it into whitespace tokens and OR-ing per-token substring hits (match if ANY token is a substring of a node id/title/type), then **enriching the query with an LLM must CONTRACT the token set, not expand it**. More tokens can only ADD OR-clauses → strictly MORE matches, never fewer. So an LLM that "focuses semantically" by emitting exhaustive concept lists (e.g. 12 title words → 66 terms) will *broaden* recall and hurt precision, the opposite of the intent.

**Fix pattern:** constrain the LLM to a FEW (≈6) distinctive, specific terms — proper nouns, code identifiers, unique feature names — and ban broad generic words (document, system, data, service, UI, component, validation, structure, …). Add a **structural cap** (e.g. keep the first N=8 after dedup) so the token set is bounded regardless of what the model returns. Prefer precision terms that are rare in the corpus even if they match 0 today (e.g. a class name matches code-graph nodes, not Jira titles) — they add precision without inflating count.

**Corollary — selectivity is relative to the indexs domain distribution.** A term that *looks* specific can be broad in a domain-saturated index: in a 71-node index that is almost entirely eArchive-import content, the tokens `import` (21 hits) and `earchive` (26) each match ~a third of the index. Judge a terms selectivity against the actual corpus, not against how specific it reads in isolation. The real lever is which tokens you emit, and whether the matcher does substring-OR vs phrase/AND/rarity-weighted matching.

Surfaced building the test-agent KGA self-exploration (G2 hypothesize step feeding G0/G1). Related: [[Retrieval tiering: query knowledge sources cheapest and most-trusted first]], [[External LLM output is a lead generator, not a source of truth]].

## Related

- [[Retrieval tiering: query knowledge sources cheapest and most-trusted first]]
- [[External LLM output is a lead generator]]
- [[not a source of truth]]

%% ai-graph-start %%

**Related notes:**
- [[Retrieval tiering query knowledge sources cheapest and most-trusted first]]
- [[Converge an exploration loop on marginal yield (zero new items), not a fixed iteration count]]
- [[External LLM output is a lead generator, not a source of truth]]
- [[Cap-before-exclude parallelism recall trap]]
- [[LLM-as-reranker JSON truncation budget max_tokens for pretty-printed output, not just element count]]

**Relations:**
- LLM query enrichment — *targets* — substring-OR matcher
- LLM query enrichment — *must contract* — token set
- retriever — *matches* — query
- retriever — *uses* — whitespace tokens
- retriever — *uses* — OR-ing per-token substring hits
- tokens — *add* — OR-clauses
- OR-clauses — *increase* — matches
- LLM — *emitting exhaustive concept lists broadens* — recall
- LLM — *emitting exhaustive concept lists hurts* — precision
- Fix pattern — *constrains* — LLM
- Fix pattern — *recommends* — specific terms
- specific terms — *include* — proper nouns
- specific terms — *include* — code identifiers
- specific terms — *include* — unique feature names
- Fix pattern — *bans* — broad generic words
- broad generic words — *example* — document
- broad generic words — *example* — system
- broad generic words — *example* — data
- broad generic words — *example* — service
- broad generic words — *example* — UI
- broad generic words — *example* — component
- broad generic words — *example* — validation
- broad generic words — *example* — structure
- Fix pattern — *adds* — structural cap
- structural cap — *bounds* — token set
- Fix pattern — *prefers* — precision terms
- precision terms — *are rare in* — corpus
- class name — *matches* — code-graph nodes
- class name — *does not match* — Jira titles
- selectivity — *is relative to* — index's domain distribution
- term — *can be broad in* — domain-saturated index
- import token — *matches* — eArchive-import content
- earchive token — *matches* — eArchive-import content
- selectivity — *judged against* — actual corpus
- tokens — *influence* — selectivity
- matcher type — *influence* — selectivity
- matcher type — *includes* — substring-OR matcher
- matcher type — *includes* — phrase/AND/rarity-weighted matching
- LLM query enrichment — *surfaced during* — test-agent KGA self-exploration
- test-agent KGA self-exploration — *involves* — G2 hypothesize step
- G2 hypothesize step — *feeds* — G0/G1
- LLM query enrichment — *related to* — Retrieval tiering: query knowledge sources cheapest and most-trusted first
- LLM query enrichment — *related to* — External LLM output is a lead generator
- LLM query enrichment — *related to* — External LLM output is not a source of truth

%% ai-graph-end %%