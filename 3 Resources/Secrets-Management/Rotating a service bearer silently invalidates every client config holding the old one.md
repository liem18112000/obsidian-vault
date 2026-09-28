---
ai_hash: 8a46aede285353b7
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-28'
created: 2026-09-28
entities:
- Service Bearer
- Client Config
- Old Token
- Client
- Server
- HTTP 401
- Authentication Issue
- Token
- Claude Code MCP Server
- '`claude mcp get <name>` (command)'
- Connection Failure
- App-level Unauthorized Error (`{"error":"unauthorized"}`)
- Application
- Bearer Middleware
- Google-frontend HTML Page
- Platform-level Rejection
- Cloud Run 401 response body distinguishes GFEIAM rejection from app-level auth (Note)
- Secret Manager
- Secret Manager Version
- '`gcloud secrets versions list <secret>` (command)'
- '`enabled` (state)'
- '`disabled` (state)'
- '`createTime` (property)'
- Rotation Script
- Secret Manager versions accumulate silently - one per deploy, all left enabled (Note)
- Rotation Stamp
- Service Environment
- '`BEARER_ROTATED_AT` (environment variable)'
- Deployment
- '`gcloud run services describe` (command)'
- Credential
- Project Install/Register Script
- Current Token
- '`.env` (file)'
- Maintenance Window
- HTTP 503
- Disabled Service
- Edge
- Request
- A disabled Cloud Run service 503s at the edge and never reaches your app (Note)
- GFEIAM Rejection
- App-level Auth
- Secret Manager versions accumulate silently - one per deploy (Note)
- Service Bearer Rotation
- Server Side (of rotation)
- Client Side (of rotation)
- Symptom (of old token)
- First Cause (of issue)
- Second Cause (of issue)
- Live Version
- Superseded Version
- One Command
- Another Change
source: session 2026-09-28 — testing-agent MCP 503 then 401
status: seedling
tags:
- secrets
- rotation
- mcp
- cloud-run
- debugging
- gotcha
title: Rotating a service bearer silently invalidates every client config holding
  the old one
type: lesson
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

%% ai-graph-start %%

**Related notes:**
- [[Rolling a bearer-gated MCP server revision can stick Claude Code in OAuth DCR — re-register to reset]]
- [[Cloud Run resolves a latest secret reference at instance start, not per request]]
- [[A disabled Cloud Run service 503s at the edge and never reaches your app]]
- [[Cloud Run 401 response body distinguishes GFEIAM rejection from app-level auth]]
- [[Registering a bearer-gated HTTP MCP server needs claude mcp add --header]]

**Relations:**
- Service Bearer Rotation — *invalidates* — Client Config
- Client Config — *holds* — Old Token
- Client — *sends* — Old Token
- Old Token — *is not* — Valid
- Server — *responds with* — HTTP 401
- HTTP 401 — *indicates* — Authentication Issue
- HTTP 401 — *indicates* — Old Token
- Service Bearer Rotation — *has* — Server Side (of rotation)
- Service Bearer Rotation — *has* — Client Side (of rotation)
- Server Side (of rotation) — *is* — One Command
- Client Side (of rotation) — *affects* — Client Config
- Claude Code MCP Server — *shows* — Symptom (of old token)
- Symptom (of old token) — *is* — Connection Failure
- `claude mcp get <name>` (command) — *reports* — Connection Failure
- Connection Failure — *is with* — HTTP 401
- Symptom (of old token) — *is* — App-level Unauthorized Error (`{"error":"unauthorized"}`)
- App-level Unauthorized Error (`{"error":"unauthorized"}`) — *occurs on* — Manual Curl
- App-level Unauthorized Error (`{"error":"unauthorized"}`) — *proves* — Request
- Request — *reached* — Application
- Application — *rejected* — Request
- Application — *uses* — Bearer Middleware
- Bearer Middleware — *rejected* — Request
- Platform-level Rejection — *results in* — Google-frontend HTML Page
- App-level Unauthorized Error (`{"error":"unauthorized"}`) — *contrasts with* — Google-frontend HTML Page
- Cloud Run 401 response body distinguishes GFEIAM rejection from app-level auth (Note) — *explains distinction between* — GFEIAM Rejection
- Cloud Run 401 response body distinguishes GFEIAM rejection from app-level auth (Note) — *explains distinction between* — App-level Auth
- Secret Manager — *manages* — Secret Manager Version
- `gcloud secrets versions list <secret>` (command) — *shows* — Secret Manager Version
- Secret Manager Version — *can be* — Live Version
- Secret Manager Version — *can be* — Superseded Version
- Live Version — *is* — `enabled` (state)
- Superseded Version — *is* — `disabled` (state)
- Live Version — *has* — `createTime` (property)
- `createTime` (property) — *indicates* — Service Bearer Rotation
- Rotation Script — *disables* — Superseded Version
- Secret Manager — *has* — Default Behavior
- Default Behavior — *is described in* — Secret Manager versions accumulate silently - one per deploy, all left enabled (Note)
- `BEARER_ROTATED_AT` (environment variable) — *is wired into* — Deployment
- Deployment — *makes visible* — Service Bearer Rotation
- `gcloud run services describe` (command) — *shows* — Service Bearer Rotation
- Project Install/Register Script — *updates* — Client Config
- Project Install/Register Script — *knows location of* — Current Token
- Current Token — *lives in* — `.env` (file)
- Service Bearer Rotation — *occurs in* — Maintenance Window
- Maintenance Window — *contains* — Another Change
- Another Change — *hides* — Service Bearer Rotation
- Disabled Service — *causes* — HTTP 503
- HTTP 503 — *masks* — HTTP 401
- A disabled Cloud Run service 503s at the edge and never reaches your app (Note) — *describes* — Disabled Service
- Edge — *rejects* — Request
- Request — *has* — Credential
- Credential — *is not* — Checked
- First Cause (of issue) — *is* — Disabled Service
- Second Cause (of issue) — *is* — Old Token
- Service Bearer Rotation — *is related to* — A disabled Cloud Run service 503s at the edge and never reaches your app (Note)
- Service Bearer Rotation — *is related to* — Cloud Run 401 response body distinguishes GFEIAM rejection from app-level auth (Note)
- Service Bearer Rotation — *is related to* — Secret Manager versions accumulate silently - one per deploy (Note)
- Service Bearer Rotation — *is related to* — Secret Manager versions accumulate silently - one per deploy, all left enabled (Note)

%% ai-graph-end %%