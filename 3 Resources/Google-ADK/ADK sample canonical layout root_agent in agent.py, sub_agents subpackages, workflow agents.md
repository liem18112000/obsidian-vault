---
title: "ADK sample canonical layout: root_agent in agent.py, sub_agents subpackages, workflow agents"
created: 2026-09-08
type: reference
status: seedling
source: "adk-samples contrib/python/llm-auditor, read 2026-09-08"
tags: [google-adk, samples, structure, agents]
---

# ADK sample canonical layout: root_agent in agent.py, sub_agents subpackages, workflow agents

The official ADK samples (github.com/google/adk-samples) follow a consistent package layout that the `adk web` / `adk run` / `adk eval` CLIs rely on:

- Each agent is a Python package `<agent_name>/` containing **`agent.py`** which builds the agent and assigns a module-level **`root_agent`** variable. The CLI discovers the agent by importing `root_agent` — this variable name is a hard convention.
- Sub-agents live in **`sub_agents/<name>/agent.py`**, each exporting its own agent object (e.g. `critic_agent`, `reviser_agent`), imported into the root `agent.py`.
- Top-level of the sample: `deployment/` (Agent Engine / to_a2a deploy script), `eval/` (`*.evalset.json` + `test_config.json`), `tests/`, `.env.example`, `pyproject.toml`, `uv.lock`, README.

Canonical multi-agent example (contrib/python/llm-auditor/llm_auditor/agent.py) — a deterministic pipeline is a **SequentialAgent** wrapping sub-agents:

```python
from google.adk.agents import SequentialAgent
from .sub_agents.critic import critic_agent
from .sub_agents.reviser import reviser_agent

llm_auditor = SequentialAgent(
    name="llm_auditor",
    description="Evaluates LLM answers, verifies via web, refines the response.",
    sub_agents=[critic_agent, reviser_agent],
)
root_agent = llm_auditor
```

Takeaways: (1) deterministic order = a workflow agent (SequentialAgent/LoopAgent/ParallelAgent), NOT a custom BaseAgent — samples reserve custom BaseAgent for genuinely bespoke control flow; (2) `description=` is load-bearing — an LlmAgent parent uses each sub-agent's description to decide LLM-driven delegation; (3) exporting `root_agent` from `agent.py` is what makes an agent `adk web`-compatible. Related: [[ADK workflow agents orchestrate deterministically without an LLM-driven loop]], [[ADK to_a2a returns a Starlette app and accepts a custom Runner and AgentCard]].

## Related

- [[ADK workflow agents orchestrate deterministically without an LLM-driven loop]]
