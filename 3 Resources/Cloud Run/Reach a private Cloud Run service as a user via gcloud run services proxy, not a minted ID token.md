---
ai_hash: b06962e30240890e
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
- mcp
- proxy
- gotcha
title: Reach a private Cloud Run service as a user via gcloud run services proxy,
  not a minted ID token
type: lesson
---

# Reach a private Cloud Run service as a user via gcloud run services proxy, not a minted ID token

A human/**user** Google account cannot mint an ID token scoped to a Cloud Run service audience: `gcloud auth print-identity-token --audiences=<url>` only works for **service accounts** (or impersonation), and a users plain ID token has the wrong audience, so a private Cloud Run service rejects it (401/403).

The right way for a user to reach a private Cloud Run service locally is **`gcloud run services proxy <service> --region <r> --port <p>`**: it authenticates with the users ADC, mints/refreshes the correct token automatically, and forwards `http://localhost:<p>` → the service. First run installs a one-time `Cloud Run Proxy` gcloud component (takes ~20s before it binds).

Bonus for MCP: registering an MCP client against the localhost proxy URL keeps **no token in the client config** (the proxy owns auth) and never expires — unlike baking `--header "Authorization: Bearer <id-token>"`, which goes stale in ~1h. Verified: `claude mcp add --transport http knowledge-gathering http://localhost:8080/mcp` → status Connected, with the proxy running.

Reaching the MCP endpoint returns 403 when IAM denies (no/failed token), and 400 once you pass IAM (the app rejects a bare GET) — a handy way to tell an auth failure from an app-level response.

See [[Cloud Run service-to-service with an app bearer needs the callee public (Authorization header collision)]], [[MCP stdio transport cannot be hosted remotely; use Streamable HTTP]].

## Related

- [[Cloud Run service-to-service with an app bearer needs the callee public (Authorization header collision)]]
- [[MCP stdio transport cannot be hosted remotely; use Streamable HTTP]]

%% ai-graph-start %%

**Related notes:**
- [[Cloud Run service-to-service with an app bearer needs the callee public (Authorization header collision)]]
- [[Registering a bearer-gated HTTP MCP server needs claude mcp add --header]]
- [[Rolling a bearer-gated MCP server revision can stick Claude Code in OAuth DCR — re-register to reset]]
- [[Memorystore Redis has no auth-proxy — local access needs an IAP jump VM]]
- [[MCP Streamable-HTTP 421 Invalid Host header behind a proxy — set TransportSecuritySettings]]

%% ai-graph-end %%