---
ai_hash: ab0af752d0eb5f9a
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-10
entities: []
source: leo-customer360 UAT deploy session 2026-09-10
status: seedling
tags:
- fastapi
- starlette
- gotcha
- leo-customer360
- customer360-api
- introspection
title: FastAPI _IncludedRouter hides routes from app.routes introspection
type: lesson
---

# FastAPI _IncludedRouter hides routes from app.routes introspection

In newer FastAPI/Starlette, `app.include_router()` no longer flattens the router's endpoints onto `app.routes` as `APIRoute` objects. Each `include_router` call instead adds one `_IncludedRouter` wrapper object that has **no** `.path`, `.prefix`, or `.routes` attribute. So naive introspection like `getattr(r, "path", "?") for r in app.routes` reports **zero** business routes and can make a perfectly healthy API look broken.

**Where the routes actually are:** `route.original_router.routes`. The paths there are router-relative (e.g. `/admin/crm/sync-runs`); prepend the app-level prefix the router was included with (e.g. `/api/v1`) to get the full path.

```python
from app import app
PFX = "/api/v1"
for ir in app.routes:
    orig = getattr(ir, "original_router", None)
    if not orig:
        continue
    for rt in getattr(orig, "routes", []):
        print(",".join(sorted(getattr(rt, "methods", []) or [])), PFX + rt.path)
```

**Also unreliable as a route inventory:** `/openapi.json` can return `{"paths": {}}` when the app customizes its OpenAPI schema or runs in SSO mode — it is not proof the routes are missing.

**And HTTP status cannot distinguish it here:** customer360-api builds its app via `create_http_api_app()` with `root_path="/c360api"` and an SSO `auth_middleware` that returns **401 before routing**. So a request to a non-existent path still returns 401, not 404 — you cannot tell "route missing" from "unauthenticated" by status code. To truly confirm a route is registered, walk `original_router` (above) or make an authenticated end-to-end call.

Discovered while smoke-testing a UAT deploy: introspection showed 0 crm routes; walking `original_router` revealed 210 real routes incl. the 3 new `crm_sync` endpoints.

See also [[Deploy an unmerged feature branch to leo-customer360 UAT with BUILD_LOCAL=1]].

## Related

- [[Deploy an unmerged feature branch to leo-customer360 UAT with BUILD_LOCAL=1]]

%% ai-graph-start %%

**Related notes:**
- [[Tell FastAPI from bare Starlette by the 404 body — {detailNot Found} vs plain Not Found]]
- [[Register one FastAPI handler under multiple path prefixes with add_api_route]]
- [[customer360-api tenant-admin router tests must inject an admin request.state.user]]
- [[Deploy an unmerged feature branch to leo-customer360 UAT with BUILD_LOCAL=1]]
- [[Stripping a path prefix at the proxy breaks framework auto-redirects; forward it un-stripped when the app has root_path]]

%% ai-graph-end %%