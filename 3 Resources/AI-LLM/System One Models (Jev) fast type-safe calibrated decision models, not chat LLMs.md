---
ai_hash: 6dc9cf9668d07716
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-23
entities:
- System One Model
- Jev
- TypeSafe.ai
- decisions software consumes directly
- unstructured state
- typed probabilistic decisions
- chat LLMs
- System 2
- Parallel sampling
- Sequential token generation
- typed value
- calibrated confidence
- schema match
- RLCD
- honest probabilities
- accuracy
- latency
- cost
- output tokens
- classification
- routing
- scoring
- extraction
- branching
- rerank
- generation
- calibration
- ECE
- reliability
- typesafe.ai/blog/introducing-system-one-models-and-jev
- model class
- vendor claims
- single blog source
- prose
source: typesafe.ai blog 2026-09
status: seedling
tags:
- ai
- llm
- typesafe
- jev
- structured-output
- calibration
title: 'System One Models (Jev): fast type-safe calibrated decision models, not chat
  LLMs'
type: term
---

# System One Models (Jev): fast type-safe calibrated decision models, not chat LLMs

A **System One Model** is a model class from TypeSafe.ai built for fast, automatic *decisions software consumes directly* — "unstructured state in, typed probabilistic decisions out" — as opposed to chat LLMs ("System 2", slow, prose). **Jev** is their first public one.

Key differences from an LLM (all vendor claims, **unverified** — single blog source):
- **Parallel sampling**, not sequential token generation: emits all decision values at once.
- Output is a **typed value + calibrated confidence** (schema match "mathematically guaranteed"), not a string you parse/validate.
- Trained via **RLCD** (Reinforcement Learning for Calibrated Decisions) — optimizes honest probabilities, so higher confidence => higher accuracy.
- ~**70-500ms** end-to-end (vs 3-329s for frontier LLMs); ~$0.042/M input, **output tokens free**.
- Good at: classification, routing, scoring, extraction, branching, rerank. NOT for generation (copy, narrative, SQL).

The load-bearing claim is **calibration**; it is domain-specific and must be re-measured (ECE/reliability) on your own data before any confidence gate trusts it.

Source: typesafe.ai/blog/introducing-system-one-models-and-jev

%% ai-graph-start %%

**Related notes:**
- [[Customer 360 uses two model layers LLM for Generate, System One for StructureDecide]]
- [[TypeSafe SDK Python system_one usage (v0.7.1)]]
- [[Calibrate a cheap-model to LLM cascade threshold using the LLM judge as oracle]]
- [[TypeSafe SDK response shape gotcha - cached-property accessors and Noul has no confidence]]
- [[laya is JEV's local in-process decision-engine twin]]

**Relations:**
- System One Model — *is a* — model class
- System One Model — *from* — TypeSafe.ai
- System One Model — *built for* — decisions software consumes directly
- System One Model — *takes as input* — unstructured state
- System One Model — *produces as output* — typed probabilistic decisions
- System One Model — *contrasts with* — chat LLMs
- System One Model — *contrasts with* — System 2
- Jev — *is a* — System One Model
- Jev — *is the first public* — System One Model
- chat LLMs — *are also known as* — System 2
- System One Model — *uses* — Parallel sampling
- chat LLMs — *use* — Sequential token generation
- System One Model — *output is a* — typed value
- System One Model — *output includes* — calibrated confidence
- System One Model — *guarantees* — schema match
- schema match — *is* — mathematically guaranteed
- System One Model — *trained via* — RLCD
- RLCD — *optimizes* — honest probabilities
- higher confidence — *implies* — higher accuracy
- System One Model — *has* — latency
- System One Model — *has* — cost
- System One Model — *has free* — output tokens
- System One Model — *is good at* — classification
- System One Model — *is good at* — routing
- System One Model — *is good at* — scoring
- System One Model — *is good at* — extraction
- System One Model — *is good at* — branching
- System One Model — *is good at* — rerank
- System One Model — *is NOT for* — generation
- calibration — *is a* — load-bearing claim
- calibration — *is* — domain-specific
- calibration — *must be re-measured on* — own data
- calibration — *measured by* — ECE
- calibration — *measured by* — reliability
- System One Model — *source is* — typesafe.ai/blog/introducing-system-one-models-and-jev
- System One Model — *has* — vendor claims
- vendor claims — *are* — unverified
- vendor claims — *from* — single blog source
- chat LLMs — *produce* — prose

%% ai-graph-end %%