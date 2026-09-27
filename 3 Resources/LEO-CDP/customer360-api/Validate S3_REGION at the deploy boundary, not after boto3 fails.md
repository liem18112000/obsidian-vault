---
ai_hash: a78f8dca1a2b5846
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-16
entities: []
source: session 2026-09-16
status: seedling
tags:
- leo-cdp
- customer360-api
- deploy
- boto3
- security
- gotcha
- input-validation
title: Validate S3_REGION at the deploy boundary, not after boto3 fails
type: lesson
---

# Validate S3_REGION at the deploy boundary, not after boto3 fails

On UAT, `customer360-api` returned HTTP 500 on every `GET /c360api/api/v1/events` for hours. Root cause: `S3_REGION` in `/opt/c360/api.env` was a 419-char **base64 blob** (the SMTP env block, incl. the Brevo key) instead of a region. `event_query_repository._build_s3_client` passes it to `boto3.client("s3", region_name=...)` → `botocore.exceptions.InvalidRegionError` → unhandled 500. Worse, the blob decoded to a live SMTP secret, so the secret became **recoverable from the container logs**.

## How it got there
The committed `deploy-api.sh` is correct (a clean run resolves `S3_REGION=us-east-1`). The bad value came from **env contamination**: the deploy ran with `S3_REGION` exported to the SMTP base64 (a copy-paste/`export` accident during SMTP feature work). `EVENT_S3_REGION="${S3_REGION:-us-east-1}"` trusted it and baked it straight into `api.env`.

## Decision: guard at the boundary, fail loud
Fixed in `deploy-api.sh` by rejecting a non-region value *before* writing `api.env`, rather than adding try/except in the app (which would only hide misconfig) or silently falling back (which could point at the wrong bucket region):
```bash
[[ "$EVENT_S3_REGION" =~ ^[a-z0-9-]{1,32}$ ]] || { echo "ERROR: S3_REGION=... not a region"; exit 1; }
```
The `^[a-z0-9-]{1,32}$` shape accepts every real AWS region (`us-east-1`, `us-gov-east-1`) and rejects base64 (uppercase/`+`/`=`/length). Validating untrusted config at the trust boundary turns a silent multi-hour outage + secret leak into a loud deploy-time failure.

## Remediation beyond the code
1. Re-run `./deploy-api.sh uat` to overwrite the bad `api.env` (regenerates `S3_REGION=us-east-1`).
2. **Rotate the Brevo SMTP key** — it sat in the logs in recoverable form.

Related: [[Pull customer360-api UAT error logs via SSH (docker logs on the api VM)]]

## Related

- [[Pull customer360-api UAT error logs via SSH (docker logs on the api VM)]]

%% ai-graph-start %%

**Related notes:**
- [[boto3 client errors echo regionkeys into logs; sanitize at construction with 'from None']]
- [[Pull customer360-api UAT error logs via SSH (docker logs on the api VM)]]
- [[ssh drops empty positional args; pass a base64 newline-joined argv + mapfile]]
- [[Configure vStorage S3 backend creds in each component .env so deploy scripts self-auth]]
- [[customer360-api events reader per-source vs single-bucket mode]]

%% ai-graph-end %%