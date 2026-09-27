---
ai_hash: 6acf0b978a537d22
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-21
entities:
- Jaeger all-in-one
- Docker container
- leo-customer360 monitoring box
- Netdata
- image tag format
- Docker Hub
- jaegertracing/all-in-one:1.62
- 1.62.0
- 1.63.0
- '1.60'
- '1.59'
- Jaeger v1 all-in-one
- jaegertracing/all-in-one
- Jaeger v2
- jaegertracing/jaeger
- config model
- Netdata's otel-plugin
- netdata/netdata:stable
- 127.0.0.1:4317
- OTLP gRPC
- Jaeger's gRPC port 4317
- host
- OTLP/HTTP :4318
- OTel exporters
- Badger storage
- named volume
- non-root uid 10001
- root ownership
- BADGER_DIRECTORY_KEY=/badger/key
- permission denied error
- container crash-loop
- --user root
- volume
- internal dev tool
- Stale image
- customer360-api
- opentelemetry-instrument
- CI
- OTel :latest
- locally-built customer360-api:latest
- local image build
- GHCR image
- CD deploy-uat command
- GHCR redeploy
- ./deploy-api.sh uat
- docker pull
- docker inspect command
- FastAPI
- OpenTelemetry zero-code instrumentation
- OTLP
- leo-customer360 tracing
- OTel
- UAT
- PROD
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
- Jaeger all-in-one — *deployed as* — Docker container
- Docker container — *runs on* — leo-customer360 monitoring box
- leo-customer360 monitoring box — *co-located with* — Netdata
- jaegertracing/all-in-one:1.62 — *not found on* — Docker Hub
- image tag format — *should be* — 1.x.Y
- 1.62.0 — *is a* — patch-versioned pin
- 1.63.0 — *is a* — patch-versioned pin
- 1.60 — *is a* — minor version pin
- 1.59 — *is a* — minor version pin
- Jaeger v1 all-in-one — *is* — jaegertracing/all-in-one
- Jaeger v2 — *is* — jaegertracing/jaeger
- Jaeger v2 — *has* — different config model
- netdata/netdata:stable — *bundles* — Netdata's otel-plugin
- Netdata's otel-plugin — *listens on* — 127.0.0.1:4317
- 127.0.0.1:4317 — *is for* — OTLP gRPC
- Jaeger all-in-one — *publishing Jaeger's gRPC port 4317 causes* — address already in use
- address already in use — *occurs on* — host
- Jaeger all-in-one — *should publish* — OTLP/HTTP :4318
- OTel exporters — *use* — OTLP/HTTP :4318
- Badger storage — *requires* — writable dir
- Jaeger all-in-one — *runs as* — non-root uid 10001
- named volume — *has* — root ownership
- BADGER_DIRECTORY_KEY=/badger/key — *fails with* — permission denied error
- permission denied error — *causes* — container crash-loop
- container crash-loop — *fixed by* — --user root
- container crash-loop — *fixed by* — pre-chown volume to 10001
- internal dev tool — *uses* — --user root
- deployed customer360-api — *lacked* — opentelemetry-instrument
- CI — *built and pushed* — OTel :latest
- box — *ran* — locally-built customer360-api:latest
- locally-built customer360-api:latest — *is a* — local image build
- GHCR image — *was* — stale
- CD deploy-uat command — *did not refresh* — stale image
- stale image — *fixed by* — GHCR redeploy
- GHCR redeploy — *uses* — ./deploy-api.sh uat
- ./deploy-api.sh uat — *performs* — docker pull
- docker inspect command — *identifies* — local image build
- docker inspect command — *identifies* — CI image
- FastAPI — *uses* — OpenTelemetry zero-code instrumentation
- OpenTelemetry zero-code instrumentation — *emits* — OTLP
- leo-customer360 tracing — *uses* — OTel
- OTel — *is off-by-default on* — UAT
- OTel — *is on at 10% on* — PROD

%% ai-graph-end %%