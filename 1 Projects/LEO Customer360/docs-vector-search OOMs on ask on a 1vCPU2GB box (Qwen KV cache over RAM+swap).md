---
ai_hash: 36c29ee63b713278
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-06
entities:
- docs-vector-search
- UAT box
- 1 vCPU / 2 GB
- uvicorn
- POST /ask
- e5 embed
- bge reranker
- Qwen2.5-0.5B
- llama.cpp KV cache
- RAM
- swapfile
- kernel
- OOM-kills
- curl 52 "Empty reply from server"
- fronting proxy
- 502s
- GET /health
- POST /search
- GEN_CTX
- GEN_MAX_TOKENS
- RERANK_ENABLED
- terraform
- 2 vCPU / 4 GB
- consumer's proxy timeout
- Two-phase RAG chatbot UX fast retrieval first, slow generation second
- Public unauthenticated CPU-bound LLM endpoint is a DoS foot-gun
- LEO Customer360 VNG topology co-located services use localhost, cross-box hops need
  explicit extra_ingress
- s-general-2x4
- public /docs-ai/ask
- frontend-admin /ai/ask proxy
- rate limiting
- X-Forwarded-For
- Caddy
- HTTP 429
- Retry-After
- CD
- ASK_RATE_MAX
- ASK_RATE_WINDOW
source: session 2026-09-06 docs-chatbot UAT deploy
status: seedling
tags:
- leo-customer360
- rag
- llm
- oom
- memory
- llama-cpp
- ops
title: docs-vector-search OOMs on /ask on a 1vCPU/2GB box (Qwen KV cache over RAM+swap)
type: observation
---

# docs-vector-search OOMs on /ask on a 1vCPU/2GB box (Qwen KV cache over RAM+swap)

The docs-vector-search RAG service on its UAT box (1 vCPU / **2 GB**) OOM-kills uvicorn during POST /ask generation. e5 embed + bge reranker + Qwen2.5-0.5B are ~1.4 GB resident; the llama.cpp KV cache during generation (GEN_CTX=4096, GEN_MAX_TOKENS=512) pushes past RAM + the 2 GB swapfile, and the kernel OOM-kills the process (seen: `Out of memory: Killed process ... task=uvicorn` + `double free or corruption`). The container auto-restarts (restart unless-stopped), so a slow client sees curl 52 "Empty reply from server" after minutes, or the fronting proxy 502s at its timeout.

Symptoms map: GET /health and POST /search are cheap and always work; POST /ask (generation) is the one that OOMs. Fast /search + slow/failing /ask is the tell.

Mitigations (cheap -> robust):
1. Lower generation memory: GEN_CTX 4096->2048 and GEN_MAX_TOKENS 512->256 (smaller KV cache, also faster). Shorter answers.
2. RERANK_ENABLED=false frees ~300 MB (README's first suggested lever); /search still works via vector similarity, ranking slightly worse.
3. Resize the box to 2 vCPU / 4 GB (terraform flavor change + reboot + cost) — the real fix.
Also bump the consumer's proxy timeout: a browser chatbot proxy at 60s is too short even for a healthy generation on this box.

Applies to [[Two-phase RAG chatbot UX fast retrieval first, slow generation second|Two-phase RAG chatbot UX: fast retrieval first, slow generation second]] (why /search-first still delivers value when /ask is degraded) and [[Public unauthenticated CPU-bound LLM endpoint is a DoS foot-gun]]. Also [[LEO Customer360 VNG topology co-located services use localhost, cross-box hops need explicit extra_ingress|LEO Customer360 VNG topology: co-located services use localhost, cross-box hops need explicit extra_ingress]] for the box layout.

## Related

- [[Two-phase RAG chatbot UX fast retrieval first, slow generation second]]
- [[Public unauthenticated CPU-bound LLM endpoint is a DoS foot-gun]]

---

**Resolution (2026-09-06):** resized the UAT docs box to **2 vCPU / 4 GB** (`s-general-2x4`, in-place terraform change — `0 destroy`, reboots). After reboot: total RAM 3921 MB, `/ask` completes in ~30 s with no OOM. Both surfaces now serve generated answers (public `/docs-ai/ask` and the frontend-admin `/ai/ask` proxy, ~29 s, under the 60 s proxy timeout). Still unauthenticated + no rate limit — a sustained `/ask` flood no longer OOM-restarts the box but still serializes on the semaphore (~30 s each), so add rate limiting for a real public launch.

**Rate limiting (done 2026-09-06):** per-IP sliding-window limit on `/ask` (default 10 req / 60 s), keyed on the rightmost X-Forwarded-For (the hop Caddy set; unspoofable past Caddy). No-XFF callers (the internal frontend-admin `/ai` proxy) are exempt. Over-limit → HTTP 429 + Retry-After. Deployed via CD; verified live (image `@sha256:14130cdd`, env `ASK_RATE_MAX/WINDOW` set, `/docs-ai/ask` 200). 429 path unit-verified rather than live-flooded.

%% ai-graph-start %%

**Related notes:**
- [[Local RAG stack for a 1 vCPU 2 GB box e5 + bge-reranker + Qwen 0.5B]]
- [[Widen the reranker candidate pool (RETRIEVE_TOP_N) or short queries retrieve junk]]
- [[LLM-as-reranker JSON truncation budget max_tokens for pretty-printed output, not just element count]]
- [[Public unauthenticated CPU-bound LLM endpoint is a DoS foot-gun]]
- [[Two-phase RAG chatbot UX fast retrieval first, slow generation second]]

**Relations:**
- docs-vector-search — *runs on* — UAT box
- UAT box — *has specification* — 1 vCPU / 2 GB
- docs-vector-search — *uses* — uvicorn
- uvicorn — *OOM-kills during* — POST /ask
- POST /ask — *triggers* — OOM-kills
- docs-vector-search — *includes* — e5 embed
- docs-vector-search — *includes* — bge reranker
- docs-vector-search — *includes* — Qwen2.5-0.5B
- Qwen2.5-0.5B — *uses* — llama.cpp KV cache
- llama.cpp KV cache — *exceeds* — RAM
- llama.cpp KV cache — *exceeds* — swapfile
- kernel — *performs* — OOM-kills
- OOM-kills — *causes* — curl 52 "Empty reply from server"
- OOM-kills — *causes* — fronting proxy 502s
- POST /ask — *is problematic* — OOMs
- GET /health — *is* — cheap
- POST /search — *is* — cheap
- GEN_CTX — *reduces* — generation memory
- GEN_MAX_TOKENS — *reduces* — generation memory
- RERANK_ENABLED=false — *frees* — ~300 MB
- RERANK_ENABLED=false — *degrades* — ranking
- UAT box — *resized to* — 2 vCPU / 4 GB
- terraform — *used for* — resize
- consumer's proxy timeout — *affects* — generation
- docs-vector-search — *relates to* — Two-phase RAG chatbot UX fast retrieval first, slow generation second
- docs-vector-search — *relates to* — Public unauthenticated CPU-bound LLM endpoint is a DoS foot-gun
- docs-vector-search — *relates to* — LEO Customer360 VNG topology co-located services use localhost, cross-box hops need explicit extra_ingress
- 2 vCPU / 4 GB — *is flavor* — s-general-2x4
- resize — *resolves* — OOM-kills
- public /docs-ai/ask — *is a* — service
- frontend-admin /ai/ask proxy — *is a* — service
- rate limiting — *applied to* — public /docs-ai/ask
- rate limiting — *uses* — X-Forwarded-For
- Caddy — *sets* — X-Forwarded-For
- rate limiting — *causes* — HTTP 429
- rate limiting — *causes* — Retry-After
- rate limiting — *deployed via* — CD
- ASK_RATE_MAX — *configures* — rate limiting
- ASK_RATE_WINDOW — *configures* — rate limiting
- UAT box — *has* — RAM
- UAT box — *has* — swapfile
- POST /ask — *is served by* — public /docs-ai/ask
- POST /ask — *is served by* — frontend-admin /ai/ask proxy

%% ai-graph-end %%