---
ai_hash: eefdb0dd2547beb1
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-06
entities: []
source: session 2026-09-06 docs-chatbot frontend-admin
status: seedling
tags:
- fastapi
- python
- reverse-proxy
- routing
title: Register one FastAPI handler under multiple path prefixes with add_api_route
type: howto
---

# Register one FastAPI handler under multiple path prefixes with add_api_route

When a FastAPI app must serve the same endpoints at more than one path prefix -- e.g. bare `/ai/*` when run standalone AND `$FRONTEND_ROOT_PATH/ai/*` when it sits behind a reverse proxy that does NOT strip the prefix (a Caddy `handle` catch-all, not `handle_path`) -- do not copy-paste the route decorators. Define each handler once as a plain `async def`, then loop over the prefixes and register with `app.add_api_route(f'{prefix}{suffix}', endpoint, methods=[...], include_in_schema=False)`.

```python
_AI_ROUTES = [('/ask', ai_ask, ['POST']), ('/health', ai_health, ['GET'])]
prefixes = ['/ai'] + ([f'{ROOT}/ai'] if ROOT and ROOT != '/' else [])
for p in prefixes:
    for suffix, endpoint, methods in _AI_ROUTES:
        app.add_api_route(f'{p}{suffix}', endpoint, methods=methods, include_in_schema=False)
```

Why this comes up: an app served at the reverse-proxy root as a catch-all still receives the *full* path (prefix included) because the proxy forwards it unstripped, so the app itself must own both path shapes. The alternative is Starlette's `root_path`, but explicit multi-prefix registration is clearer when only a few routes need it and the same app is also run bare in local dev. Relates to [[Static vs server-backed host decides proxy-vs-direct-CORS for browser widgets]].

## Related

- [[Static vs server-backed host decides proxy-vs-direct-CORS for browser widgets]]

%% ai-graph-start %%

**Related notes:**
- [[Stripping a path prefix at the proxy breaks framework auto-redirects; forward it un-stripped when the app has root_path]]
- [[Caddy handle_path strips the path prefix, handle keeps it]]
- [[Static vs server-backed host decides proxy-vs-direct-CORS for browser widgets]]
- [[Serve a SPA under a sub-path via the app base-path option, not proxy strip]]
- [[Caddy path matcher p does not match the bare p; use a named matcher for both]]

%% ai-graph-end %%