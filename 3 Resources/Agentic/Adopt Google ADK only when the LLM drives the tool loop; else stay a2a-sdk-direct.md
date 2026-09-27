---
ai_hash: bddc00730f907805
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-03
entities:
- Google ADK
- LLM
- tool loop
- a2a-sdk-direct
- agent
- adk-python
- deterministic, Python-driven pipeline
- test-agent
- KGA
- TPD
- a2a-sdk
- A2A protocol
- MCP
- Vertex
- Gemini
- Non-Gemini models
- Claude
- LiteLlm wrapper
- anthropic[vertex]
- direct Vertex control
- LLM-driven agent loop
- LlmAgent
- Sequential/Loop/Parallel workflow agents
- Runner/Sessions/Memory services
- to_a2a()
- A2A/MCP mesh
- a2a-sdk-direct agents
- client-side, LLM-driven interrogation / refine loop
- crawl pipelines
- plan pipelines
- test-agent/docs/adk-vs-current-stack.excalidraw
- current stack
- test-agent common shared engine
- Interrogation loop asks nothing
source: session 2026-09-03
status: seedling
tags:
- adk
- a2a
- agentic
- test-agent
- architecture
- decision
title: Adopt Google ADK only when the LLM drives the tool loop; else stay a2a-sdk-direct
type: lesson
---

# Adopt Google ADK only when the LLM drives the tool loop; else stay a2a-sdk-direct

The choice between Google ADK (adk-python) and building an agent directly on **a2a-sdk** is a question of the agent's **SHAPE, not the vendor**. Adopt ADK only for an agent where the **LLM drives the tool loop** (reason -> pick tool -> observe -> repeat). For a **deterministic, Python-driven pipeline** where the LLM is just one call inside a fixed flow (like the test-agent's KGA/TPD), stay on a2a-sdk directly.

**Why the framework rarely pays off for a pipeline agent**
- ADK is **built on the same foundation** you already use: a2a-sdk (A2A protocol) + MCP + Vertex. It **wraps** that base, it does not replace it — so a wholesale switch buys little.
- ADK is **Gemini-first**. Non-Gemini models (Claude) run only through a **LiteLlm wrapper** — an extra abstraction hop that hides the direct Vertex control you get from `anthropic[vertex]` (max_tokens, non-blocking calls, fallback).
- It **removes none of the real work**: the Atlassian crawl, ADF/HTML parsing, and gherkin rendering are all still yours to write.

**The one thing ADK genuinely adds**: an LLM-driven agent loop — `LlmAgent`, the `Sequential/Loop/Parallel` workflow agents, and Runner/Sessions/Memory services.

**Selective-adoption path** (the right way in, if/when needed): build just the LLM-driven agent as an `LlmAgent`, expose it with `to_a2a()`, and it slots into the existing A2A/MCP mesh alongside the a2a-sdk-direct agents — no rewrite of the others. In the test-agent, the natural landing spot is the client-side, LLM-driven **interrogation / refine loop**, not the crawl or plan pipelines.

Diagram: `test-agent/docs/adk-vs-current-stack.excalidraw`.

## Related
- [[test-agent common shared engine]]
- [[Interrogation loop asks nothing]]

## Related

- [[test-agent common shared engine]]
- [[Interrogation loop asks nothing]]

%% ai-graph-start %%

**Related notes:**
- [[test-agent-v2 KGA has no live LlmAgent — explore steps are the first ADK LlmAgent target]]
- [[ADK workflow agents orchestrate deterministically without an LLM-driven loop]]
- [[ADK canonical orchestration SequentialAgent, LlmAgent+AgentTool, or callbacks — not custom BaseAgent]]
- [[A deterministic scorer is a negative case for LLM-agent-ification — reuse ADK via custom EvalMetric, not LlmAgent]]
- [[test-agent-v2 TPD has five raw-Vertex generators — the ADK LlmAgent conversion targets]]

**Relations:**
- Google ADK — *is also known as* — adk-python
- Google ADK — *should be adopted when* — LLM drives tool loop
- a2a-sdk-direct — *should be used for* — deterministic, Python-driven pipeline
- LLM — *is one call in* — deterministic, Python-driven pipeline
- test-agent — *uses* — KGA
- test-agent — *uses* — TPD
- Google ADK — *is built on* — a2a-sdk
- Google ADK — *is built on* — A2A protocol
- Google ADK — *is built on* — MCP
- Google ADK — *is built on* — Vertex
- Google ADK — *wraps* — a2a-sdk
- Google ADK — *wraps* — A2A protocol
- Google ADK — *wraps* — MCP
- Google ADK — *wraps* — Vertex
- Google ADK — *is* — Gemini-first
- Non-Gemini models — *run through* — LiteLlm wrapper
- Claude — *is a type of* — Non-Gemini models
- LiteLlm wrapper — *hides* — direct Vertex control
- anthropic[vertex] — *provides* — direct Vertex control
- Google ADK — *adds* — LLM-driven agent loop
- LLM-driven agent loop — *includes* — LlmAgent
- LLM-driven agent loop — *includes* — Sequential/Loop/Parallel workflow agents
- LLM-driven agent loop — *includes* — Runner/Sessions/Memory services
- LlmAgent — *can be exposed with* — to_a2a()
- to_a2a() — *slots into* — A2A/MCP mesh
- A2A/MCP mesh — *works with* — a2a-sdk-direct agents
- test-agent — *has* — client-side, LLM-driven interrogation / refine loop
- client-side, LLM-driven interrogation / refine loop — *is a landing spot for* — LlmAgent
- test-agent — *has* — crawl pipelines
- test-agent — *has* — plan pipelines
- test-agent/docs/adk-vs-current-stack.excalidraw — *depicts comparison of* — Google ADK and current stack
- test-agent common shared engine — *is related to* — test-agent
- Interrogation loop asks nothing — *is related to* — client-side, LLM-driven interrogation / refine loop

%% ai-graph-end %%