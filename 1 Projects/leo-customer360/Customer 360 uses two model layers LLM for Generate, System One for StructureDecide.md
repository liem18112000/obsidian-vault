---
ai_hash: 769ef3b92d13ac0c
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-23
entities:
- Customer 360
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
- structured_decision
- agent_code
- thin non-LiteLLM adapter
- identity gray-zone adjudication
- real-time next-best-action/personalization
- event routing
- PII/data-quality classification
- cold-start scoring
- POC
- ECE/reliability
- latency p50/p95
- cost/1k
- LLM path
- docs/research-papers/system-one-models-jev-in-customer360.md
- one-model-across-customer360.md
- System One Models (Jev) fast type-safe calibrated decision models, not chat LLMs
- decision model
- typed function call
- Jev job
- one model, many jobs thesis
- primitives
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
- [[TypeSafe AI's Jev - A System One Model for Fast, Structured Decisions]]
- [[System One Models (Jev) fast type-safe calibrated decision models, not chat LLMs]]
- [[Integrating Jev into Test-Agent-V2 for Enhanced Decision-Making]]
- [[Understanding JEV - Mechanism, Primitives, and Calibration in Decision Making]]
- [[customer360 AI campaign lifecycle agent plans, api persists draft, email_engine renders at send]]

**Relations:**
- Customer 360 — *uses* — LLM
- Customer 360 — *uses* — System One
- LLM — *handles* — Generate
- System One — *handles* — Structure / Decide
- Generate — *uses* — gpt-5.6
- gpt-5.6 — *accessed via* — LiteLLM
- LiteLLM — *runs in* — customer360-agent
- Structure / Decide — *uses* — Jev
- Jev — *is a type of* — System One
- System One — *is a type of* — decision model
- Embed & match — *uses* — pgvector
- customer360.cdp_ai_agents — *has field* — model_type
- model_type — *can be* — structured_decision
- customer360.cdp_ai_agents — *has field* — agent_code
- Jev job — *is a* — agent_code row
- System One call — *is a* — typed function call
- thin non-LiteLLM adapter — *handles* — System One call
- System One — *best fits* — identity gray-zone adjudication
- System One — *best fits* — real-time next-best-action/personalization
- System One — *best fits* — event routing
- System One — *best fits* — PII/data-quality classification
- System One — *best fits* — cold-start scoring
- Adoption — *gated on* — POC
- POC — *measures* — ECE/reliability
- POC — *measures* — latency p50/p95
- POC — *measures* — cost/1k
- POC — *compares performance to* — LLM path
- docs/research-papers/system-one-models-jev-in-customer360.md — *is companion to* — one-model-across-customer360.md
- System One Models (Jev) fast type-safe calibrated decision models, not chat LLMs — *describes* — System One
- System One Models (Jev) fast type-safe calibrated decision models, not chat LLMs — *describes* — Jev
- one model, many jobs thesis — *names* — primitives
- primitives — *consist of* — Generate
- primitives — *consist of* — Structure / Decide
- primitives — *consist of* — Embed & match
- one model, many jobs thesis — *proposes LLM for* — Generate
- one model, many jobs thesis — *proposes LLM for* — Structure / Decide
- one model, many jobs thesis — *proposes LLM for* — Embed & match

%% ai-graph-end %%