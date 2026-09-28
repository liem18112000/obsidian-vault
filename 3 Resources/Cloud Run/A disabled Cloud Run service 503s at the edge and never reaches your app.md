---
ai_hash: 2a390f70046df61b
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-25
entities: []
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
- [[Cloud Run 401 response body distinguishes GFEIAM rejection from app-level auth]]
- [[Cloud Run GFE reserves healthz — use livez for your health endpoint]]
- [[Cloud Run v2 has startup_probe + liveness_probe, no readiness probe]]
- [[Terraform-managed Cloud Run set env flags in TF, not gcloud run update]]
- [[Cloud Run resolves a latest secret reference at instance start, not per request]]

%% ai-graph-end %%