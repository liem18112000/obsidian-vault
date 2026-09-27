---
title: "KGA gather exclude= takes node ids, not keywords"
created: 2026-09-13
type: lesson
status: seedling
source: "session 2026-09-13 LUZ-156281 demo"
tags: [kga, gather, exclude, test-agent-v2, gotcha, memory-bleed]
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
