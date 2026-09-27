---
ai_hash: 806b2b80c6603a25
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-03
entities: []
source: research session 2026-09-03
status: seedling
tags:
- adk
- gcp
- agent-engine
- cloud-run
- serverless
- compute
title: ADK serverless agent tier is CPU-only; LLM compute is offloaded to a managed
  model API
type: lesson
---

# ADK serverless agent tier is CPU-only; LLM compute is offloaded to a managed model API

When you deploy a Google ADK (Agent Development Kit) agent to a **serverless** target, the agent process is a lightweight **CPU-only orchestration workload** — it runs the Python reasoning loop, tool calls, and session state. The heavy "AI compute" (the LLM) is **not on that machine**: it is a remote call to a managed model API (Gemini / Vertex AI) that runs on Google's own GPU/TPU fleet you never provision.

**Consequence:** for a normal "agent calls Gemini" pattern you want **CPU boxes and no GPU/TPU**. GPU/TPU on the agent tier only matters if you *co-locate a model* with the agent (self-hosted OSS LLM, embedding/reranker, Whisper, etc.). This is why Vertex AI Agent Engine (the purpose-built ADK runtime) exposes **only CPU + memory knobs** — the absence of a GPU setting is the design tell.

Applies to the `test-agent` project too: it calls Vertex/Gemini remotely, so Cloud Run CPU instances are correct and the GPU/TPU question is moot.

## Related

- [[ADK deploy targets compute matrix Agent Engine vs Cloud Run vs GKE (CPUGPUTPU)|ADK deploy targets compute matrix: Agent Engine vs Cloud Run vs GKE (CPU/GPU/TPU)]]

%% ai-graph-start %%

**Related notes:**
- [[ADK deploy targets compute matrix Agent Engine vs Cloud Run vs GKE (CPUGPUTPU)]]
- [[How ADK agents are deployed adk deploy cloud_run agent_engine gke + get_fast_api_app]]
- [[Adopt Google ADK only when the LLM drives the tool loop; else stay a2a-sdk-direct]]
- [[Vertex AI Agent Engine Memory Bank is per-user chat memory, not a domain knowledge base]]
- [[When agent-ifying LLM calls, preserve the per-call latency budget by gating non-essential generators behind a flag]]

%% ai-graph-end %%