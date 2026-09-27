---
ai_hash: b0ec589fa6d4b3cb
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-06
entities: []
source: session 2026-09-06 docs-chatbot plan
status: seedling
tags:
- web
- cors
- reverse-proxy
- architecture
- security
title: Static vs server-backed host decides proxy-vs-direct-CORS for browser widgets
type: lesson
---

# Static vs server-backed host decides proxy-vs-direct-CORS for browser widgets

When embedding a browser chatbot/widget that must call a backend API, the *type of host page* decides how to reach the API safely:

- **Static site** (GitHub Pages, a Quartz build, any CDN-served SPA with no server of its own): there is no backend to proxy through, so the widget must call the API's **public URL directly**. That forces, on the API side: **CORS** with an *exact-origin* allow-list (never `*` on an unauthenticated endpoint), a **public reverse-proxy route** (e.g. a Caddy `handle_path` block), and **rate limiting**.
- **Server-backed app** (e.g. a FastAPI-served SPA): add a thin **same-origin server-side proxy route** that forwards to the API over the private network. This is strictly better — the browser call is same-origin so there is **no CORS surface**, the backend API **stays unexposed** to the internet, and you can **reuse the app's existing session** to authenticate the call.

So the decision rule is: *proxy server-side when a server exists; only fall back to direct-call-with-CORS when the host page is static.* A single API can support both at once (adding CORS is harmless for the proxied path).

Surfaced while planning a docs RAG chatbot embedded in both a static Quartz docs site and a FastAPI admin SPA, both consuming the same search service.

## Related

- [[Public unauthenticated CPU-bound LLM endpoint is a DoS foot-gun]]

%% ai-graph-start %%

**Related notes:**
- [[Public unauthenticated CPU-bound LLM endpoint is a DoS foot-gun]]
- [[Register one FastAPI handler under multiple path prefixes with add_api_route]]
- [[Injecting a custom Quartz component when the engine is cloned fresh in CI]]
- [[Two-phase RAG chatbot UX fast retrieval first, slow generation second]]
- [[Serve a SPA under a sub-path via the app base-path option, not proxy strip]]

%% ai-graph-end %%