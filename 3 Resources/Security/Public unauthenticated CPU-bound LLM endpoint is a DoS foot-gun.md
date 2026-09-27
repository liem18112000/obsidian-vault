---
ai_hash: 3d05fd15ab730160
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-06
entities: []
source: session 2026-09-06 docs-chatbot plan
status: seedling
tags:
- security
- llm
- dos
- rate-limiting
- rag
title: Public unauthenticated CPU-bound LLM endpoint is a DoS foot-gun
type: lesson
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

%% ai-graph-start %%

**Related notes:**
- [[Two-phase RAG chatbot UX fast retrieval first, slow generation second]]
- [[Static vs server-backed host decides proxy-vs-direct-CORS for browser widgets]]
- [[docs-vector-search OOMs on ask on a 1vCPU2GB box (Qwen KV cache over RAM+swap)]]
- [[IP rate limiting must honor X-Forwarded-For behind a proxy]]
- [[Bound ThreadPoolExecutor + budget keeps per-item LLM scoring inside a web request window]]

%% ai-graph-end %%