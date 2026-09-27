---
ai_hash: 930b53a5d73d4811
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-27
entities:
- link-following crawl
- graph-adjacent but topically-tangential nodes
- Jira
- Confluence
- eArchive story
- LUZ-158390
- Agentic Framework
- LUZ-159671
- S2 Testing Agent
- LUZ-159670
- S1
- relates to
- is-blocked-by
- Depth
- Link-type scope
- Relevance pass
- knowledge refinement
- LLM relevance judge
- kga gather loop
- Knowledge-Gathering loop is a bounded frontier crawl with a verify edge
- in-scope edge
- semantic relevance
- nodes
- issue link
- issue-link type.name
- Jira payload
- scope config
- downstream judge
- gather loop
source: session 2026-08-27 — /gather LUZ-158390
status: seedling
tags:
- crawl
- graph
- relevance
- scope
- agentic
title: A link-following crawl pulls in graph-adjacent but topically-tangential nodes
type: lesson
---

# A link-following crawl pulls in graph-adjacent but topically-tangential nodes

A bounded link-following crawl (frontier over Jira/Confluence links) follows **every in-scope edge regardless of semantic relevance**, so it surfaces nodes that are graph-adjacent but off-topic. Real example: gathering the eArchive story LUZ-158390 pulled in the "Agentic Framework" tooling stories LUZ-159671 (S2 Testing Agent) and LUZ-159670 (S1) — because LUZ-159671 has a **"relates to" issue link** to LUZ-158390 (the Testing Agent USES that ticket as its crawl target), and LUZ-159670 came one hop further (159671 is-blocked-by 159670) only because depth=2.

Not a bug — the edges are real. But "related in the Jira graph" ≠ "relevant to the feature". Controls:
- **Depth** is the bluntest filter: depth 1 keeps only direct links; each extra hop widens topical drift fast.
- **Link-type scope**: treat edge kinds differently — follow `blocks`/`is blocked by` (true dependencies) but NOT `relates to` (associative). The issue-link `type.name` is available in the Jira payload, so classification can gate follow-vs-record per relationship.
- **Relevance pass**: a later step (knowledge refinement / an LLM relevance judge) flags off-topic nodes rather than the crawl trying to be smart.

Design takeaway: separate "reachable" from "relevant". The gather loop should be complete+cheap (follow structural edges, dedup, bound); relevance filtering belongs to scope config or a downstream judge, not the crawl.

Context: kga gather loop (LUZ-159671 test-agent).

## Related

- [[Knowledge-Gathering loop is a bounded frontier crawl with a verify edge]]

%% ai-graph-start %%

**Related notes:**
- [[Knowledge-Gathering loop is a bounded frontier crawl with a verify edge]]
- [[Extracting every link from Jira ADF and Confluence storage]]
- [[Converge an exploration loop on marginal yield (zero new items), not a fixed iteration count]]
- [[LLM query enrichment for a substring-OR matcher must contract, not expand, the token set]]
- [[KGA gather exclude= takes node ids, not keywords]]

**Relations:**
- link-following crawl — *pulls in* — graph-adjacent but topically-tangential nodes
- link-following crawl — *operates on* — Jira
- link-following crawl — *operates on* — Confluence
- LUZ-158390 — *is a* — eArchive story
- LUZ-159671 — *is part of* — Agentic Framework
- LUZ-159671 — *is a* — S2 Testing Agent
- LUZ-159670 — *is part of* — Agentic Framework
- LUZ-159670 — *is a* — S1
- LUZ-159671 — *relates to* — LUZ-158390
- LUZ-159671 — *is-blocked-by* — LUZ-159670
- Depth — *is a control for* — link-following crawl
- Link-type scope — *is a control for* — link-following crawl
- Relevance pass — *is a control for* — link-following crawl
- Relevance pass — *involves* — knowledge refinement
- Relevance pass — *involves* — LLM relevance judge
- kga gather loop — *is context for* — LUZ-159671
- Knowledge-Gathering loop is a bounded frontier crawl with a verify edge — *is related to* — kga gather loop
- link-following crawl — *follows* — in-scope edge
- in-scope edge — *can lack* — semantic relevance
- link-following crawl — *surfaces* — nodes
- nodes — *are* — graph-adjacent but off-topic
- S2 Testing Agent — *uses* — LUZ-158390
- Jira — *has* — issue link
- issue link — *has property* — type.name
- Jira payload — *contains* — issue-link type.name
- relevance filtering — *belongs to* — scope config
- relevance filtering — *belongs to* — downstream judge
- link-following crawl — *should not perform* — relevance filtering
- gather loop — *should be* — complete+cheap

%% ai-graph-end %%