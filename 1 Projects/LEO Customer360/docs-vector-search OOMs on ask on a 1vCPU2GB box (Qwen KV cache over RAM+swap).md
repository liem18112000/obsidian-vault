---
title: "docs-vector-search OOMs on /ask on a 1vCPU/2GB box (Qwen KV cache over RAM+swap)"
created: 2026-09-06
type: observation
status: seedling
source: "session 2026-09-06 docs-chatbot UAT deploy"
tags: [leo-customer360, rag, llm, oom, memory, llama-cpp, ops]
---

# docs-vector-search OOMs on /ask on a 1vCPU/2GB box (Qwen KV cache over RAM+swap)

The docs-vector-search RAG service on its UAT box (1 vCPU / **2 GB**) OOM-kills uvicorn during POST /ask generation. e5 embed + bge reranker + Qwen2.5-0.5B are ~1.4 GB resident; the llama.cpp KV cache during generation (GEN_CTX=4096, GEN_MAX_TOKENS=512) pushes past RAM + the 2 GB swapfile, and the kernel OOM-kills the process (seen: `Out of memory: Killed process ... task=uvicorn` + `double free or corruption`). The container auto-restarts (restart unless-stopped), so a slow client sees curl 52 "Empty reply from server" after minutes, or the fronting proxy 502s at its timeout.

Symptoms map: GET /health and POST /search are cheap and always work; POST /ask (generation) is the one that OOMs. Fast /search + slow/failing /ask is the tell.

Mitigations (cheap -> robust):
1. Lower generation memory: GEN_CTX 4096->2048 and GEN_MAX_TOKENS 512->256 (smaller KV cache, also faster). Shorter answers.
2. RERANK_ENABLED=false frees ~300 MB (README's first suggested lever); /search still works via vector similarity, ranking slightly worse.
3. Resize the box to 2 vCPU / 4 GB (terraform flavor change + reboot + cost) — the real fix.
Also bump the consumer's proxy timeout: a browser chatbot proxy at 60s is too short even for a healthy generation on this box.

Applies to [[Two-phase RAG chatbot UX: fast retrieval first, slow generation second]] (why /search-first still delivers value when /ask is degraded) and [[Public unauthenticated CPU-bound LLM endpoint is a DoS foot-gun]]. Also [[LEO Customer360 VNG topology: co-located services use localhost, cross-box hops need explicit extra_ingress]] for the box layout.

## Related

- [[Two-phase RAG chatbot UX: fast retrieval first]]
- [[slow generation second]]
- [[Public unauthenticated CPU-bound LLM endpoint is a DoS foot-gun]]

---

**Resolution (2026-09-06):** resized the UAT docs box to **2 vCPU / 4 GB** (`s-general-2x4`, in-place terraform change — `0 destroy`, reboots). After reboot: total RAM 3921 MB, `/ask` completes in ~30 s with no OOM. Both surfaces now serve generated answers (public `/docs-ai/ask` and the frontend-admin `/ai/ask` proxy, ~29 s, under the 60 s proxy timeout). Still unauthenticated + no rate limit — a sustained `/ask` flood no longer OOM-restarts the box but still serializes on the semaphore (~30 s each), so add rate limiting for a real public launch.

**Rate limiting (done 2026-09-06):** per-IP sliding-window limit on `/ask` (default 10 req / 60 s), keyed on the rightmost X-Forwarded-For (the hop Caddy set; unspoofable past Caddy). No-XFF callers (the internal frontend-admin `/ai` proxy) are exempt. Over-limit → HTTP 429 + Retry-After. Deployed via CD; verified live (image `@sha256:14130cdd`, env `ASK_RATE_MAX/WINDOW` set, `/docs-ai/ask` 200). 429 path unit-verified rather than live-flooded.
