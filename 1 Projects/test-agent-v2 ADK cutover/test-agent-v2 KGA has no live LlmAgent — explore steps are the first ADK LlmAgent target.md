---
ai_hash: 6fef870c3b9b6a5e
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-08
entities:
- test-agent-v2 KGA
- LlmAgent
- ADK
- explore steps
- knowledge_gathering agent
- BaseAgent
- Vertex
- ADK agent-reasoning primitives
- output_schema
- tools
- AgentTool
- agent_model()
- claude_llm()
- LlmAgent-model façade
- common/adk/model.py
- src
- explore/hypothesize.py
- explore/ask_llm.py
- common.llm.vertex.complete()
- JSON prompt
- _coerce_* parser
- asyncio.to_thread
- explore/expand.py
- ModelProvider
- ADK ctx
- run_async(ctx)
- HITL resume-state
- ADK session.state
- DatabaseSessionService
- services.py
- GCS
- bank.read_refine_state
- memory tools
- common/adk/tools.py
- FunctionTool
- KgaRouter._read_tool
- GatherAgent
- explore loop
- gather_agent.py
- test_explore_loop_flag_is_inert_in_adk_gather
- loop/crawl.py
- loop/fetch/*
- max_nodes
- max_seconds
- ADK built-in logging
- env-gated per-agent app logging
- Google ADK
- a2a-sdk-direct
source: session 2026-09-08
status: seedling
tags:
- google-adk
- test-agent-v2
- knowledge-gathering
- llmagent
- refactor
title: test-agent-v2 KGA has no live LlmAgent — explore steps are the first ADK LlmAgent
  target
type: observation
---

# test-agent-v2 KGA has no live LlmAgent — explore steps are the first ADK LlmAgent target

In test-agent-v2, the `knowledge_gathering` (KGA) agent is 100%% custom ADK `BaseAgent` + raw Vertex — it reuses **none** of ADK's agent-reasoning primitives (`LlmAgent`, `output_schema`, tools, `AgentTool`). The `agent_model()`/`claude_llm()` LlmAgent-model façade in `common/adk/model.py` has **zero callers** in `src` (the cutover doc even marks it "the branch to delete… no live LlmAgent consumes them yet").

The only LLM work in KGA is two explore steps — `explore/hypothesize.py` and `explore/ask_llm.py` — and both call raw `common.llm.vertex.complete()`, hand-writing a JSON prompt + fence-stripping `_coerce_*` parser, offloaded via `asyncio.to_thread` from `explore/expand.py`.

Ranked ADK agent-aspect reuse opportunities:
1. **(best, low risk)** Convert `hypothesize`/`ask_llm` to `LlmAgent(model=agent_model(...), output_schema=<pydantic>)` — deletes bespoke JSON coercion, gives `ModelProvider` its first real consumer, drops the thread offload (LlmAgent is async), and must be invoked through the ADK ctx (child `run_async(ctx)` or `AgentTool`), not a bare call.
2. Fold HITL resume-state onto ADK `session.state` (DatabaseSessionService already wired in `services.py`) instead of the parallel GCS `bank.read_refine_state` channel.
3. (conditional) Expose the read-only memory tools (already ADK-shaped functions in `common/adk/tools.py`) as `FunctionTool`s on an LlmAgent, instead of `KgaRouter._read_tool` string dispatch — only if NL memory access is wanted (adds an LLM to a deterministic path).
4. Port the explore **loop** into `GatherAgent` — the documented A1 gap (gather_agent.py warns the loop is inert under ADK; canary test `test_explore_loop_flag_is_inert_in_adk_gather`). This is loop-control, not an LlmAgent conversion.

Explicit non-candidates: `loop/crawl.py` (BFS frontier) and `loop/fetch/*` are deterministic bounded I/O — keep them deterministic; agent-ifying forfeits max_nodes/max_seconds guarantees. "Reuse ADK more" != make everything an LlmAgent.

Related: [[ADK built-in logging does not cover env-gated per-agent app logging]].

## Related

- [[ADK built-in logging does not cover env-gated per-agent app logging]]
- [[Adopt Google ADK only when the LLM drives the tool loop; else stay a2a-sdk-direct]]

%% ai-graph-start %%

**Related notes:**
- [[Adopt Google ADK only when the LLM drives the tool loop; else stay a2a-sdk-direct]]
- [[test-agent-v2 TPD has five raw-Vertex generators — the ADK LlmAgent conversion targets]]
- [[A deterministic scorer is a negative case for LLM-agent-ification — reuse ADK via custom EvalMetric, not LlmAgent]]
- [[ADK LlmAgent with output_schema cannot use tools or transfer to other agents]]
- [[Drive the KGA A2A agent offline via Starlette TestClient for evaluation]]

**Relations:**
- test-agent-v2 KGA — *has no live* — LlmAgent
- explore steps — *are* — first ADK LlmAgent target
- knowledge_gathering agent — *is* — custom ADK BaseAgent
- knowledge_gathering agent — *uses* — raw Vertex
- knowledge_gathering agent — *reuses none of* — ADK agent-reasoning primitives
- ADK agent-reasoning primitives — *include* — LlmAgent
- ADK agent-reasoning primitives — *include* — output_schema
- ADK agent-reasoning primitives — *include* — tools
- ADK agent-reasoning primitives — *include* — AgentTool
- LlmAgent-model façade — *is* — agent_model()
- LlmAgent-model façade — *is* — claude_llm()
- LlmAgent-model façade — *is defined in* — common/adk/model.py
- LlmAgent-model façade — *has zero callers in* — src
- explore/hypothesize.py — *is a* — LLM work in KGA
- explore/ask_llm.py — *is a* — LLM work in KGA
- explore/hypothesize.py — *calls* — common.llm.vertex.complete()
- explore/ask_llm.py — *calls* — common.llm.vertex.complete()
- common.llm.vertex.complete() — *uses* — JSON prompt
- common.llm.vertex.complete() — *uses* — _coerce_* parser
- JSON prompt — *is offloaded via* — asyncio.to_thread
- _coerce_* parser — *is offloaded via* — asyncio.to_thread
- asyncio.to_thread — *is from* — explore/expand.py
- explore/hypothesize.py — *can be converted to* — LlmAgent
- explore/ask_llm.py — *can be converted to* — LlmAgent
- LlmAgent — *uses* — agent_model()
- LlmAgent — *uses* — output_schema
- Conversion to LlmAgent — *deletes* — bespoke JSON coercion
- Conversion to LlmAgent — *gives* — ModelProvider
- Conversion to LlmAgent — *drops* — thread offload
- LlmAgent — *is* — async
- LlmAgent — *must be invoked through* — ADK ctx
- ADK ctx — *can use* — run_async(ctx)
- ADK ctx — *can use* — AgentTool
- HITL resume-state — *can be folded onto* — ADK session.state
- DatabaseSessionService — *is wired in* — services.py
- GCS — *is a parallel channel for* — bank.read_refine_state
- memory tools — *are* — ADK-shaped functions
- memory tools — *are in* — common/adk/tools.py
- memory tools — *can be exposed as* — FunctionTool
- FunctionTool — *on* — LlmAgent
- KgaRouter._read_tool — *is* — string dispatch
- explore loop — *can be ported into* — GatherAgent
- GatherAgent — *is described in* — gather_agent.py
- explore loop — *is inert under* — ADK
- test_explore_loop_flag_is_inert_in_adk_gather — *is a* — canary test
- loop/crawl.py — *is* — deterministic bounded I/O
- loop/fetch/* — *is* — deterministic bounded I/O
- agent-ifying loop/crawl.py — *forfeits* — max_nodes
- agent-ifying loop/fetch/* — *forfeits* — max_seconds
- test-agent-v2 KGA — *is related to* — ADK built-in logging
- test-agent-v2 KGA — *is related to* — env-gated per-agent app logging
- ADK built-in logging — *does not cover* — env-gated per-agent app logging
- test-agent-v2 KGA — *is related to* — Adopt Google ADK only when the LLM drives the tool loop; else stay a2a-sdk-direct

%% ai-graph-end %%