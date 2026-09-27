---
ai_hash: c2211cde4410e230
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-28
entities: []
source: session 2026-08-27
status: seedling
tags:
- mcp
- http
- cloud-run
- gotcha
- security
title: MCP Streamable-HTTP 421 Invalid Host header behind a proxy — set TransportSecuritySettings
type: lesson
---

# MCP Streamable-HTTP 421 Invalid Host header behind a proxy — set TransportSecuritySettings

The MCP Python SDK protects Streamable-HTTP servers against DNS-rebinding by validating the inbound `Host` (and `Origin`) header — `TransportSecuritySettings(enable_dns_rebinding_protection=True)` is the DEFAULT. Behind a reverse proxy / Cloud Run the inbound Host is the public domain (e.g. `*.run.app`), which is not in the default allowlist, so every request is rejected with **HTTP 421 "Invalid Host header"** — even though auth passed.

Gotcha: `MCPServer.run(transport="streamable-http", host=...)` wires the allowlist for you, but if you build the app yourself with `mcp.streamable_http_app()` (e.g. to add your own ASGI middleware) you inherit the strict default and must pass `transport_security` explicitly:

```python
from mcp.server.transport_security import TransportSecuritySettings
app = mcp.streamable_http_app(transport_security=TransportSecuritySettings(
    enable_dns_rebinding_protection=False, allowed_hosts=[], allowed_origins=[]))
# or keep protection on with allowed_hosts=["my-svc.run.app"]
```
Disabling it is fine when the server is on a public domain gated by a bearer, not a browser-reachable localhost. Diagnostic: 403 = Cloud Run IAM denied; 401 = your bearer middleware; 421 = the Host guard; 400 = reached the MCP app.

See [[MCP stdio transport cannot be hosted remotely; use Streamable HTTP]].

## Related

- [[MCP stdio transport cannot be hosted remotely; use Streamable HTTP]]

%% ai-graph-start %%

**Related notes:**
- [[Rolling a bearer-gated MCP server revision can stick Claude Code in OAuth DCR — re-register to reset]]
- [[MCP stdio transport cannot be hosted remotely; use Streamable HTTP]]
- [[Reach a private Cloud Run service as a user via gcloud run services proxy, not a minted ID token]]
- [[Cloud Run service-to-service with an app bearer needs the callee public (Authorization header collision)]]
- [[Cloud Run one-port limit forces co-located HTTP servers into separate services]]

%% ai-graph-end %%