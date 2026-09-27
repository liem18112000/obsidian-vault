---
title: "Public unauthenticated CPU-bound LLM endpoint is a DoS foot-gun"
created: 2026-09-06
type: lesson
status: seedling
source: "session 2026-09-06 docs-chatbot plan"
tags: [security, llm, dos, rate-limiting, rag]
---

# Public unauthenticated CPU-bound LLM endpoint is a DoS foot-gun

An LLM inference endpoint (e.g. a local RAG service generating with a small GGUF model on **1 vCPU / 2 GB**) is **CPU-bound and slow per request**. Exposing it **publicly and unauthenticated** is a denial-of-service foot-gun: a handful of concurrent requests saturate the box and starve every other user, no attacker sophistication required.

Mitigations before any public exposure:
- **Rate limit** at the edge (per-IP, e.g. a few requests/min) — reverse-proxy plugin or in-app token bucket.
- **Concurrency cap**: a module-level semaphore (`asyncio.Semaphore(1-2)`) around generation so calls **queue** instead of thrashing the CPU.
- **Cap output length** (max generated tokens) to bound worst-case cost per call.
- Prefer to **not expose it at all**: front it with a server-side proxy that requires a session (see [[Static vs server-backed host decides proxy-vs-direct-CORS for browser widgets]]), or gate the public route behind a lightweight token / challenge.

The generic principle: any unauthenticated endpoint whose per-request cost is high (LLM, image processing, report generation) needs cost-based throttling, not just the usual request-count limits.

## Related

- [[Static vs server-backed host decides proxy-vs-direct-CORS for browser widgets]]
