---
ai_hash: 2a10e193ec17a171
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-08
entities: []
source: session 2026-09-08 — docs-vector-search hardening
status: seedling
tags:
- security
- rate-limiting
- reverse-proxy
- x-forwarded-for
- fail-closed
title: Absence of X-Forwarded-For must not mean trusted internal caller
type: lesson
---

# Absence of X-Forwarded-For must not mean trusted internal caller

When a service sits behind a reverse proxy (Caddy/nginx/LB), it is tempting to treat "request arrived with no `X-Forwarded-For`" as "came from our internal network, so trust it / skip the rate limit". This is a security bug: an attacker who can reach the service off-path (an exposed port, SSRF, a nested/misconfigured proxy) simply **omits the header** and inherits the internal exemption — unlimited access to the expensive endpoint.

**Correct pattern:**
- **Identify internal callers by a shared secret header**, not by header absence. Compare in constant time (`hmac.compare_digest`). No secret configured ⇒ nobody is exempt.
- **Derive the client IP from a configured trusted proxy-hop count** — the real client is the Nth entry from the right of the XFF chain (Caddy appends 1 ⇒ rightmost). Left-hand entries are attacker-controlled and must not pick the bucket.
- **Fail closed:** when forwarding info is missing or the chain is shorter than expected, fall back to the direct peer IP and *still* rate-limit — never exempt.

**Two related limiter gotchas:**
- A per-process in-memory limiter dict (ip → hits) never evicts and grows with every distinct IP ever seen ⇒ unbounded memory/OOM. Sweep buckets whose newest hit is older than the window (throttled to once/window).
- With >1 worker, an in-process limit multiplies by worker count (and gate/state leaks). Divide budgets by `WEB_CONCURRENCY`, run a single worker, or back the limiter with a shared store (Redis).

## Related

- [[Empty array in Postgres WHERE NOT (id = ANY(...)) deletes every row]]

%% ai-graph-start %%

**Related notes:**
- [[IP rate limiting must honor X-Forwarded-For behind a proxy]]
- [[data-tracking rate limiter caps global throughput because it reads request.client.host behind a proxy]]
- [[Fail-open bearer auth middleware antipattern]]
- [[Public unauthenticated CPU-bound LLM endpoint is a DoS foot-gun]]
- [[A fixed-window rate limiter set to exactly the target rate rejects part of a paced stream at that rate]]

%% ai-graph-end %%