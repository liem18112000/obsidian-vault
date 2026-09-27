---
title: "adk web/run discovers agents by importing package.agent.root_agent via AgentLoader"
created: 2026-09-08
type: howto
status: seedling
source: "v2 E1, google-adk 2.8.0, 2026-09-08"
tags: [google-adk, adk-cli, discovery, structure]
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
