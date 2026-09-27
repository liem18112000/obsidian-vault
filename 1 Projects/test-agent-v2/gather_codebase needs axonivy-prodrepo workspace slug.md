---
title: "gather_codebase needs axonivy-prod/<repo> workspace slug"
created: 2026-09-13
type: lesson
status: seedling
source: "session 2026-09-13 LUZ-156281 demo"
tags: [codegraph, bitbucket, test-agent-v2, gotcha, gather]
---

# gather_codebase needs axonivy-prod/<repo> workspace slug

The Testing-Agent (test-agent-v2) `gather_codebase(repo=...)` MCP tool expects a **workspace-qualified** Bitbucket slug `<workspace>/<repo>`. Internally `CodeGraphFetcher.fetch` does `ws, _, repo = ident.partition("/")`, so a bare slug like `luz_finance` becomes `ws="luz_finance", repo=""` → Bitbucket API 404 → the call returns `0 nodes, 1 gap: <slug>` (a silent grounding failure, not an error).

**Correct slug for the LUZ / dunning / billing codebase:** `axonivy-prod/luz_finance` (workspace is `axonivy-prod`; mainbranch `master`). Other repos live under the same workspace, e.g. `axonivy-prod/luz_docs_import`.

**Gotcha:** a `0 nodes, 1 gap: <repo>` result means the SLUG is wrong (or the graph could not be built), NOT that the repo is empty. Always pass `axonivy-prod/<repo>`.

Related: no graphify cache exists in the v2 bucket (`gs://klara-nonprod-kga-v2-memory/memory/graphify/` is empty) and the old v1 bucket `mt-receive-ai-agent-memory` is gone (404), so v2 must build the graph live from Bitbucket each time — a big repo like luz_finance can exceed the 120s MCP foreground window and continue in the background.

## Related

- [[Bitbucket app-password discovery endpoints deprecated (CHANGE-2770)]]
