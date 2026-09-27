---
ai_hash: ba78efdb567e068c
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
- GET /health
- POST /search
- GEN_CTX
- GEN_MAX_TOKENS
- RERANK_ENABLED
- 2 vCPU / 4 GB
- terraform
- proxy timeout
- 'Two-phase RAG chatbot UX: fast retrieval first, slow generation second'
- Public unauthenticated CPU-bound LLM endpoint is a DoS foot-gun
- 'LEO Customer360 VNG topology: co-located services use localhost, cross-box hops
  need explicit extra_ingress'
- Rate limiting
- X-Forwarded-For
- Caddy
- HTTP 429
- Retry-After
- CD
- ASK_RATE_MAX
- ASK_RATE_WINDOW
- s-general-2x4
- public /docs-ai/ask
- frontend-admin /ai/ask
- container
- slow client
- fronting proxy
- curl 52
- Empty reply from server
- browser chatbot proxy
- 'Two-phase RAG chatbot UX: fast retrieval first'
- slow generation second
- RAG service
- process
- generated answers
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

Applies to [[Two-phase RAG chatbot UX: fast retrieval first, slow generation second]] (why /search-first still delivers value when /ask is degraded) and [[Public unauthenticated CPU-bound LLM endpoint is a DoS foot-gun]]. Also [[LEO Customer360 VNG topology: co-located services use localhost, cross-box hops need explicit extra_ingress]] for the box layout.

## Related

- [[Two-phase RAG chatbot UX: fast retrieval first]]
- [[slow generation second]]
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
- [[Build a RAG index as an explicit deploy step, run a serve-only container]]

**Relations:**
- docs-vector-search — *is_a* — RAG service
- docs-vector-search — *runs_on* — UAT box
- UAT box — *has_specs* — 1 vCPU / 2 GB
- docs-vector-search — *OOM-kills* — uvicorn
- uvicorn — *is_a* — process
- uvicorn — *killed_during* — POST /ask
- POST /ask — *causes* — OOM-kills
- e5 embed — *is_component_of* — docs-vector-search
- bge reranker — *is_component_of* — docs-vector-search
- Qwen2.5-0.5B — *is_component_of* — docs-vector-search
- docs-vector-search — *has_model_resident_memory* — ~1.4 GB
- Qwen2.5-0.5B — *uses* — llama.cpp KV cache
- llama.cpp KV cache — *pushes_past* — RAM
- llama.cpp KV cache — *pushes_past* — swapfile
- kernel — *OOM-kills* — process
- container — *auto-restarts* — uvicorn
- container — *has_restart_policy* — unless-stopped
- slow client — *sees* — curl 52
- curl 52 — *means* — Empty reply from server
- fronting proxy — *returns* — 502
- GET /health — *is* — cheap
- POST /search — *is* — cheap
- POST /ask — *OOMs* — true
- GEN_CTX — *configures* — llama.cpp KV cache
- GEN_MAX_TOKENS — *configures* — llama.cpp KV cache
- Lower generation memory — *mitigates* — OOMs
- GEN_CTX — *reduced_from* — 4096
- GEN_CTX — *reduced_to* — 2048
- GEN_MAX_TOKENS — *reduced_from* — 512
- GEN_MAX_TOKENS — *reduced_to* — 256
- RERANK_ENABLED=false — *frees* — ~300 MB
- Resize the box to 2 vCPU / 4 GB — *is_a* — real fix
- Resize the box to 2 vCPU / 4 GB — *uses* — terraform
- docs-vector-search — *applies_to* — Two-phase RAG chatbot UX: fast retrieval first, slow generation second
- docs-vector-search — *applies_to* — Public unauthenticated CPU-bound LLM endpoint is a DoS foot-gun
- docs-vector-search — *applies_to* — LEO Customer360 VNG topology: co-located services use localhost, cross-box hops need explicit extra_ingress
- UAT box — *resized_to* — 2 vCPU / 4 GB
- UAT box — *has_flavor* — s-general-2x4
- POST /ask — *completes_in* — ~30 s
- POST /ask — *no_longer_causes* — OOM
- public /docs-ai/ask — *serves* — generated answers
- frontend-admin /ai/ask — *serves* — generated answers
- frontend-admin /ai/ask — *is_a* — proxy
- browser chatbot proxy — *has_timeout* — 60s
- browser chatbot proxy — *timeout_is* — too short
- POST /ask — *completes_under* — proxy timeout
- Rate limiting — *applied_to* — POST /ask
- Rate limiting — *is_type* — per-IP sliding-window limit
- Rate limiting — *keyed_on* — X-Forwarded-For
- Caddy — *sets* — X-Forwarded-For
- Over-limit — *returns* — HTTP 429
- Over-limit — *returns* — Retry-After
- Rate limiting — *deployed_via* — CD
- ASK_RATE_MAX — *configures* — Rate limiting
- ASK_RATE_WINDOW — *configures* — Rate limiting
- Two-phase RAG chatbot UX: fast retrieval first — *is_part_of* — Two-phase RAG chatbot UX: fast retrieval first, slow generation second
- slow generation second — *is_part_of* — Two-phase RAG chatbot UX: fast retrieval first, slow generation second

%% ai-graph-end %%