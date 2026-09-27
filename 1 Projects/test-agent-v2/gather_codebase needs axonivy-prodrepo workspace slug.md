---
ai_hash: 11f02430d9ace457
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-13
entities:
- gather_codebase
- axonivy-prodrepo
- axonivy-prod
- Testing-Agent
- test-agent-v2
- MCP tool
- workspace-qualified Bitbucket slug
- Bitbucket slug
- CodeGraphFetcher.fetch
- ident
- Bitbucket API
- luz_finance
- LUZ / dunning / billing codebase
- master
- luz_docs_import
- graphify cache
- v2 bucket
- gs://klara-nonprod-kga-v2-memory/memory/graphify/
- v1 bucket
- mt-receive-ai-agent-memory
- MCP foreground window
- Bitbucket
- CHANGE-2770
- Bitbucket app-password discovery endpoints
source: session 2026-09-13 LUZ-156281 demo
status: seedling
tags:
- codegraph
- bitbucket
- test-agent-v2
- gotcha
- gather
title: gather_codebase needs axonivy-prod/<repo> workspace slug
type: lesson
---

# gather_codebase needs axonivy-prod/<repo> workspace slug

The Testing-Agent (test-agent-v2) `gather_codebase(repo=...)` MCP tool expects a **workspace-qualified** Bitbucket slug `<workspace>/<repo>`. Internally `CodeGraphFetcher.fetch` does `ws, _, repo = ident.partition("/")`, so a bare slug like `luz_finance` becomes `ws="luz_finance", repo=""` → Bitbucket API 404 → the call returns `0 nodes, 1 gap: <slug>` (a silent grounding failure, not an error).

**Correct slug for the LUZ / dunning / billing codebase:** `axonivy-prod/luz_finance` (workspace is `axonivy-prod`; mainbranch `master`). Other repos live under the same workspace, e.g. `axonivy-prod/luz_docs_import`.

**Gotcha:** a `0 nodes, 1 gap: <repo>` result means the SLUG is wrong (or the graph could not be built), NOT that the repo is empty. Always pass `axonivy-prod/<repo>`.

Related: no graphify cache exists in the v2 bucket (`gs://klara-nonprod-kga-v2-memory/memory/graphify/` is empty) and the old v1 bucket `mt-receive-ai-agent-memory` is gone (404), so v2 must build the graph live from Bitbucket each time — a big repo like luz_finance can exceed the 120s MCP foreground window and continue in the background.

## Related

- [[Bitbucket app-password discovery endpoints deprecated (CHANGE-2770)]]

%% ai-graph-start %%

**Related notes:**
- [[gather_codebase on a mid-refine context routes into the refine loop]]
- [[Building KlaraLuz Ivy projects off-VPN by routing Maven through Google Artifact Registry]]
- [[KGA crawler fetches repo source files via client mixin plus NodeFetcher registered by kind]]
- [[How luz_docs_integration_test repo location is resolved on disk]]
- [[KGA gather exclude= takes node ids, not keywords]]

**Relations:**
- gather_codebase — *needs* — axonivy-prodrepo workspace slug
- gather_codebase — *needs* — axonivy-prod/<repo> workspace slug
- Testing-Agent — *is_also_known_as* — test-agent-v2
- gather_codebase — *is_a* — MCP tool
- gather_codebase — *expects* — workspace-qualified Bitbucket slug
- workspace-qualified Bitbucket slug — *format_is* — <workspace>/<repo>
- CodeGraphFetcher.fetch — *partitions* — ident
- luz_finance — *is_a* — Bitbucket slug
- Bitbucket slug — *like* — luz_finance
- luz_finance — *becomes_ws_and_repo_as* — ws="luz_finance", repo=""
- ws="luz_finance", repo="" — *causes* — Bitbucket API 404
- Bitbucket API 404 — *is_a_type_of* — silent grounding failure
- silent grounding failure — *returns* — 0 nodes, 1 gap: <slug>
- Correct slug — *for* — LUZ / dunning / billing codebase
- Correct slug — *is* — axonivy-prod/luz_finance
- axonivy-prod/luz_finance — *has_workspace* — axonivy-prod
- axonivy-prod/luz_finance — *has_mainbranch* — master
- luz_docs_import — *lives_under* — axonivy-prod
- 0 nodes, 1 gap: <repo> — *means* — SLUG is wrong
- 0 nodes, 1 gap: <repo> — *means* — graph could not be built
- v2 bucket — *is* — gs://klara-nonprod-kga-v2-memory/memory/graphify/
- gs://klara-nonprod-kga-v2-memory/memory/graphify/ — *is* — empty
- v2 bucket — *lacks* — graphify cache
- v1 bucket — *is* — mt-receive-ai-agent-memory
- mt-receive-ai-agent-memory — *is* — gone
- v2 — *must_build_graph_from* — Bitbucket
- luz_finance — *is_a* — big repo
- big repo — *can_exceed* — MCP foreground window
- CHANGE-2770 — *is_related_to* — Bitbucket app-password discovery endpoints
- Bitbucket app-password discovery endpoints — *are* — deprecated

%% ai-graph-end %%