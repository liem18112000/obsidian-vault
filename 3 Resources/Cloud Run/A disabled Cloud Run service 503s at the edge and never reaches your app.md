---
ai_hash: 957d8b4cafad84eb
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-28'
created: 2026-09-25
entities:
- Cloud Run
- Cloud Run service
- Disabled Cloud Run service
- 503 Service Unavailable
- Google Frontend HTML page
- run.googleapis.com/scalingMode
- run.googleapis.com/manualInstanceCount
- Revision logs
- gcloud run services describe
- 'Ready: True'
- 'ConfigurationsReady: True'
- 'RoutesReady: True'
- Annotations
- gcloud run services update
- Autoscaling
- MCP server
- claude mcp get <name>
- 'HTTP 503: Streamable HTTP error'
- HTTP status
- 5xx
- curl's 000
- Instance
- Startup probes
- Liveness probes
- Edge
- Application
- Testing consequence
- Probe
- Cloud Run names who changed a service in lastModifier and the UpdateService audit
  log
- A scaled-to-zero service in a shared cloud project is a decision, not a fault
- Cloud Run resolves a latest secret reference at instance start, not per request
- test-agent-v2 Cloud Run services use a -v2 name suffix
- Service-level setting
- Bearer token
- ingress=all
- allUsers
- roles/run.invoker
source: session 2026-09-25 — test-agent-v2 bearer rotation
status: seedling
tags:
- gcp
- cloud-run
- debugging
- gotcha
- health-check
title: A disabled Cloud Run service 503s at the edge and never reaches your app
type: lesson
---

# A disabled Cloud Run service 503s at the edge and never reaches your app

Cloud Run lets you **disable a service** without deleting it. On the wire it looks like this:

```
run.googleapis.com/scalingMode: manual
run.googleapis.com/manualInstanceCount: '0'
```

Every request then gets a Google Frontend HTML page — `503 Service Unavailable` / **"Service is disabled"** — generated before any container is involved. In the revision logs the tell is `Shutting down user disabled instance`, often *after* the startup and liveness probes logged success: the container boots fine and is then killed because the service is off.

What makes it expensive to diagnose:

- `gcloud run services describe` still reports **`Ready: True`**, `ConfigurationsReady: True`, `RoutesReady: True`.
- `ingress=all` and the `allUsers` `roles/run.invoker` binding are both still intact.
- So every "is it reachable?" check you'd normally reach for says yes, while every actual request says 503.

The annotations are the only honest signal. Compare a broken service against a working one in the same project:

```bash
gcloud run services describe "$svc" --region "$r" --project "$p" \
  --format='value(metadata.annotations["run.googleapis.com/scalingMode"],
                  metadata.annotations["run.googleapis.com/manualInstanceCount"])'
```

An autoscaling (live) service returns empty for both; a disabled one returns `manual  0`.

**Testing consequence:** never score an HTTP status as a pass/fail verdict without excluding 5xx first. A probe that classifies "not 401 ⇒ authenticated" will read a 503 as success and hand you a green tick on a service that is switched off. Treat `5xx` and curl's `000` as *inconclusive* — they never reached the application, so they carry no information about whatever you were testing.

**Turning it back on:** `gcloud run services update "$svc" --region "$r" --scaling=auto` clears both annotations and restores autoscaling. It is a service-level setting, so each disabled service needs its own call.

**How it reaches you through a client:** an MCP server pointed at a disabled Cloud Run URL surfaces as `claude mcp get <name>` reporting *Failed to connect / HTTP 503: Streamable HTTP error*, with the GFE HTML page quoted inline. The bearer token in the config is a red herring — nothing ever read it.

To attribute the shutdown to a person and a date, see [[Cloud Run names who changed a service in lastModifier and the UpdateService audit log]]; for why you should ask before reversing it, [[A scaled-to-zero service in a shared cloud project is a decision, not a fault]].

## Related

- [[Cloud Run resolves a latest secret reference at instance start, not per request]]
- [[test-agent-v2 Cloud Run services use a -v2 name suffix]]

%% ai-graph-start %%

**Related notes:**
- [[A scaled-to-zero service in a shared cloud project is a decision, not a fault]]
- [[Rotating a service bearer silently invalidates every client config holding the old one]]
- [[Cloud Run 401 response body distinguishes GFEIAM rejection from app-level auth]]
- [[Cloud Run names who changed a service in lastModifier and the UpdateService audit log]]
- [[Cloud Run GFE reserves healthz — use livez for your health endpoint]]

**Relations:**
- Cloud Run — *allows disabling* — Cloud Run service
- Disabled Cloud Run service — *returns* — 503 Service Unavailable
- 503 Service Unavailable — *is* — Google Frontend HTML page
- Google Frontend HTML page — *is generated at* — Edge
- Disabled Cloud Run service — *does not reach* — Application
- Disabled Cloud Run service — *has annotation* — run.googleapis.com/scalingMode
- run.googleapis.com/scalingMode — *value is* — manual
- Disabled Cloud Run service — *has annotation* — run.googleapis.com/manualInstanceCount
- run.googleapis.com/manualInstanceCount — *value is* — 0
- Revision logs — *show* — Shutting down user disabled instance
- Instance — *is killed by* — Disabled Cloud Run service
- Startup probes — *log success for* — Instance
- Liveness probes — *log success for* — Instance
- gcloud run services describe — *reports* — Ready: True
- gcloud run services describe — *reports* — ConfigurationsReady: True
- gcloud run services describe — *reports* — RoutesReady: True
- Disabled Cloud Run service — *retains* — ingress=all
- Disabled Cloud Run service — *retains* — allUsers
- allUsers — *with* — roles/run.invoker
- Annotations — *are* — honest signal
- gcloud run services describe — *can display* — Annotations
- gcloud run services update — *enables* — Autoscaling
- gcloud run services update — *clears* — run.googleapis.com/scalingMode
- gcloud run services update — *clears* — run.googleapis.com/manualInstanceCount
- gcloud run services update — *is used to turn on* — Cloud Run service
- MCP server — *pointed at* — Disabled Cloud Run service
- MCP server — *shows* — claude mcp get <name>
- claude mcp get <name> — *reports* — HTTP 503: Streamable HTTP error
- Bearer token — *is not read by* — Disabled Cloud Run service
- Testing consequence — *relates to* — HTTP status
- HTTP status — *should exclude* — 5xx
- Probe — *can misclassify* — 503 Service Unavailable
- 503 Service Unavailable — *as* — success
- 5xx — *is* — inconclusive
- 5xx — *for* — Testing consequence
- curl's 000 — *is* — inconclusive
- curl's 000 — *for* — Testing consequence
- Cloud Run service — *is a* — Service-level setting
- Cloud Run service — *related to* — Cloud Run names who changed a service in lastModifier and the UpdateService audit log
- Cloud Run service — *related to* — A scaled-to-zero service in a shared cloud project is a decision, not a fault
- Cloud Run service — *related to* — Cloud Run resolves a latest secret reference at instance start, not per request
- Cloud Run service — *related to* — test-agent-v2 Cloud Run services use a -v2 name suffix

%% ai-graph-end %%