---
title: "TypeSafe SDK response shape gotcha - cached-property accessors and Noul has no confidence"
created: 2026-09-22
type: lesson
status: seedling
source: "session 2026-09-22, live SDK introspection"
tags: [typesafe, sdk, python, gotcha, jev]
---

# TypeSafe SDK response shape gotcha - cached-property accessors and Noul has no confidence

Two things about `SystemOneResponse` (typesafe-sdk v0.7.1) contradict a naive reading of the docs — both bit-worthy when mapping results into your own types.

**1. Accessors are cached-properties over a flat `answers` dict.** The raw model fields are only `model`, `usage`, `answers` — where `answers` is `dict[qid -> NoulAnswer|ChoiceAnswer|ScoreAnswer]` (discriminated on `type`). But the class *also* exposes `choices`, `nouls`, `scores` as `cached_property` views that filter `answers` by type. So `result.choices[qid].choice`, `result.nouls[qid].noul`, `result.scores[qid].score` all work — the docs example is right, just not because those are stored fields.

**2. Not every answer carries confidence/probabilities.** `ChoiceAnswer` = `{choice, confidence, probabilities}`; `ScoreAnswer` = `{score, confidence, legend, probabilities}`; **`NoulAnswer` = `{noul}` only** — no `confidence`, no `probabilities`. So any code that reads `.confidence` off a noul must default it. `.score` is a genuine **continuous 0-1 float** (e.g. 0.635), *not* an ordinal rank/index; `probabilities` is a per-option/per-level dict summing ~1 (score levels keyed by index "0"/"1"/"2").

Consequence in `test-agent-v2` (`common/adk/providers/jev.py`, the `JevProvider` `DecisionProvider`): confidence is probed defensively and falls back to `TYPESAFE_DEFAULT_CONFIDENCE` (1.0) exactly for the noul case; the score passes straight through as a 0-1 float. Confidence feeds the cascade gate `TPD_DECISION_CONF_MIN` (default 0.8) — a live score came back at conf=0.56, correctly below-bar → falls back to the LLM judge. See [[TypeSafe SDK Python system_one usage (v0.7.1)]].

## Related

- [[TypeSafe SDK Python system_one usage (v0.7.1)]]
