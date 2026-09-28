---
title: "Rotating a service bearer silently invalidates every client config holding the old one"
created: 2026-09-28
type: lesson
status: seedling
source: "session 2026-09-28 — testing-agent MCP 503 then 401"
tags: [secrets, rotation, mcp, cloud-run, debugging, gotcha]
---

# Rotating a service bearer silently invalidates every client config holding the old one

Rotating the bearer a service checks does nothing to the copies of the old token already pasted into client configs. Those clients keep sending a credential that stopped being valid, and the server answers `401` — which reads as "my auth is broken" rather than "my token is simply old". The server side of a rotation is one command; the client side is every place the token was ever copied to, and nothing reminds you of them.

For a Claude Code MCP server the symptom is `claude mcp get <name>` reporting a connection failure with `HTTP 401`, or an app-level `{"error":"unauthorized"}` body on a manual curl. The `{...}` JSON body is the important detail — it proves the request reached the application and was rejected by its own bearer middleware, as opposed to the Google-frontend HTML page you get from platform-level rejection (see [[Cloud Run 401 response body distinguishes GFEIAM rejection from app-level auth]]).

**Dating the rotation, to confirm the token is the problem:**

- Secret Manager version states. `gcloud secrets versions list <secret>` shows the live version `enabled` and the superseded ones `disabled`; the newest `enabled` version's `createTime` is when rotation happened. (A rotation script that disables old versions is the tidy case — the sloppy default is [[Secret Manager versions accumulate silently - one per deploy, all left enabled|leaving them all enabled]].)
- A rotation stamp in the service env. Wiring a `BEARER_ROTATED_AT` epoch env var into the deployment makes the rotation visible in `gcloud run services describe` without touching the secret at all — a cheap, non-sensitive breadcrumb worth adding to any service that rotates credentials.

**Fixing it:** re-run the project's own install/register script rather than hand-editing the client config. It already knows where the current token lives (typically a local `.env`) and re-registers idempotently, so you never handle the secret yourself and cannot paste a stale one back in.

**The stacking trap:** a rotation performed in the same maintenance window as another change hides behind it. Here a service was scaled to zero at 08:00 and its bearer rotated at 08:25; the 503 from the [[A disabled Cloud Run service 503s at the edge and never reaches your app|disabled service]] masked the 401 completely, because a request the edge rejects never gets far enough to have its credential checked. Fixing the first cause revealed a second, independent one. When one maintenance window contains several changes, expect to peel them off one at a time rather than assuming the first fix is the fix.

## Related

- [[A disabled Cloud Run service 503s at the edge and never reaches your app]]
- [[Cloud Run 401 response body distinguishes GFEIAM rejection from app-level auth]]
- [[Secret Manager versions accumulate silently - one per deploy]]
- [[all left enabled]]
