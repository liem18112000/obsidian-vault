---
ai_hash: aa483354c15ef32f
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-23
entities: []
source: session 2026-09-23 add-github-support
status: seedling
tags:
- test-agent-v2
- kga
- crawler
- architecture
- bitbucket
- github
title: KGA crawler fetches repo source files via client mixin plus NodeFetcher registered
  by kind
type: howto
---

# KGA crawler fetches repo source files via client mixin plus NodeFetcher registered by kind

In test-agent-v2 the Knowledge-Gathering Agent crawler pulls a single source file from a repo host through **two layers**, and adding a new host (e.g. GitHub alongside Bitbucket) means touching both plus a few wiring points.

**Layer 1 — client mixin** (`src/common/atlassian/`): one `BaseClient` owns the httpx client + bounded retry (1/2/4s). Per-host read-only endpoints are mixins composed onto `AtlassianClient` (`JiraMixin`, `ConfluenceMixin`, `BitbucketMixin`, `GitHubMixin`). Each exposes a `get_<host>_src(owner, repo, path, ref) -> str` returning **raw text** (not JSON). `factory.build_client()` reads credentials from env.

**Layer 2 — NodeFetcher** (`src/knowledge_gathering/gather/crawl/fetch/`): a `NodeFetcher` subclass sets `kind = "github"`; `__init_subclass__` auto-registers it in `NodeFetcher.registry`. `fetch_node()` dispatches on the node-id prefix (`kind:ident`). The fetcher splits the ident and calls the Layer-1 client method.

**Wiring points to add a host:**
1. node-type constant in `common/models/graph.py` + export in `models/__init__.py`.
2. `common/extract/classify.py` `classify_url()` → maps a browser URL to `(TYPE, "github:owner/repo/blob/ref/path")`.
3. import the new fetcher module in `fetch/__init__.py` (import = registration).
4. add the kind to `crawl.py` `_fetchable()` (the allow-list of followable kinds).

**Load-bearing gotcha:** `BITBUCKET`/`GITHUB` are **NOT** in the default `Scope.follow_types` (`JIRA_ISSUE, CONFLUENCE_PAGE, ATTACHMENT`). So repo file links are *recorded* (`in_scope=False`) but only *followed and fetched* when a caller passes a Scope that includes the type. Production `agent.py` uses the default Scope, so repo files are inventory-only there unless scope is widened — the fetcher looks dead until you add the type to follow_types.

See [[GitHub fetch credentials PAT Bearer anonymous public Enterprise api-v3 raw Contents API]].

## Related

- [[GitHub fetch credentials PAT Bearer anonymous public Enterprise api-v3 raw Contents API]]

%% ai-graph-start %%

**Related notes:**
- [[Bitbucket Cloud API differs from JiraConfluence host, auth, raw src]]
- [[GitHub fetch credentials PAT Bearer anonymous public Enterprise api-v3 raw Contents API]]
- [[Drive the KGA A2A agent offline via Starlette TestClient for evaluation]]
- [[gather_codebase needs axonivy-prodrepo workspace slug]]
- [[Knowledge-Gathering loop is a bounded frontier crawl with a verify edge]]

%% ai-graph-end %%