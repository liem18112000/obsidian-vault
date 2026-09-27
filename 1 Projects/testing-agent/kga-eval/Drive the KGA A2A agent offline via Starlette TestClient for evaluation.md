---
ai_hash: a22d4f84897c0bfb
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-04
entities:
- KGA A2A agent
- Starlette TestClient
- evaluation
- Knowledge-Gathering Agent (KGA)
- network
- Cloud Run
- LLM
- executor
- A2A JSON-RPC stack
- KnowledgeGatheringExecutor
- MemoryBank
- FakeBucket
- recorded client
- DefaultRequestHandler
- create_jsonrpc_routes
- Starlette app
- message/send payload
- crawl
- G0–G5 fan-out
- tests/test_executor_a2a.py
- run_gather
- EventQueue
- A2A TaskUpdater messages
- JSON-RPC response
- FakeRequestContext
- CapturingEventQueue
- run_gather_offline
- RunTrace
- repo
- TestClient pattern
- RunLog
- bank
- CrawlResult.run
- knowledge_gathering.loop.crawl
- Tier trace
- md_blocks
- Refine
- common.interrogate.loop.refine
- RefineResult
- understanding
- Atlassian client
- get_issue
- get_issue_remote_links
- get_issue_dev_status
- search_jql
- search_cql
- get_page
- _dev_status
- dev links
- harness
- prod GCS_BUCKET
- index
- tmp bank
- Testing Agent KGA evaluation harness (ADK + RAGAS)
source: session 2026-09-04
status: seedling
tags:
- testing-agent
- kga
- evaluation
- a2a
- pytest
- gotcha
title: Drive the KGA A2A agent offline via Starlette TestClient for evaluation
type: howto
---

# Drive the KGA A2A agent offline via Starlette TestClient for evaluation

To evaluate the Knowledge-Gathering Agent (KGA) with no network, no Cloud Run, and no LLM, drive the real executor in-process through the A2A JSON-RPC stack: wrap `KnowledgeGatheringExecutor(bank=MemoryBank(FakeBucket()), client=<recorded>)` in a `DefaultRequestHandler` + `create_jsonrpc_routes` Starlette app and hit it with Starlette `TestClient` via a `message/send` payload. This runs the genuine crawl + G0–G5 fan-out but touches nothing external. Pattern lifted from `tests/test_executor_a2a.py`.

**Why this over calling `run_gather` directly:** `run_gather(ex, ctx, q, text)` returns `None` — its output is emitted through `await reply(...)` into the EventQueue as A2A `TaskUpdater` messages. Extracting the reply text from raw events is fiddly; the TestClient path returns a clean JSON-RPC response you recursively scrape for every `text` value. Use `ensure_ascii=False` / raw text (not `json.dumps`) so em-dash tier markers (e.g. "External-LLM leads —") still substring-match.

**Gotchas dug out:**
- The doc-proposed fakes `FakeRequestContext` / `CapturingEventQueue` / `run_gather_offline` / `RunTrace` do NOT exist in the repo — they were only a design spec. Build on the TestClient pattern instead.
- `RunLog` (sources/nodes_fetched/…) is **write-only markdown** in the bank — there is no `read_run_log`. To recover the pack you read `bank.load_index()[0].nodes.keys()`; to get a structured `CrawlResult.run` you must call `knowledge_gathering.loop.crawl(...)` directly (run_gather discards it).
- **Tier trace** is not a RunLog field — it is string-matched from the gather reply’s md_blocks (order G2→B1→G0→G1→G4): "Hypothesized focus:", "climbed to structural parent", "Prior knowledge from memory", "Atlassian search (seed was thin) surfaced", "External-LLM leads".
- **Refine** cannot be driven in one shot through the executor (it pauses per round in `input-required`). Use the synchronous driver `common.interrogate.loop.refine(bank, ctx, seed=..., answer_fn=accept_recommendation) -> RefineResult` to run it end-to-end offline; `.understanding` is the brief.
- A recorded Atlassian client only needs the high-level async methods (`get_issue`, `get_issue_remote_links`, optional `get_issue_dev_status`/`search_jql`/`search_cql`/`get_page`); `_dev_status` uses `getattr(client, ...)` so omitting dev-status just yields no dev links (no crash).
- Never point the harness at the prod `GCS_BUCKET` — a gather WRITES the index. Use `MemoryBank(FakeBucket())` or a tmp bank.

Related: [[Testing Agent KGA evaluation harness (ADK + RAGAS)]]

## Related

- [[Testing Agent KGA evaluation harness (ADK + RAGAS)]]

%% ai-graph-start %%

**Related notes:**
- [[test-agent-v2 KGA has no live LlmAgent — explore steps are the first ADK LlmAgent target]]
- [[Test an ADK LlmAgent(output_schema=) offline with a BaseLlm fake yielding canned JSON]]
- [[Run test-agent-v2 locally with docker-compose (no GCP)]]
- [[Testing Agent builds each pipeline stage as a package mirroring the knowledge_gathering skeleton]]
- [[KGA crawler fetches repo source files via client mixin plus NodeFetcher registered by kind]]

**Relations:**
- KGA A2A agent — *driven via* — Starlette TestClient
- Starlette TestClient — *used for* — evaluation
- evaluation — *of* — Knowledge-Gathering Agent (KGA)
- Knowledge-Gathering Agent (KGA) — *operates without* — network
- Knowledge-Gathering Agent (KGA) — *operates without* — Cloud Run
- Knowledge-Gathering Agent (KGA) — *operates without* — LLM
- executor — *driven through* — A2A JSON-RPC stack
- KnowledgeGatheringExecutor — *uses* — MemoryBank
- MemoryBank — *uses* — FakeBucket
- KnowledgeGatheringExecutor — *uses* — recorded client
- KnowledgeGatheringExecutor — *wrapped in* — DefaultRequestHandler
- DefaultRequestHandler — *part of* — Starlette app
- create_jsonrpc_routes — *part of* — Starlette app
- Starlette TestClient — *hits* — Starlette app
- Starlette TestClient — *sends* — message/send payload
- Starlette TestClient — *runs* — crawl
- Starlette TestClient — *runs* — G0–G5 fan-out
- TestClient pattern — *lifted from* — tests/test_executor_a2a.py
- run_gather — *returns* — None
- run_gather — *emits output to* — EventQueue
- EventQueue — *receives* — A2A TaskUpdater messages
- TestClient pattern — *returns* — JSON-RPC response
- FakeRequestContext — *does not exist in* — repo
- CapturingEventQueue — *does not exist in* — repo
- run_gather_offline — *does not exist in* — repo
- RunTrace — *does not exist in* — repo
- TestClient pattern — *is alternative to* — FakeRequestContext
- RunLog — *stored in* — bank
- RunLog — *is* — write-only markdown
- read_run_log — *does not exist* — repo
- pack — *recovered from* — bank
- CrawlResult.run — *obtained from* — knowledge_gathering.loop.crawl
- run_gather — *discards* — CrawlResult.run
- Tier trace — *not a field of* — RunLog
- Tier trace — *extracted from* — md_blocks
- Refine — *cannot be driven by* — executor
- Refine — *driven by* — common.interrogate.loop.refine
- RefineResult — *has* — understanding
- Atlassian client — *requires* — get_issue
- Atlassian client — *requires* — get_issue_remote_links
- Atlassian client — *requires* — get_issue_dev_status
- Atlassian client — *requires* — search_jql
- Atlassian client — *requires* — search_cql
- Atlassian client — *requires* — get_page
- _dev_status — *uses* — Atlassian client
- harness — *should not use* — prod GCS_BUCKET
- crawl — *writes* — index
- prod GCS_BUCKET — *stores* — index
- harness — *uses* — MemoryBank
- harness — *uses* — tmp bank
- KGA A2A agent evaluation — *related to* — Testing Agent KGA evaluation harness (ADK + RAGAS)

%% ai-graph-end %%