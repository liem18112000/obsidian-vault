---
ai_hash: 64eefbc18496d3c8
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-13
entities:
- KGA
- gather_knowledge
- exclude= parameter
- node ids
- keywords
- test-agent-v2
- canonical node ids
- free-text topics
- exclude_ids()
- normalize_seed
- Jira issue keys
- Confluence numeric page ids
- URLs
- ZIP import
- memory-bleed
- prior gather output
- fresh context
- new gather
- Post-hoc exclude
- already-converged context
- cached rounds
- expansion
- crawl
- memory/atlassian/lead seeders
- semantic recall
- gather_codebase
- axonivy-prod/<repo> workspace slug
source: session 2026-09-13 LUZ-156281 demo
status: seedling
tags:
- kga
- gather
- exclude
- test-agent-v2
- gotcha
- memory-bleed
title: KGA gather exclude= takes node ids, not keywords
type: lesson
---

# KGA gather exclude= takes node ids, not keywords

The KGA `gather_knowledge(exclude=...)` parameter (test-agent-v2) takes **canonical node ids**, not free-text topics. `exclude_ids()` splits the string on whitespace/commas and runs each token through `normalize_seed`, so you must pass things that normalise to real node ids:

- Jira issue keys → `LUZ-158230` (becomes `jira:LUZ-158230`)
- Confluence numeric page ids → `49665769474` (becomes `confluence:49665769474`)
- URLs → the URL

A plain phrase like `"ZIP import"` "normalises to itself and matches no node" — it silently excludes nothing. So to prune a memory-bleed you must enumerate the specific bled node ids (read them from the prior gather output), e.g. `exclude="LUZ-158230 LUZ-158243 49665769474"`.

Also: `exclude=` only bites on a **fresh context** (a new gather). Post-hoc exclude on an already-converged context replays cached rounds and is a no-op. And exclude is applied to expansion + crawl + the memory/atlassian/lead seeders, so excluded ids are kept out of semantic recall too.

## Related

- [[gather_codebase needs axonivy-prod/<repo> workspace slug]]

%% ai-graph-start %%

**Related notes:**
- [[exclude= does not lift PQS precision because cloud-discover re-promotes 8 services]]
- [[gather_codebase on a mid-refine context routes into the refine loop]]
- [[gather_codebase needs axonivy-prodrepo workspace slug]]
- [[A link-following crawl pulls in graph-adjacent but topically-tangential nodes]]
- [[Drive the KGA A2A agent offline via Starlette TestClient for evaluation]]

**Relations:**
- KGA — *uses* — gather_knowledge
- gather_knowledge — *has parameter* — exclude= parameter
- exclude= parameter — *takes* — node ids
- exclude= parameter — *does not take* — keywords
- gather_knowledge — *is part of* — test-agent-v2
- node ids — *are also known as* — canonical node ids
- keywords — *are also known as* — free-text topics
- exclude_ids() — *uses* — normalize_seed
- normalize_seed — *converts* — Jira issue keys
- normalize_seed — *converts* — Confluence numeric page ids
- normalize_seed — *processes* — URLs
- ZIP import — *is an example of* — free-text topics
- ZIP import — *matches no* — node
- exclude= parameter — *prunes* — memory-bleed
- exclude= parameter — *requires* — specific bled node ids
- specific bled node ids — *from* — prior gather output
- exclude= parameter — *is effective on* — fresh context
- fresh context — *is a* — new gather
- Post-hoc exclude — *on* — already-converged context
- Post-hoc exclude — *replays* — cached rounds
- exclude= parameter — *applies to* — expansion
- exclude= parameter — *applies to* — crawl
- exclude= parameter — *applies to* — memory/atlassian/lead seeders
- exclude= parameter — *prevents* — semantic recall
- gather_knowledge — *is related to* — gather_codebase
- gather_codebase — *needs* — axonivy-prod/<repo> workspace slug

%% ai-graph-end %%