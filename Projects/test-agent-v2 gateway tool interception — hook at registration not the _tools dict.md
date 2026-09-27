---
ai_hash: 89f258e657824532
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-16
entities: []
tags:
- test-agent-v2
- mcp
- gateway
- gotcha
---

# You can't intercept a gateway MCP tool by wrapping the returned dict

Each domain bridge in test-agent-v2 does `register_tools(mcp, session) -> {name: fn}`, and the
gateway merges those dicts: `_tools = { **register_tpd(...), ... }`.

**Gotcha:** that returned `{name: fn}` map is a *convenience* (used for `globals().update(_tools)` /
agent_cards). The MCP server dispatches the **original `@mcp.tool()`-decorated fn** captured at
registration time — NOT the dict value. So wrapping `_tools["implement_plan"]` after the merge does
**nothing** to the actual tool-call path.

**To intercept a tool, hook at registration**, not after. For the run-benchmark feature I added an
optional `on_finish` callback param to the TPD bridge's `register_tools`; the gateway (which owns
`tev_session`) passes a callback, and `implement_plan` calls it in a `finally`. Dependency points
gateway→bridge, so the TPD agent never imports TEV.

**Second gotcha:** `implement_plan` is multi-turn — it returns `[state: in_progress]` per chunk
until `[state: done]`. An on-finish hook must fire only on a **terminal** reply (done, or an
exception where the reply is None), else it runs on every paused chunk. Gate: `reply is None or
"[state: in_progress]" not in reply`.

Flag `BENCHMARK_ON_FINISH` (default-off in code via `common.learn.config._on`, set `=1` on the
gateway service in `services.tf` — the project's standard flag pattern).

Related: [[test-agent-v2 run benchmark — TEV ownership forced by layering]]

%% ai-graph-start %%

**Related notes:**
- [[test-agent-v2 run benchmark — TEV ownership forced by layering]]
- [[test-agent-v2 deploy + get_deliverables E2E verification]]
- [[Deployed TPD implement_plan trips Cloud Run liveness (event-loop blocked by Vertex gen)]]
- [[approve_plan is an agent-side write, unlike knowledge_gathering's read-only approve]]
- [[When agent-ifying LLM calls, preserve the per-call latency budget by gating non-essential generators behind a flag]]

%% ai-graph-end %%