---
ai_hash: ae5cfaccfdfbff08
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-08
entities: []
source: test-agent-v2 Agent Engine research, session 2026-09-08
status: seedling
tags:
- google-adk
- agent-engine
- memory-bank
- vertex-ai
- memory
title: Vertex AI Agent Engine Memory Bank is per-user chat memory, not a domain knowledge
  base
type: concept
---

# Vertex AI Agent Engine Memory Bank is per-user chat memory, not a domain knowledge base

Vertex AI **Agent Engine Memory Bank** (the managed memory that ships with "Agent Runtime") is **per-user conversational long-term memory**, not a general knowledge base. Gemini asynchronously extracts facts/preferences from a user's **Agent Engine Session** history, consolidates them (resolving contradictions), stores them **scoped by a `user_id`**, and serves them back by similarity search. Think "the user prefers API-level tests" — not "here is the crawled domain graph".

**Do not conflate it with your own retrieval store.** If your agent already has a domain knowledge base (e.g. a crawled link-graph + distilled notes + vector recall), Memory Bank does NOT replace it — the two answer different questions (per-user chat facts vs global domain knowledge) and should coexist. Migrating a domain KB into Memory Bank is a category error.

**ADK wiring:** `from google.adk.memory import VertexAiMemoryBankService; mem = VertexAiMemoryBankService(project, location, agent_engine_id)`; pass to `Runner(memory_service=mem)`. Write with `add_session_to_memory(session)` (triggers GenerateMemories via Gemini, async — not orchestrated by the Runner); read with `search_memory(app_name, user_id, query)` or the built-in `load_memory` / `preload_memory` tools on an LlmAgent. An ADK agent deployed to Agent Engine Runtime uses `VertexAiMemoryBankService` **by default**.

**Two gotchas:** (1) the memory-generation model is **Gemini, managed** — independent of your agent's model (Claude-via-LiteLlm still works for the agent; the extractor is Gemini regardless). (2) `VertexAiMemoryBankService` needs an **`agent_engine_id`** — i.e. a Memory Bank only exists in the context of a deployed Agent Engine instance (`projects/…/reasoningEngines/<ID>`); you can still call it from outside Agent Engine (e.g. from Cloud Run) as a **hybrid** to get the managed memory without moving your serving surface onto Agent Engine.

See [[Test an ADK LlmAgent(output_schema=) offline with a BaseLlm fake yielding canned JSON]].

## Related

- [[Test an ADK LlmAgent(output_schema=) offline with a BaseLlm fake yielding canned JSON]]

%% ai-graph-start %%

**Related notes:**
- [[ADK serverless agent tier is CPU-only; LLM compute is offloaded to a managed model API]]
- [[test-agent-v2 Cloud SQL Postgres holds app + ADK-session + A2A-task tables on one engine]]
- [[test-agent-v2 KGA has no live LlmAgent — explore steps are the first ADK LlmAgent target]]
- [[ADK DatabaseSessionService can subsume a separate A2A task store and state-file rehydration]]
- [[Testing-Agent GCS memory bank one bucket, memory root, five subfolders]]

%% ai-graph-end %%