---
ai_hash: 4ad7ba33a883b9e4
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-21
entities: []
source: session 2026-08-21
status: seedling
tags:
- opentelemetry
- OTLP
- jaeger
- tracing
- FastAPI
- observability
title: Trace FastAPI with OpenTelemetry zero-code instrumentation emitting OTLP
type: howto
---

# Trace FastAPI with OpenTelemetry zero-code instrumentation emitting OTLP

The **fastest, lightest way to profile/trace requests through a FastAPI service** (esp. on plain VMs) is **OpenTelemetry zero-code auto-instrumentation** emitting **OTLP** — no application code changes.

## Instrument (zero code)
1. Add deps: `opentelemetry-distro` + `opentelemetry-exporter-otlp`.
2. In the Dockerfile: `opentelemetry-bootstrap -a install` — auto-installs instrumentors for FastAPI, SQLAlchemy, Redis, psycopg2, httpx/requests.
3. Prefix the process: `opentelemetry-instrument uvicorn app:app ...`.

You get, for free: the HTTP span, **every SQL query**, Redis calls, and outbound HTTP as child spans — and automatic **W3C `traceparent`** propagation, so a request crossing multiple services stitches into **one** trace.

## Emit OTLP — the key decoupling
**OTLP** is the vendor-neutral wire format, so you instrument once and switch backends by a single env var:
- **Jaeger >= 1.35** natively receives OTLP on **4317** (gRPC) / **4318** (HTTP) when `COLLECTOR_OTLP_ENABLED=true`; UI on **:16686**.
- Grafana Tempo and managed clouds (e.g. **VNG Cloud vMonitor**) also ingest OTLP.

> Gotcha / term: **Jaeger's own client SDKs are RETIRED.** The Jaeger project now tells you to use OpenTelemetry SDKs. So "use Jaeger" today = "instrument with OTel, view in Jaeger."

## Config env vars
- `OTEL_SERVICE_NAME` (per service)
- `OTEL_TRACES_EXPORTER=otlp`
- `OTEL_EXPORTER_OTLP_ENDPOINT`, `OTEL_EXPORTER_OTLP_PROTOCOL` (grpc/http)
- `OTEL_TRACES_SAMPLER=parentbased_traceidratio` + `OTEL_TRACES_SAMPLER_ARG` (1.0 dev / 0.1 prod)
- `OTEL_SDK_DISABLED=true` — fully no-op; use on a resource-tight box and flip on only while profiling.
- `OTEL_PYTHON_LOG_CORRELATION=true` — stamps `trace_id` into logs.

## Prod: put an OTel Collector in front
To decouple apps from the backend, fan out, or centralize sampling: apps -> **localhost OTel Collector** -> exporter(s). Only the collector knows the vendor, so swapping Jaeger <-> VNG managed <-> Tempo needs **no app change**.

## Related
[[leo-customer360 deploys as Docker containers on VNG vServer VMs over SSH]]

## Related

- [[leo-customer360 deploys as Docker containers on VNG vServer VMs over SSH]]

%% ai-graph-start %%

**Related notes:**
- [[leo-customer360 tracing OTel off-by-default on UAT, on at 10% on PROD]]
- [[Jaeger Dependencies DAG is empty until a trace spans 2 services; force one with opentelemetry-instrument from inside a container]]
- [[Jaeger all-in-one on the shared vServer image-tag, Netdata 4317, and badger-perms gotchas]]
- [[leo-customer360 deploys as Docker containers on VNG vServer VMs over SSH]]
- [[docker restart does not re-read --env-file; recreate the container to apply env changes]]

%% ai-graph-end %%