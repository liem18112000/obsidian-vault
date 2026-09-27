---
ai_hash: 057b06a322df1ecb
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-27
entities: []
source: session 2026-08-27 — kga deployments/main.tf
status: seedling
tags:
- cloud-run
- terraform
- health-check
- gcp
- gotcha
title: Cloud Run v2 has startup_probe + liveness_probe, no readiness probe
type: lesson
---

# Cloud Run v2 has startup_probe + liveness_probe, no readiness probe

Cloud Run v2 (`google_cloud_run_v2_service`) supports **`startup_probe`** and **`liveness_probe`** inside `template.containers`, but there is **no readiness probe** (unlike Kubernetes) — Cloud Run gates traffic on the startup probe instead. So an app can expose `/readyz` for humans/monitoring, but Terraform only wires `/healthz`-style liveness + startup.

Details that matter:
- `http_get.port` **defaults to the container serving port** (8080 / the injected `$PORT`), so you can omit it.
- **`startup_probe.failure_threshold * period_seconds`** is your cold-start budget — e.g. `failure_threshold=10`, `period_seconds=5` ≈ 50s before the revision is marked failed. Set this generously for slow imports.
- Liveness restarts the container on repeated failure; keep its `period_seconds` longer (e.g. 30s) to avoid churn.
- Both take `http_get { path = "/healthz" }`; the health handler must return 200 fast and cheap (no dependency calls) — put dependency checks behind a separate readiness endpoint.

%% ai-graph-start %%

**Related notes:**
- [[Cloud Run GFE reserves healthz — use livez for your health endpoint]]
- [[Cloud Run v2 multi-container sidecar in Terraform]]
- [[Cloud Run v2 deletion_protection defaults true — set false and apply before destroy]]
- [[A disabled Cloud Run service 503s at the edge and never reaches your app]]
- [[Cloud Run v2 service design gotchas]]

%% ai-graph-end %%