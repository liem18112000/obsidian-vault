---
ai_hash: f2fda53fdf835ec1
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-27
entities: []
source: session 2026-08-27
status: seedling
tags:
- cloud-run
- gcp
- architecture
- deployment
title: Cloud Run one-port limit forces co-located HTTP servers into separate services
type: lesson
---

# Cloud Run one-port limit forces co-located HTTP servers into separate services

A Cloud Run **service exposes exactly one public port**. So when two HTTP servers logically sit together — e.g. an A2A agent and the MCP bridge that fronts it — you cannot publish both on the same service. Two clean shapes:

- **Separate services (preferred):** deploy the same image twice with different command + env — agent on its own URL, bridge pointing `KGA_A2A_URL` at that URL. One process per container (the Cloud Run best practice).
- **Single container:** the bridge is the public front on `$PORT`; the agent runs as an internal process on `localhost:8081`. One deployable, but needs a supervisor to run two processes.

Bundling `mcp` into the image (`pip install ".[bridge]"`) lets one image serve either role, chosen by the container command.

See [[MCP stdio transport cannot be hosted remotely; use Streamable HTTP]], [[Multi-turn agent sessions need min-instances=1 and session affinity on Cloud Run]].

## Related

- [[MCP stdio transport cannot be hosted remotely; use Streamable HTTP]]
- [[Multi-turn agent sessions need min-instances=1 and session affinity on Cloud Run]]

%% ai-graph-start %%

**Related notes:**
- [[Co-locating a stateful MCP bridge as an agent sidecar couples their scaling]]
- [[Multi-turn agent sessions need min-instances=1 and session affinity on Cloud Run]]
- [[MCP stdio transport cannot be hosted remotely; use Streamable HTTP]]
- [[Cloud Run service-to-service with an app bearer needs the callee public (Authorization header collision)]]
- [[Cloud Run v2 multi-container sidecar in Terraform]]

%% ai-graph-end %%