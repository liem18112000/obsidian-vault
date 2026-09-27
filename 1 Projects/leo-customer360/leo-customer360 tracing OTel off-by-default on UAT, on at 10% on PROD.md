---
ai_hash: db0382caf5197bdc
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-21
entities:
- leo-customer360
- tracing
- OpenTelemetry
- UAT environment
- PROD environment
- FastAPI services
- API request tracing
- box sizing
- OTEL_SDK_DISABLED
- UAT api vServer
- 1 vCPU / 2 GB
- ads service
- frontend service
- Keycloak
- Redis
- Portainer
- Netdata
- Jaeger
- profiling
- 10% head sampling
- parentbased_traceidratio
- 4x8 boxes
- deployments/lib/otel.sh
- otel_env_lines
- OTEL_* env block
- deploy-api.sh
- deploy-ads.sh
- deploy-frontend.sh
- OTEL_ENABLED
- OTEL_ENDPOINT
- OTEL_SAMPLER_ARG
- Jaeger all-in-one
- badger on-disk storage
- COLLECTOR_OTLP_ENABLED
- deployments/monitoring module
- SSO-gate pattern
- jaeger_enabled
- monitoring overlays
- OTLP/HTTP
- '4318'
- grpc
- '4317'
- --network host
- 127.0.0.1:4318
- private VPC
- mon_server_key
- Jaeger UI
- '16686'
- SSH tunnel
- deployments/monitoring/README.md
- Docker containers
- VNG vServer VMs
- SSH
- zero-code instrumentation
source: session 2026-08-21
status: seedling
tags:
- leo-customer360
- tracing
- opentelemetry
- jaeger
- design-decision
title: 'leo-customer360 tracing: OTel off-by-default on UAT, on at 10% on PROD'
type: argument
---

# leo-customer360 tracing: OTel off-by-default on UAT, on at 10% on PROD

Design decision for API request tracing in **leo-customer360**: instrument all three FastAPI services with OpenTelemetry, but gate it by environment because of the box sizing.

## Decision
- **UAT: tracing OFF by default** (`OTEL_SDK_DISABLED=true`). The UAT `api` vServer is a tiny **1 vCPU / 2 GB** box already shared by api + ads + frontend + Keycloak + Redis + Portainer + Netdata. Always-on instrumentation + a running Jaeger would risk starving it. Instead we profile **on demand**: start the Jaeger container, flip the target service's `OTEL_SDK_DISABLED=false`, capture, then revert. Zero standing overhead.
- **PROD: tracing ON at 10% head sampling** (`parentbased_traceidratio`, arg 0.1). Dedicated per-service 4x8 boxes have headroom.

## Why / How
- **Why:** protect the crowded UAT box; pay for tracing only when actually debugging. PROD can afford continuous low-rate sampling.
- **How it's wired:** a shared `deployments/lib/otel.sh` (`otel_env_lines <svc> <env> [jaeger_host]`) emits the OTEL_* env block, dot-sourced by deploy-api.sh / deploy-ads.sh / deploy-frontend.sh and appended to each service's container env-file. Override per run with `OTEL_ENABLED` / `OTEL_ENDPOINT` / `OTEL_SAMPLER_ARG`.
- **Backend placement:** Jaeger all-in-one (v1, badger on-disk storage, `--memory` capped, `COLLECTOR_OTLP_ENABLED=true`) is added to the existing `deployments/monitoring` module — same deploy script + SSO-gate pattern as Portainer/Netdata. Toggled by `jaeger_enabled` in the monitoring overlays (false on uat, true on prod).
- **Transport:** OTLP/HTTP on 4318 (grpc 4317). UAT services use `--network host` so they hit `127.0.0.1:4318`; PROD per-service boxes reach the monitoring box's Jaeger over the private VPC (deploy scripts resolve `mon_server_key` private IP). Jaeger UI (16686) is loopback-bound -> SSH tunnel. Runbook: deployments/monitoring/README.md (Jaeger section).

## Related
[[leo-customer360 deploys as Docker containers on VNG vServer VMs over SSH]]
[[Trace FastAPI with OpenTelemetry zero-code instrumentation emitting OTLP]]

## Related

- [[leo-customer360 deploys as Docker containers on VNG vServer VMs over SSH]]
- [[Trace FastAPI with OpenTelemetry zero-code instrumentation emitting OTLP]]

%% ai-graph-start %%

**Related notes:**
- [[Trace FastAPI with OpenTelemetry zero-code instrumentation emitting OTLP]]
- [[leo-customer360 deploys as Docker containers on VNG vServer VMs over SSH]]
- [[Jaeger all-in-one on the shared vServer image-tag, Netdata 4317, and badger-perms gotchas]]
- [[docker restart does not re-read --env-file; recreate the container to apply env changes]]
- [[Monitoring SSO-gate adding a dashboard needs a Keycloak redirect_uri re-sync]]

**Relations:**
- leo-customer360 — *uses* — tracing
- tracing — *is implemented with* — OpenTelemetry
- leo-customer360 — *contains* — FastAPI services
- FastAPI services — *are instrumented with* — OpenTelemetry
- API request tracing — *is a type of* — tracing
- tracing — *is off-by-default on* — UAT environment
- tracing — *is on at 10% on* — PROD environment
- UAT environment — *uses* — OTEL_SDK_DISABLED
- PROD environment — *uses* — 10% head sampling
- 10% head sampling — *is configured with* — parentbased_traceidratio
- UAT environment — *has* — UAT api vServer
- UAT api vServer — *has resources* — 1 vCPU / 2 GB
- UAT api vServer — *is shared by* — ads service
- UAT api vServer — *is shared by* — frontend service
- UAT api vServer — *is shared by* — Keycloak
- UAT api vServer — *is shared by* — Redis
- UAT api vServer — *is shared by* — Portainer
- UAT api vServer — *is shared by* — Netdata
- Jaeger — *is used for* — profiling
- PROD environment — *has* — 4x8 boxes
- deployments/lib/otel.sh — *contains function* — otel_env_lines
- otel_env_lines — *emits* — OTEL_* env block
- OTEL_* env block — *is used by* — deploy-api.sh
- OTEL_* env block — *is used by* — deploy-ads.sh
- OTEL_* env block — *is used by* — deploy-frontend.sh
- OTEL_ENABLED — *overrides* — OTEL_* env block
- OTEL_ENDPOINT — *overrides* — OTEL_* env block
- OTEL_SAMPLER_ARG — *overrides* — OTEL_* env block
- Jaeger — *is deployed as* — Jaeger all-in-one
- Jaeger all-in-one — *uses* — badger on-disk storage
- Jaeger all-in-one — *has option* — COLLECTOR_OTLP_ENABLED
- Jaeger — *is part of* — deployments/monitoring module
- deployments/monitoring module — *uses* — SSO-gate pattern
- Jaeger — *is toggled by* — jaeger_enabled
- jaeger_enabled — *is found in* — monitoring overlays
- tracing — *uses transport* — OTLP/HTTP
- OTLP/HTTP — *uses port* — 4318
- tracing — *uses transport* — grpc
- grpc — *uses port* — 4317
- UAT environment — *services use* — --network host
- UAT environment — *services hit* — 127.0.0.1:4318
- PROD environment — *services reach* — Jaeger
- PROD environment — *services reach Jaeger over* — private VPC
- Jaeger UI — *uses port* — 16686
- Jaeger UI — *is accessed via* — SSH tunnel
- deployments/monitoring/README.md — *documents* — Jaeger
- leo-customer360 — *deploys as* — Docker containers
- Docker containers — *run on* — VNG vServer VMs
- VNG vServer VMs — *are accessed over* — SSH
- FastAPI — *supports* — zero-code instrumentation
- OpenTelemetry — *provides* — zero-code instrumentation

%% ai-graph-end %%