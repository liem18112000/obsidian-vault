---
ai_hash: 4af955691d834763
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-08
entities: []
source: adk-samples deep-read 2026-09-08
status: seedling
tags:
- google-adk
- agents
- orchestration
- architecture
title: 'ADK canonical orchestration: SequentialAgent, LlmAgent+AgentTool, or callbacks
  — not custom BaseAgent'
type: concept
---

# ADK canonical orchestration: SequentialAgent, LlmAgent+AgentTool, or callbacks — not custom BaseAgent

The official ADK samples (google/adk-samples contrib/python: llm-auditor, financial-advisor, market-research; python/agents/customer-service) use exactly **three orchestration shapes and ZERO custom BaseAgent subclasses**:

1. **Fixed pipeline → `SequentialAgent(sub_agents=[a, b])`** (llm-auditor: critic→reviser). Deterministic order, no LLM router.
2. **Dynamic multi-specialist → one `LlmAgent` with `tools=[AgentTool(agent=child), …]`** (financial-advisor, market-research). The coordinator LLM *decides* which specialists to call; inter-agent state flows via each child's `output_key` and the coordinator prompt references those state keys by name. Even "parallel" is done by PROMPTING the coordinator to emit all tool calls in one response — not `ParallelAgent`.
3. **Single `LlmAgent` + many plain function tools** (customer-service), with all custom logic in **callbacks**.

Corollaries (the canonical style):
- **Tools are plain typed functions** passed bare in `tools=[...]` (ADK auto-builds the JSON schema from type hints + Google-style docstrings); return dict/JSON; they do NOT take ToolContext. No `FunctionTool(...)` wrapper appears.
- **Custom per-step logic lives in callbacks**, not agent subclasses: `before_agent` (seed state), `before_model` (rate-limit / patch empty LlmRequest parts), `before_tool` (**return a dict to short-circuit the tool** = a deterministic guardrail), `after_tool`/`after_model` (post-process; e.g. append grounding citations).
- **`output_schema`, `planner`, `generate_content_config`, `ParallelAgent`, `LoopAgent`** are absent from these reference samples — opt-in, not baseline.

When to still use a **custom BaseAgent**: ADK reserves it for genuinely bespoke control flow the three shapes can't express — e.g. a deterministic text-command router, wrapping a large imperative engine, or HITL pause/resume across turns. Don't reach for it when a SequentialAgent or AgentTool coordinator would do. Related: [[ADK sample canonical layout root_agent in agent.py, sub_agents subpackages, workflow agents|ADK sample canonical layout: root_agent in agent.py, sub_agents subpackages, workflow agents]], [[ADK workflow agents orchestrate deterministically without an LLM-driven loop]], [[Persist ADK session state from a custom agent via Event state_delta]].

## Related

- [[ADK sample canonical layout: root_agent in agent.py]]
- [[sub_agents subpackages]]
- [[workflow agents]]
- [[ADK workflow agents orchestrate deterministically without an LLM-driven loop]]

%% ai-graph-start %%

**Related notes:**
- [[ADK sample canonical layout root_agent in agent.py, sub_agents subpackages, workflow agents]]
- [[ADK workflow agents orchestrate deterministically without an LLM-driven loop]]
- [[ADK modeltool callbacks only fire for LlmAgent-mediated calls; use a Runner Plugin for cross-cutting]]
- [[ADK LlmAgent with output_schema cannot use tools or transfer to other agents]]
- [[test-agent-v2 KGA has no live LlmAgent — explore steps are the first ADK LlmAgent target]]

%% ai-graph-end %%