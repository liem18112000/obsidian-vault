---
ai_hash: 6261100fde938e9d
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-03
entities: []
source: session 2026-09-03 (test-agent deploy f7e8af5)
status: seedling
tags:
- cloud-run
- gcp
- iam
- debugging
- gotcha
title: 'Cloud Run 401: response body distinguishes GFE/IAM rejection from app-level
  auth'
type: lesson
---

# Cloud Run 401: response body distinguishes GFE/IAM rejection from app-level auth

When a Cloud Run service returns **HTTP 401** to a probe (e.g. `curl` against an MCP bridge `/mcp` endpoint), the **response body decides** whether IAM was wiped or the app rejected you — the `Server` header does not.

- **Not a discriminator:** `Server: Google Frontend` is present on *every* Cloud Run response, because GFE proxies all traffic. Seeing it tells you nothing about who returned the 401.
- **App-level 401 (healthy):** the request reached the container, so the **IAM invoker binding is intact** and auth is working as designed. You get the applications own error — e.g. `content-type: application/json` with body `{"error":"unauthorized"}` from a FastMCP/Starlette bearer-auth middleware.
- **GFE/IAM 401 (broken):** the `allUsers`/authenticated invoker binding is missing, so the request never reached the container and GFE returns a **Google HTML error page** instead.

So: **JSON app-error body = IAM fine**; **HTML GFE page = invoker binding wiped**. This lets you confirm deploy health *without valid credentials* — an unauthenticated probe is enough.

The classic cause of the HTML/GFE case is a `terraform -replace` of a `google_cloud_run_v2_service`, which drops its `allUsers` invoker `iam_member` and must be re-added. A plain `terraform apply` that only changes the image (`0 added, N changed, 0 destroyed`) does **not** touch IAM, so a JSON 401 after such an apply is the expected, healthy result.

## Related
[[terraform -replace of a cloud_run_v2_service wipes its allUsers invoker binding]]

## Related

- [[terraform -replace of a cloud_run_v2_service wipes its allUsers invoker binding]]

%% ai-graph-start %%

**Related notes:**
- [[Terraform-managed Cloud Run set env flags in TF, not gcloud run update]]
- [[A disabled Cloud Run service 503s at the edge and never reaches your app]]
- [[Cloud Run GFE reserves healthz — use livez for your health endpoint]]
- [[Cloud Run service-to-service with an app bearer needs the callee public (Authorization header collision)]]
- [[Tell FastAPI from bare Starlette by the 404 body — {detailNot Found} vs plain Not Found]]

%% ai-graph-end %%