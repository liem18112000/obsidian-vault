---
title: "Customer 360 uses two model layers: LLM for Generate, System One for Structure/Decide"
created: 2026-09-23
type: argument
status: seedling
source: "docs/research-papers/system-one-models-jev-in-customer360.md"
tags: [leo-customer360, cdp, ai-architecture, cdp_ai_agents, jev]
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

- [[System One Models (Jev) fast type-safe calibrated decision models]]
- [[not chat LLMs]]
