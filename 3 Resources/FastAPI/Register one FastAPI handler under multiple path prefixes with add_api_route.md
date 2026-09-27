---
title: "Register one FastAPI handler under multiple path prefixes with add_api_route"
created: 2026-09-06
type: howto
status: seedling
source: "session 2026-09-06 docs-chatbot frontend-admin"
tags: [fastapi, python, reverse-proxy, routing]
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
