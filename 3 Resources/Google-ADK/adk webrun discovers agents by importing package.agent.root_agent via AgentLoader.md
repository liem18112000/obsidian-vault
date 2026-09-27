---
ai_hash: 217cb22bf14b5635
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-08
entities: []
source: v2 E1, google-adk 2.8.0, 2026-09-08
status: seedling
tags:
- google-adk
- adk-cli
- discovery
- structure
title: adk web/run discovers agents by importing package.agent.root_agent via AgentLoader
type: howto
---

# adk web/run discovers agents by importing package.agent.root_agent via AgentLoader

`adk web <dir>` / `adk run <name>` discover an agent by importing the package `<dir>/<name>` and reading a module-level variable named exactly **`root_agent`** — canonically from that package's **`agent.py`**. The loader is `google.adk.cli.utils.agent_loader.AgentLoader(agents_dir=<dir>)`; `loader.load_agent('<name>')` returns the root agent. You can verify discovery non-interactively (without launching the web server) with:

```python
from google.adk.cli.utils.agent_loader import AgentLoader
AgentLoader(agents_dir='src').load_agent('knowledge_gathering')  # -> the root_agent object
```

**Migration technique** when an existing `agent.py` is already taken by legacy code (e.g. it holds an A2A AgentCard, not an ADK `root_agent`): move the legacy content to a differently-named module (e.g. `a2a_card.py`), redirect its importers, and promote the ADK root into `agent.py` — that frees the canonical filename so `adk web`/`adk run` can find `root_agent` while the legacy shell keeps working. The `__init__.py` should `load_dotenv()`, set `GOOGLE_GENAI_USE_VERTEXAI`, best-effort `google.auth.default()` for the project, THEN `from . import agent` (ordering: env before agent construction). Related: [[ADK sample canonical layout: root_agent in agent.py, sub_agents subpackages, workflow agents]], [[ADK agent name must be a valid Python identifier]].

## Related

- [[ADK sample canonical layout: root_agent in agent.py]]
- [[sub_agents subpackages]]
- [[workflow agents]]

%% ai-graph-start %%

**Related notes:**
- [[ADK sample canonical layout root_agent in agent.py, sub_agents subpackages, workflow agents]]
- [[How ADK agents are deployed adk deploy cloud_run agent_engine gke + get_fast_api_app]]
- [[ADK to_a2a returns a Starlette app and accepts a custom Runner and AgentCard]]
- [[ADK to_a2a auto-card is generic; pass agent_card= to keep a rich AgentCard]]
- [[ADK agent name must be a valid Python identifier]]

%% ai-graph-end %%