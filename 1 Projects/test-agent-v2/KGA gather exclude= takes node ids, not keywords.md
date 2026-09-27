---
ai_hash: 1f0294c350e17793
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-13
entities:
- KGA
- gather_knowledge
- exclude=
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
- LUZ-158230
- jira:LUZ-158230
- '49665769474'
- confluence:49665769474
- ZIP import
- memory-bleed
- fresh context
- new gather
- Post-hoc exclude
- already-converged context
- cached rounds
- expansion
- crawl
- memory seeder
- atlassian seeder
- lead seeder
- semantic recall
- gather_codebase
- axonivy-prodrepo workspace slug
- axonivy-prod/<repo> workspace slug
- whitespace/commas
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

- [[gather_codebase needs axonivy-prodrepo workspace slug|gather_codebase needs axonivy-prod/<repo> workspace slug]]

%% ai-graph-start %%

**Related notes:**
- [[exclude= does not lift PQS precision because cloud-discover re-promotes 8 services]]
- [[gather_codebase on a mid-refine context routes into the refine loop]]
- [[gather_codebase needs axonivy-prodrepo workspace slug]]
- [[Agent Loop 1 - Knowledge Gathering - v2]]
- [[A link-following crawl pulls in graph-adjacent but topically-tangential nodes]]

**Relations:**
- KGA — *uses* — gather_knowledge
- gather_knowledge — *has parameter* — exclude=
- exclude= — *takes* — canonical node ids
- exclude= — *does not take* — keywords
- gather_knowledge — *is part of* — test-agent-v2
- keywords — *are also known as* — free-text topics
- exclude_ids() — *splits string on* — whitespace/commas
- exclude_ids() — *processes tokens with* — normalize_seed
- normalize_seed — *handles* — Jira issue keys
- normalize_seed — *handles* — Confluence numeric page ids
- normalize_seed — *handles* — URLs
- Jira issue keys — *normalize to example* — jira:LUZ-158230
- LUZ-158230 — *is an example of* — Jira issue keys
- Confluence numeric page ids — *normalize to example* — confluence:49665769474
- 49665769474 — *is an example of* — Confluence numeric page ids
- ZIP import — *normalizes to* — itself
- ZIP import — *matches no* — canonical node ids
- exclude= — *prunes* — memory-bleed
- exclude= — *requires* — canonical node ids
- exclude= — *applies to* — fresh context
- fresh context — *is a* — new gather
- Post-hoc exclude — *on already-converged context is a* — no-op
- Post-hoc exclude — *replays* — cached rounds
- exclude= — *is applied to* — expansion
- exclude= — *is applied to* — crawl
- exclude= — *is applied to* — memory seeder
- exclude= — *is applied to* — atlassian seeder
- exclude= — *is applied to* — lead seeder
- canonical node ids — *are kept out of* — semantic recall
- gather_codebase — *needs* — axonivy-prodrepo workspace slug
- axonivy-prodrepo workspace slug — *is a type of* — axonivy-prod/<repo> workspace slug

%% ai-graph-end %%