---
title: "test-agent-v2 KGA has no live LlmAgent — explore steps are the first ADK LlmAgent target"
created: 2026-09-08
type: observation
status: seedling
source: "session 2026-09-08"
tags: [google-adk, test-agent-v2, knowledge-gathering, llmagent, refactor]
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
