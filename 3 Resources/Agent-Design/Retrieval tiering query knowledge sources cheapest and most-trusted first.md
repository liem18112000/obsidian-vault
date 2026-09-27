---
ai_hash: ca8c2fba51cb5ceb
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-03
entities:
- Retrieval tiering
- Knowledge sources
- Agent
- Internal memory
- In-domain authoritative search
- External web
- External LLM
- Cost
- Trust
- Jira
- Confluence
- JQL
- CQL
- RAG
- Self-exploring agent
- Source triangulation
- Grounding gate
- Knowledge Gathering Agent
- Lead generator
- Source of truth
- Citation
- Targeted breadth
- Results in-domain
- Results trustworthy
source: session 2026-09-03 — self-exploring KGA design
status: seedling
tags:
- agent-design
- rag
- retrieval
- knowledge-gathering
title: 'Retrieval tiering: query knowledge sources cheapest and most-trusted first'
type: concept
---

# Retrieval tiering: query knowledge sources cheapest and most-trusted first

When an agent can consult several knowledge sources, query them in order of **increasing cost and decreasing trust**, spending an expensive/less-trusted tier only when the cheaper ones fall short. A typical ladder:

1. **Internal memory** — what the agent already gathered/verified. Free, already-trusted. Reuse beats re-fetch.
2. **In-domain authoritative search** — the org's own corpora (e.g. Jira/Confluence via JQL/CQL). Cheap, authoritative.
3. **External web** — verifiable but out-of-domain; must be **cited**.
4. **External LLM** — cheap breadth, but **unverified**; a lead generator only (see [[External LLM output is a lead generator, not a source of truth]]).

**Why it matters**
- **Bounds cost** — you do not pay for the expensive tiers on the many tickets the cheap tiers already answer.
- **Keeps results in-domain and trustworthy** — the authoritative sources dominate; the noisy tiers are last resort and gated.
- **Turns "one input" into targeted breadth** — each tier's hits become candidate seeds subject to the same scope/dedup/budget rules, so breadth never becomes unbounded drift.

This is the retrieval half of a **RAG / self-exploring agent**: hits from any tier are *promoted to seeds* and re-enter the deterministic crawl/fetch machinery unchanged. Pair with **source triangulation** (>=2 independent sources => high confidence) and a **grounding gate** on the untrusted tiers.

Surfaced designing the self-exploring Knowledge Gathering Agent (test-agent/docs/RESEARCH-self-exploring-knowledge-gather.md).

## Related

- [[External LLM output is a lead generator, not a source of truth]]

%% ai-graph-start %%

**Related notes:**
- [[External LLM output is a lead generator, not a source of truth]]
- [[LLM query enrichment for a substring-OR matcher must contract, not expand, the token set]]
- [[Converge an exploration loop on marginal yield (zero new items), not a fixed iteration count]]
- [[Ground-then-refine gathering grounds, refinement interprets and confirms]]
- [[Knowledge-Gathering loop is a bounded frontier crawl with a verify edge]]

**Relations:**
- Retrieval tiering — *queries* — Knowledge sources
- Retrieval tiering — *orders by* — increasing Cost
- Retrieval tiering — *orders by* — decreasing Trust
- Agent — *consults* — Knowledge sources
- Internal memory — *is a* — Knowledge sources
- Internal memory — *has property* — Free
- Internal memory — *has property* — already-trusted
- In-domain authoritative search — *is a* — Knowledge sources
- In-domain authoritative search — *has property* — Cheap
- In-domain authoritative search — *has property* — authoritative
- In-domain authoritative search — *uses* — Jira
- In-domain authoritative search — *uses* — Confluence
- In-domain authoritative search — *uses* — JQL
- In-domain authoritative search — *uses* — CQL
- External web — *is a* — Knowledge sources
- External web — *has property* — verifiable
- External web — *requires* — Citation
- External LLM — *is a* — Knowledge sources
- External LLM — *has property* — cheap breadth
- External LLM — *has property* — unverified
- External LLM — *is a* — Lead generator
- External LLM — *is not a* — Source of truth
- Retrieval tiering — *bounds* — Cost
- Retrieval tiering — *keeps* — Results in-domain
- Retrieval tiering — *keeps* — Results trustworthy
- Retrieval tiering — *enables* — Targeted breadth
- Retrieval tiering — *is part of* — RAG
- Retrieval tiering — *is part of* — Self-exploring agent
- RAG — *pairs with* — Source triangulation
- RAG — *pairs with* — Grounding gate
- Self-exploring agent — *pairs with* — Source triangulation
- Self-exploring agent — *pairs with* — Grounding gate
- Knowledge Gathering Agent — *is a type of* — Self-exploring agent

%% ai-graph-end %%