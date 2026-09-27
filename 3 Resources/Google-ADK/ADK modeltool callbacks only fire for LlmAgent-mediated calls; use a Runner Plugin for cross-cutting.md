---
ai_hash: dee1c04ed74431bf
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-08
entities: []
source: v2 E4, google-adk 2.8.0, 2026-09-08
status: seedling
tags:
- google-adk
- callbacks
- plugins
- gotcha
title: ADK model/tool callbacks only fire for LlmAgent-mediated calls; use a Runner
  Plugin for cross-cutting
type: lesson
---

# ADK model/tool callbacks only fire for LlmAgent-mediated calls; use a Runner Plugin for cross-cutting

ADK's per-agent callbacks — `before_model_callback`/`after_model_callback` (fire around an LlmAgent's model call) and `before_tool_callback`/`after_tool_callback` (fire around a FunctionTool/AgentTool call) — only have an attach point when the work goes THROUGH ADK's LlmAgent/tool layer.

If an agent is a custom `BaseAgent` that calls its LLM directly (e.g. via a vendor SDK / a reused engine) and does its own I/O rather than via ADK `tools=[...]`, then those model/tool callbacks NEVER fire for it — there is no ADK-mediated model call or tool call to wrap. This is easy to get wrong when porting a deterministic engine (LLM-as-a-leaf-inside-the-engine) onto ADK: reaching for `before_model_callback` to add a guardrail/rate-limit/recall-injection does nothing.

Where cross-cutting logic DOES fire regardless of agent type: a **Runner Plugin** (`BasePlugin.before_run_callback`/`after_run_callback`) — registered once on the Runner, it runs per invocation for every agent, LlmAgent or custom BaseAgent alike. So: head-of-request drains, projections, global setup → Plugin `before_run`. Model-request rewriting / tool guardrails → per-agent `before_model`/`before_tool`, but ONLY once the step is an actual LlmAgent/tool. Rule of thumb: match the hook to whether the work is ADK-mediated. Related: [[ADK canonical orchestration SequentialAgent, LlmAgent+AgentTool, or callbacks — not custom BaseAgent|ADK canonical orchestration: SequentialAgent, LlmAgent+AgentTool, or callbacks — not custom BaseAgent]], [[Persist ADK session state from a custom agent via Event state_delta]].

## Related

- [[ADK canonical orchestration: SequentialAgent]]
- [[LlmAgent+AgentTool]]
- [[or callbacks — not custom BaseAgent]]

%% ai-graph-start %%

**Related notes:**
- [[ADK canonical orchestration SequentialAgent, LlmAgent+AgentTool, or callbacks — not custom BaseAgent]]
- [[ADK LoggingPlugin gives free invocation-lifecycle tracing via a Runner BasePlugin]]
- [[ADK workflow agents orchestrate deterministically without an LLM-driven loop]]
- [[ADK LongRunningFunctionTool HITL nested in SequentialAgent has resume bugs]]
- [[ADK built-in logging does not cover env-gated per-agent app logging]]

%% ai-graph-end %%