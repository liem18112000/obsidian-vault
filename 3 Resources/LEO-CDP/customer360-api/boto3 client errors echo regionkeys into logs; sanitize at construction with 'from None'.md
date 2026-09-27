---
ai_hash: 383789fa3d2035a3
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-16
entities: []
source: session 2026-09-16
status: seedling
tags:
- leo-cdp
- customer360-api
- security
- boto3
- logging
- fastapi
- exception-handling
title: boto3 client errors echo region/keys into logs; sanitize at construction with
  'from None'
type: lesson
---

# boto3 client errors echo region/keys into logs; sanitize at construction with 'from None'

When `boto3.client("s3", region_name=..., aws_access_key_id=...)` gets a bad value, botocore raises an error whose **message embeds the offending value** (e.g. `InvalidRegionError: Provided region_name <value> doesnt match a supported format`). If that value is sensitive (or a config field that got contaminated with a secret), the value leaks into any log that prints the exception.

## Why the apps existing try/except did not catch it
In `customer360-api`, the S3 client is built eagerly in `EventQueryRepository.__init__`, which runs inside FastAPIs `Depends(get_event_query_repository)` — i.e. BEFORE the route handlers `try/except EventQueryError`. So the construction error was unhandled → Starlettes default 500 handler logged the full traceback (value included). Query-TIME boto errors were already sanitized; only the CONSTRUCTION path leaked.

## Fix: sanitize at construction, drop the cause chain
```python
try:
    return boto3.client("s3", **client_kwargs)
except (BotoCoreError, ClientError):
    raise EventQueryError("Invalid S3 client configuration") from None
```
`from None` (sets `__suppress_context__`) is essential: `raise X from exc` would still print the original botocore message (with the value) as the tracebacks "direct cause". Verified: with `from None`, the value appears in neither `str(err)` nor `traceback.format_exception(...)`. No behaviour change for a valid config.

## General rule
Never let a provider SDK exception reach the logs verbatim when its message can contain config/secret values — catch at the boundary and re-raise a message you control, without chaining. Reuse the modules existing sanitized error type rather than inventing one.

Related: [[Validate S3_REGION at the deploy boundary, not after boto3 fails]]

## Related

- [[Validate S3_REGION at the deploy boundary, not after boto3 fails]]

%% ai-graph-start %%

**Related notes:**
- [[Validate S3_REGION at the deploy boundary, not after boto3 fails]]
- [[Pull customer360-api UAT error logs via SSH (docker logs on the api VM)]]
- [[Module-level load_dotenv lets unit tests hit real cloud credentials]]
- [[ssh drops empty positional args; pass a base64 newline-joined argv + mapfile]]

%% ai-graph-end %%