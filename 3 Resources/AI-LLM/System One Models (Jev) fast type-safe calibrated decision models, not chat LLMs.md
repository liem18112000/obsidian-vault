---
title: "System One Models (Jev): fast type-safe calibrated decision models, not chat LLMs"
created: 2026-09-23
type: term
status: seedling
source: "typesafe.ai blog 2026-09"
tags: [ai, llm, typesafe, jev, structured-output, calibration]
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
