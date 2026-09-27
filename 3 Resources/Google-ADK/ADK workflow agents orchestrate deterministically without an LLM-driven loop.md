---
ai_hash: 443d1bea5e070d23
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-07
entities: []
source: adk-transform export 2026-09-07
status: seedling
tags:
- google-adk
- agents
- architecture
title: ADK workflow agents orchestrate deterministically without an LLM-driven loop
type: concept
---

# ADK workflow agents orchestrate deterministically without an LLM-driven loop

Google ADK's workflow agents — SequentialAgent, LoopAgent, ParallelAgent — plus a custom BaseAgent (you implement `_run_async_impl` and yield Events) provide **deterministic orchestration**: the control flow is fixed in code, the LLM does *not* decide the next step. Only an explicit LlmAgent sub-agent invokes a model.

**Why it matters:** the common assumption that "adopting ADK" means handing control to an autonomous LLM loop is false. A hand-written deterministic Python pipeline can be re-hosted on ADK — keeping its exact control flow — by mapping fixed sequences to SequentialAgent, bounded repeat-until-converged to LoopAgent(max_iterations=…) with a sub-agent that `escalate`s on the stop condition, and imperative chunks that must run as-is to a custom BaseAgent that calls the existing function and yields events. LLM calls stay leaves. This is what makes a determinism-preserving migration possible instead of a rewrite into an agentic loop.

Corollary: wrapping existing code in a custom BaseAgent ("re-trigger, don't rewrite") is the encouraged escape hatch for porting a mature engine — you gain ADK's sessions/callbacks/tracing without touching the proven logic.

Related: [[ADK LongRunningFunctionTool HITL nested in SequentialAgent has resume bugs]], [[ADK DatabaseSessionService can subsume a separate A2A task store and state-file rehydration]].

## Related

- [[ADK LongRunningFunctionTool HITL nested in SequentialAgent has resume bugs]]
- [[ADK DatabaseSessionService can subsume a separate A2A task store and state-file rehydration]]

%% ai-graph-start %%

**Related notes:**
- [[ADK canonical orchestration SequentialAgent, LlmAgent+AgentTool, or callbacks — not custom BaseAgent]]
- [[ADK LongRunningFunctionTool HITL nested in SequentialAgent has resume bugs]]
- [[Adopt Google ADK only when the LLM drives the tool loop; else stay a2a-sdk-direct]]
- [[ADK DatabaseSessionService can subsume a separate A2A task store and state-file rehydration]]
- [[ADK sample canonical layout root_agent in agent.py, sub_agents subpackages, workflow agents]]

%% ai-graph-end %%