---
ai_hash: 77cb43ad0e141a77
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-05
entities: []
source: c360 python code review 2026-09-05
status: seedling
tags:
- rate-limiting
- proxy
- security
- gotcha
title: IP rate limiting must honor X-Forwarded-For behind a proxy
type: lesson
---

# IP rate limiting must honor X-Forwarded-For behind a proxy

Rate limiters that key on `request.client.host` (the immediate socket peer) are miscoped behind a reverse proxy / load balancer, where that address is the proxy IP for every request. All clients then share one counter: one abuser trips the limit for everyone (DoS), and real per-client throttling never engages.

Behind a trusted proxy, derive the client IP from a validated `X-Forwarded-For` (leftmost untrusted hop) or the platform's real-IP header, and scope brute-force limits per account/username too, not IP alone.

%% ai-graph-start %%

**Related notes:**
- [[Absence of X-Forwarded-For must not mean trusted internal caller]]
- [[data-tracking rate limiter caps global throughput because it reads request.client.host behind a proxy]]
- [[A fixed-window rate limiter set to exactly the target rate rejects part of a paced stream at that rate]]
- [[Public unauthenticated CPU-bound LLM endpoint is a DoS foot-gun]]
- [[Acquire a client-side rate limiter once per call, outside the retry loop]]

%% ai-graph-end %%