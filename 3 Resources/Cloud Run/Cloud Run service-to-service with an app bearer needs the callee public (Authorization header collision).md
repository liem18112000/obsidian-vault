---
ai_hash: 59e7e9c54b223aff
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-27
entities: []
source: session 2026-08-27
status: seedling
tags:
- cloud-run
- gcp
- auth
- iam
- gotcha
title: Cloud Run service-to-service with an app bearer needs the callee public (Authorization
  header collision)
type: lesson
---

# Cloud Run service-to-service with an app bearer needs the callee public (Authorization header collision)

When one Cloud Run service calls another and the *callee application* authenticates requests with its own bearer token in the HTTP `Authorization` header, you hit a collision: Cloud Runs built-in IAM also expects a **Google ID token** in that same `Authorization: Bearer …` header. One header cannot carry both, so a private (IAM-gated) callee cannot simultaneously receive the callers app token.

Practical resolutions:
- Make the **callee public** (`allow_unauthenticated=true`) and let its *app-level* bearer be the only gate — the header then carries only the app token. (Chosen for the A2A agent so the MCP bridge can call it with just the A2A bearer.)
- Or keep the callee private and move the app token to a **different header** the app reads (requires app support) — then `Authorization` is free for the ID token.
- Protect the *caller* (the public front) separately — e.g. keep the bridge private and have the client send a Google ID token to reach it, since that hop has no app token.

See [[Cloud Run one-port limit forces co-located HTTP servers into separate services]], [[A2A-to-MCP bridge is an MCP stdio server that is also an A2A client]].

## Related

- [[Cloud Run one-port limit forces co-located HTTP servers into separate services]]
- [[A2A-to-MCP bridge is an MCP stdio server that is also an A2A client]]

%% ai-graph-start %%

**Related notes:**
- [[Reach a private Cloud Run service as a user via gcloud run services proxy, not a minted ID token]]
- [[Bridgegateway use separate secrets for the inbound caller token and the outbound backend token]]
- [[Cloud Run one-port limit forces co-located HTTP servers into separate services]]
- [[Multi-turn agent sessions need min-instances=1 and session affinity on Cloud Run]]
- [[Cloud Run 401 response body distinguishes GFEIAM rejection from app-level auth]]

%% ai-graph-end %%