---
ai_hash: a605e8bbecf7de6d
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-23
entities:
- Customer 360 CDP
- LLM
- System One
- Generate
- Structure / Decide
- Embed & match
- gpt-5.6
- LiteLLM
- customer360-agent
- Jev
- pgvector
- customer360.cdp_ai_agents
- thin non-LiteLLM adapter
- identity gray-zone adjudication
- real-time next-best-action/personalization
- event routing
- PII/data-quality classification
- cold-start scoring
- Proof of Concept
- ECE/reliability
- latency p50/p95
- cost/1k
- cdp_profile_merge_history
- review queue
- docs/research-papers/system-one-models-jev-in-customer360.md
- one-model-across-customer360.md
- System One Models (Jev)
- chat LLMs
- model_type = structured_decision
- agent_code row
- System One call
- typed function call
- chat-completion
source: docs/research-papers/system-one-models-jev-in-customer360.md
status: seedling
tags:
- leo-customer360
- cdp
- ai-architecture
- cdp_ai_agents
- jev
title: 'Customer 360 uses two model layers: LLM for Generate, System One for Structure/Decide'
type: argument
---

# Customer 360 uses two model layers: LLM for Generate, System One for Structure/Decide

Design decision for the LEO Customer 360 CDP: run **two** horizontal model layers, not one. The companion "one model, many jobs" thesis names three primitives — Generate, Structure, Embed & match — but points a single chat LLM at all three. A System One decision model completes it by taking the middle seat:

- **Generate** (copy, narrative, reason codes, text-to-SQL) -> LLM `gpt-5.6` (via LiteLLM in `customer360-agent`).
- **Structure / Decide** (classification, routing, scoring, matching, rerank) -> System One model (Jev). See [[System One Models (Jev) fast type-safe calibrated decision models, not chat LLMs]].
- **Embed & match** -> `pgvector` (unchanged).

Integration stays "just a table": add `model_type = structured_decision` to the `customer360.cdp_ai_agents` CHECK; each Jev job is a new `agent_code` row. Needs a **thin non-LiteLLM adapter** (a System One call is a typed function call, not a chat-completion).

Best fits (why the LLM structurally cant): **identity gray-zone adjudication** (`{match, confidence}`, calibrated p drives the human-queue gate), **real-time next-best-action/personalization** (sub-second lets it run *in the request path*, not just nightly Dagster), **event routing**, **PII/data-quality classification**, and **cold-start scoring** before a calibrated numeric model is trained (then the registry row swaps `model_name`).

Adoption is **gated on a POC**, because every latency/cost/accuracy/calibration figure is a single-source vendor claim (**unverified**). Run it on identity adjudication (labels come free from `cdp_profile_merge_history` + the review queue): measure ECE/reliability, latency p50/p95, and cost/1k vs the LLM path. Adopt only if calibration holds AND latency+cost beat the LLM.

Report: `docs/research-papers/system-one-models-jev-in-customer360.md` (companion to `one-model-across-customer360.md`).

## Related

- [[System One Models (Jev) fast type-safe calibrated decision models, not chat LLMs]]

%% ai-graph-start %%

**Related notes:**
- [[System One Models (Jev) fast type-safe calibrated decision models, not chat LLMs]]
- [[Reasoning models reject temperature != 1; use litellm.drop_params for multi-model agents]]
- [[customer360 AI campaign lifecycle agent plans, api persists draft, email_engine renders at send]]
- [[LEO CDP schema migrations are ordered plain SQL, not dbmate or alembic]]
- [[Hermetic E2E test of an LLM agent mock only the SDK boundary, inject the prompt snapshot]]

**Relations:**
- Customer 360 CDP — *uses* — LLM
- Customer 360 CDP — *uses* — System One
- LLM — *handles* — Generate
- System One — *handles* — Structure / Decide
- Embed & match — *handled by* — pgvector
- Generate — *implemented by* — gpt-5.6
- gpt-5.6 — *accessed via* — LiteLLM
- LiteLLM — *used in* — customer360-agent
- Structure / Decide — *implemented by* — Jev
- Jev — *is a type of* — System One
- System One — *is a type of* — System One Models (Jev)
- System One Models (Jev) — *is not a* — chat LLMs
- customer360.cdp_ai_agents — *includes* — model_type = structured_decision
- Jev — *creates* — agent_code row
- System One — *requires* — thin non-LiteLLM adapter
- thin non-LiteLLM adapter — *processes* — System One call
- System One call — *is a* — typed function call
- System One call — *is not a* — chat-completion
- System One — *excels at* — identity gray-zone adjudication
- System One — *excels at* — real-time next-best-action/personalization
- System One — *excels at* — event routing
- System One — *excels at* — PII/data-quality classification
- System One — *excels at* — cold-start scoring
- LLM — *struggles with* — identity gray-zone adjudication
- LLM — *struggles with* — real-time next-best-action/personalization
- LLM — *struggles with* — event routing
- LLM — *struggles with* — PII/data-quality classification
- LLM — *struggles with* — cold-start scoring
- System One — *adoption gated by* — Proof of Concept
- Proof of Concept — *focuses on* — identity gray-zone adjudication
- identity gray-zone adjudication — *gets labels from* — cdp_profile_merge_history
- identity gray-zone adjudication — *gets labels from* — review queue
- Proof of Concept — *measures* — ECE/reliability
- Proof of Concept — *measures* — latency p50/p95
- Proof of Concept — *measures* — cost/1k
- Proof of Concept — *compares against* — LLM
- docs/research-papers/system-one-models-jev-in-customer360.md — *companion to* — one-model-across-customer360.md
- System One Models (Jev) — *related to* — System One
- chat LLMs — *related to* — LLM

%% ai-graph-end %%