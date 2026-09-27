---
ai_hash: 0966cdc1f3b5ff5b
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-21
entities:
- Jaeger all-in-one
- Docker container
- leo-customer360 monitoring box
- Netdata
- Docker Hub
- jaegertracing/all-in-one:1.62
- jaegertracing/all-in-one:1.62.0
- jaegertracing/all-in-one:1.63.0
- Jaeger v1 all-in-one
- jaegertracing/all-in-one
- Jaeger v2
- jaegertracing/jaeger
- Netdata otel-plugin
- Host port 4317 (OTLP gRPC)
- Jaeger gRPC 4317
- OTLP/HTTP 4318
- OTel exporters
- Badger storage
- Docker named volume
- Non-root UID (10001)
- Root user
- customer360-api
- OpenTelemetry instrumentation
- CI
- OTel :latest image
- Locally-built Docker image
- CD
- GHCR redeploy
- '`docker inspect` command'
- FastAPI
- OpenTelemetry zero-code instrumentation
- OTLP
- leo-customer360 tracing
- UAT environment
- PROD environment
source: session 2026-08-21
status: seedling
tags:
- leo-customer360
- jaeger
- docker
- netdata
- badger
- gotcha
- ghcr
title: 'Jaeger all-in-one on the shared vServer: image-tag, Netdata :4317, and badger-perms
  gotchas'
type: lesson
---

# Jaeger all-in-one on the shared vServer: image-tag, Netdata :4317, and badger-perms gotchas

Three gotchas hit deploying **Jaeger all-in-one** as a Docker container on the leo-customer360 monitoring box (co-located with Netdata), plus a stale-image finding.

## 1. Image tag format: 1.x.Y not 1.x
`jaegertracing/all-in-one:1.62` **does not exist** on Docker Hub (`... not found`). The published pins are patch-versioned: **`1.62.0`**, `1.63.0`, … (a bare `1.60`/`1.59` exists for some minors but NOT 1.62). Always pin the full `1.x.y`. (Jaeger v1 all-in-one is `jaegertracing/all-in-one`; v2 is `jaegertracing/jaeger` with a different config model.)

## 2. Netdata's otel-plugin owns host :4317 -> OTLP gRPC conflict
Recent `netdata/netdata:stable` (host network) bundles an **`otel-plugin` that LISTENS on 127.0.0.1:4317** (OTLP gRPC). A co-located Jaeger publishing `-p 0.0.0.0:4317:4317` fails: **`bind host port 0.0.0.0:4317: address already in use`**. Fix: **do NOT publish Jaeger's gRPC 4317 on the host** — publish only OTLP/HTTP `:4318` (which is what the OTel exporters use here anyway). gRPC still works in-container; it's just not host-published.

## 3. Badger storage on a named volume needs a writable dir
Jaeger all-in-one runs as a **non-root uid (10001)**; a fresh Docker **named volume is root-owned**, so `BADGER_DIRECTORY_KEY=/badger/key` fails with **`mkdir /badger/key: permission denied`** and the container crash-loops. Fix: run the container **`--user root`** (or pre-chown the volume to 10001). We chose `--user root` for this internal dev tool.

## 4. Stale image: 'CD deployed' but the box ran an old local build
Symptom: deployed `customer360-api` had no `opentelemetry-instrument` even though CI built+pushed an OTel `:latest`. Cause: the box was running a **locally-built `customer360-api:latest` (image with no registry prefix)** from an old `BUILD_LOCAL` run, and its pulled `ghcr.io/.../customer360-api:latest` copy was stale — CD's `--deploy-uat` never refreshed it. Fix: a clean GHCR redeploy (`./deploy-api.sh uat`, default mode) `docker pull`s the fresh `:latest` and runs it. Tell by `docker inspect --format {{.Config.Image}}`: a bare name = local build; `ghcr.io/...` = the CI image.

## Related
[[Trace FastAPI with OpenTelemetry zero-code instrumentation emitting OTLP]]
[[leo-customer360 tracing OTel off-by-default on UAT, on at 10% on PROD|leo-customer360 tracing: OTel off-by-default on UAT, on at 10% on PROD]]

## Related

- [[Trace FastAPI with OpenTelemetry zero-code instrumentation emitting OTLP]]
- [[leo-customer360 tracing OTel off-by-default on UAT, on at 10% on PROD]]

%% ai-graph-start %%

**Related notes:**
- [[leo-customer360 tracing OTel off-by-default on UAT, on at 10% on PROD]]
- [[Monitoring SSO-gate adding a dashboard needs a Keycloak redirect_uri re-sync]]
- [[docker restart does not re-read --env-file; recreate the container to apply env changes]]
- [[Jaeger on a small box use in-memory bounded storage, not badger, to avoid OOM 502]]
- [[Trace FastAPI with OpenTelemetry zero-code instrumentation emitting OTLP]]

**Relations:**
- Jaeger all-in-one — *deployed_as* — Docker container
- Docker container — *runs_on* — leo-customer360 monitoring box
- leo-customer360 monitoring box — *co-located_with* — Netdata
- jaegertracing/all-in-one:1.62 — *does_not_exist_on* — Docker Hub
- jaegertracing/all-in-one:1.62.0 — *is_a_published_pin_for* — Jaeger all-in-one
- jaegertracing/all-in-one:1.63.0 — *is_a_published_pin_for* — Jaeger all-in-one
- Jaeger v1 all-in-one — *uses_image* — jaegertracing/all-in-one
- Jaeger v2 — *uses_image* — jaegertracing/jaeger
- Netdata otel-plugin — *listens_on* — Host port 4317 (OTLP gRPC)
- Jaeger gRPC 4317 — *conflicts_with* — Netdata otel-plugin
- OTLP/HTTP 4318 — *used_by* — OTel exporters
- Jaeger all-in-one — *uses* — Badger storage
- Badger storage — *requires* — writable dir
- Docker named volume — *is* — root-owned
- Jaeger all-in-one — *runs_as* — Non-root UID (10001)
- Non-root UID (10001) — *causes_permission_denied_on* — Docker named volume
- Root user — *is_a_fix_for* — permission denied
- customer360-api — *lacked* — OpenTelemetry instrumentation
- CI — *built_and_pushed* — OTel :latest image
- Locally-built Docker image — *was_running_for* — customer360-api
- CD — *failed_to_refresh* — Locally-built Docker image
- GHCR redeploy — *pulls* — OTel :latest image
- `docker inspect` command — *identifies* — Locally-built Docker image
- `docker inspect` command — *identifies* — CI image
- FastAPI — *can_be_traced_with* — OpenTelemetry zero-code instrumentation
- OpenTelemetry zero-code instrumentation — *emits* — OTLP
- leo-customer360 tracing — *is_off_by_default_in* — UAT environment
- leo-customer360 tracing — *is_on_at_10_percent_in* — PROD environment

%% ai-graph-end %%