---
ai_hash: 4f760e5a245018a7
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-20
entities: []
source: leo-customer360 master-profiles 500, 2026-08
status: seedling
tags:
- debugging
- cors
- http-500
- frontend
- gotcha
title: A client CORS/unreachable-API error can mask a backend 500 — read the server
  log
type: lesson
---

# A client CORS/unreachable-API error can mask a backend 500 — read the server log

A browser/SPA error like "Could not reach the API ... make sure CORS is enabled" frequently does NOT mean a network/CORS problem — if the status is **HTTP 500**, the request DID reach the server and the backend threw. Browsers surface a 500 (or its missing CORS headers on the error response) as a generic connectivity/CORS message, which misdirects debugging to the proxy/CORS when the real fault is application code. Always read the SERVER log/traceback for the true cause before touching CORS or the proxy.

Second tell from the same incident: the **UI still showed data** despite the failed call — many admin SPAs render demo/placeholder/fallback data when a fetch fails, so "there is data on screen" is not proof the API succeeded.

Concrete case: `GET /api/v1/master-profiles/` returned 500 from a `NameError` (a helper called as a bare function but only defined as a class method); sibling endpoints returned 200, proving auth/SSO/CORS were all fine.

Related: [[Redeploy OIDC consumers only after the issuer URL is reachable]]

%% ai-graph-start %%

**Related notes:**
- [[Log red herrings enclosing class name and baseline-noise lines]]
- [[Redeploy OIDC consumers only after the issuer URL is reachable]]
- [[Static vs server-backed host decides proxy-vs-direct-CORS for browser widgets]]
- [[KC_HTTP_RELATIVE_PATH moves Keycloak health endpoints under the prefix too]]
- [[Cloud Run 401 response body distinguishes GFEIAM rejection from app-level auth]]

%% ai-graph-end %%