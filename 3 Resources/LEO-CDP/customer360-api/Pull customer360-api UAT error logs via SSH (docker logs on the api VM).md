---
ai_hash: 4f11354ad489b4b5
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-16
entities: []
source: session 2026-09-16
status: seedling
tags:
- leo-cdp
- customer360-api
- logging
- ssh
- observability
- gotcha
title: Pull customer360-api UAT error logs via SSH (docker logs on the api VM)
type: howto
---

# Pull customer360-api UAT error logs via SSH (docker logs on the api VM)

customer360-api runtime logs are **not aggregated** (no Loki/CloudWatch) — they are plain Docker `json-file` logs on the remote "api" VM (container `customer360-api`, run with `--log-opt max-size=10m --log-opt max-file=3`), reached over SSH. Portainer on the box offers the same logs via UI.

## How to pull them (UAT)
1. `cd leo-customer360/deployments/server`; `set -a; source ./.env; set +a` (that `.env` holds the AWS creds for the S3 terraform backend); `terraform init`; `terraform workspace select uat`.
2. Resolve the api box floating IP: `terraform output -json servers` → key `api` → `internal_interfaces[].floating_ip` (was `49.213.71.76` on UAT).
3. `ssh -i ~/.ssh/c360-api_ed25519 leocdp360@<ip>` then `sudo docker logs --since 4h --timestamps customer360-api`.

## Gotcha — OTEL noise swamps the tail
OpenTelemetry/OTLP trace-exporter failures (Jaeger unreachable: `Failed to export span batch`, `Connection reset by peer`) make up ~47% of the log volume. A plain `docker logs --tail 200` returns *only* OTEL lines and hides every real error. **Always `grep -viE 'opentelemetry|otlp'` BEFORE tailing.**

## Real-error shape to look for
The S3 events endpoint builds its boto3 client with `region_name=settings.event_s3_region` (env `S3_REGION`, alias `event_s3_region`) in `core/repositories/event_query_repository.py:92`. A bad `S3_REGION` value → `botocore.exceptions.InvalidRegionError` → HTTP 500 on `GET /c360api/api/v1/events`. Seen on UAT when the deploy wrote a base64 blob into `S3_REGION` instead of a real region.

Related: [[customer360-api]]

## Related

- [[customer360-api]]

%% ai-graph-start %%

**Related notes:**
- [[Validate S3_REGION at the deploy boundary, not after boto3 fails]]
- [[Verify uat customer360-api health publicly at beta.leocdp.comc360apihealth]]
- [[Running leo-customer360 deploys locally needs vStorage backend creds; CI can't do monitoringLB]]
- [[leo-customer360 tracing OTel off-by-default on UAT, on at 10% on PROD]]
- [[customer360-api events reader per-source vs single-bucket mode]]

%% ai-graph-end %%