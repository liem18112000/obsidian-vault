---
ai_hash: 66958e8f7ff2be80
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-08
entities: []
source: adk CLI 2.8.0 + adk.dev/deploy, 2026-09-08
status: seedling
tags:
- google-adk
- deployment
- cloud-run
- agent-engine
title: 'How ADK agents are deployed: adk deploy cloud_run / agent_engine / gke + get_fast_api_app'
type: howto
---

# How ADK agents are deployed: adk deploy cloud_run / agent_engine / gke + get_fast_api_app

Google ADK ships a first-class deploy CLI (confirmed on google-adk 2.8.0 + docs at adk.dev/deploy). `adk deploy` has three targets:

- **`adk deploy cloud_run --project=P --region=R [--service_name=… --with_ui --session_service_uri=… ] <agent_dir>`** — packages the agent, generates a container running ADK's own FastAPI server, and `gcloud run deploy`s it. Serves the ADK **REST/SSE API**: `POST /run`, `POST /run_sse`, `GET /list-apps`, and session/artifact endpoints (`/apps/{app}/users/{user}/sessions/…`); `--with_ui` adds the `adk web` dev UI.
- **`adk deploy agent_engine --project=P --region=R --staging_bucket=gs://B <agent_dir>`** — Vertex AI **Agent Engine**, the managed runtime (equivalently the Python `AdkApp(agent=root_agent) + vertexai.agent_engines.create(requirements=[…], extra_packages=[…])`). Query via the SDK: `remote.create_session()` + `remote.stream_query()`.
- **`adk deploy gke`** — GKE.

The container entrypoint under the hood is **`get_fast_api_app`** (`from google.adk.cli.fast_api import get_fast_api_app`). You can write your own `main.py` for a manual Cloud Run container:
```python
app = get_fast_api_app(agents_dir='src', a2a=True, web=False, session_service_uri=..., artifact_service_uri='gs://bucket')
```
It auto-discovers every agent package under agents_dir (each needs `agent.py:root_agent`), and with **`a2a=True`** ALSO exposes A2A per agent alongside the REST API. Deploy manually with `gcloud run deploy <svc> --source .`.

Gotcha / choice: `get_fast_api_app`/`adk deploy cloud_run` serve ADK's OWN REST API — this is NOT the same as `to_a2a(agent)` which serves ONLY the A2A JSON-RPC protocol at `/`. If a client speaks A2A (e.g. an MCP↔A2A bridge), use `to_a2a` (or `get_fast_api_app(a2a=True)`), not the vanilla `/run` server. Related: [[ADK to_a2a returns a Starlette app and accepts a custom Runner and AgentCard]], [[adk webrun discovers agents by importing package.agent.root_agent via AgentLoader|adk web/run discovers agents by importing package.agent.root_agent via AgentLoader]].

## Related

- [[ADK to_a2a returns a Starlette app and accepts a custom Runner and AgentCard]]
- [[adk webrun discovers agents by importing package.agent.root_agent via AgentLoader|adk web/run discovers agents by importing package.agent.root_agent via AgentLoader]]

%% ai-graph-start %%

**Related notes:**
- [[adk webrun discovers agents by importing package.agent.root_agent via AgentLoader]]
- [[ADK to_a2a returns a Starlette app and accepts a custom Runner and AgentCard]]
- [[ADK deploy targets compute matrix Agent Engine vs Cloud Run vs GKE (CPUGPUTPU)]]
- [[ADK serverless agent tier is CPU-only; LLM compute is offloaded to a managed model API]]
- [[ADK sample canonical layout root_agent in agent.py, sub_agents subpackages, workflow agents]]

%% ai-graph-end %%