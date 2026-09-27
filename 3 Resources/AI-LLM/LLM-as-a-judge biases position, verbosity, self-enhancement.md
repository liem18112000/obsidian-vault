---
ai_hash: 3f8cd6fb94560754
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-03
entities:
- LLM-as-a-judge
- Position bias
- Verbosity bias
- Self-enhancement bias
- LLM
- model's output
- randomize option order
- evaluate both orders and require agreement
- cap/normalize length
- score against explicit rubric criteria
- different model family
- judge
- generator
- biases
- inflate scores
- eval
- Rubric grading
- single blob judgment
- named criteria
- scored
- chain-of-thought justification
- G-Eval
- DeepEval
- promptfoo `llm-rubric`
- verifier
- reliability
- 'Assured test generation: keep an LLM test only if it builds'
- passes
- and raises coverage
source: deep research 2026-09-03
status: seedling
tags:
- llm
- evaluation
- llm-as-judge
- bias
title: 'LLM-as-a-judge biases: position, verbosity, self-enhancement'
type: lesson
---

# LLM-as-a-judge biases: position, verbosity, self-enhancement

Using a strong LLM to score another model`s output (LLM-as-a-judge) is powerful but carries systematic biases you must design around:

- **Position bias** — the judge favors whichever answer is shown first. Mitigate: randomize option order (or evaluate both orders and require agreement).
- **Verbosity bias** — longer answers are scored higher regardless of quality. Mitigate: cap/normalize length; score against explicit rubric criteria.
- **Self-enhancement bias** — the judge prefers outputs from its own model family. Mitigate: use a **different model family** as judge than as generator.

**Why it matters:** these biases silently inflate scores and make an eval look trustworthy when it is not. Prefer **rubric grading** (named criteria, each scored, with chain-of-thought justification — e.g. G-Eval / DeepEval / promptfoo `llm-rubric`) over a single blob judgment, and treat the judge as a [[Assured test generation keep an LLM test only if it builds, passes, and raises coverage|verifier]] whose own reliability must be validated.

## Related

- [[Assured test generation: keep an LLM test only if it builds]]
- [[passes]]
- [[and raises coverage]]

%% ai-graph-start %%

**Related notes:**
- [[Assured test generation keep an LLM test only if it builds, passes, and raises coverage]]
- [[AI self-critique loop - a post-generation critic pass rates the artifact and feeds the next run]]
- [[LLM query enrichment for a substring-OR matcher must contract, not expand, the token set]]
- [[A deterministic scorer is a negative case for LLM-agent-ification — reuse ADK via custom EvalMetric, not LlmAgent]]
- [[External LLM output is a lead generator, not a source of truth]]

**Relations:**
- LLM-as-a-judge — *has bias* — Position bias
- LLM-as-a-judge — *has bias* — Verbosity bias
- LLM-as-a-judge — *has bias* — Self-enhancement bias
- LLM-as-a-judge — *uses* — LLM
- LLM — *scores* — model's output
- Position bias — *mitigate with* — randomize option order
- Position bias — *mitigate with* — evaluate both orders and require agreement
- Verbosity bias — *mitigate with* — cap/normalize length
- Verbosity bias — *mitigate with* — score against explicit rubric criteria
- Self-enhancement bias — *mitigate with* — different model family
- different model family — *as* — judge
- different model family — *than as* — generator
- biases — *can* — inflate scores
- biases — *can make* — eval
- Rubric grading — *is preferred over* — single blob judgment
- Rubric grading — *includes* — named criteria
- Rubric grading — *includes* — scored
- Rubric grading — *includes* — chain-of-thought justification
- G-Eval — *is example of* — Rubric grading
- DeepEval — *is example of* — Rubric grading
- promptfoo `llm-rubric` — *is example of* — Rubric grading
- LLM-as-a-judge — *can be treated as* — verifier
- verifier — *has* — reliability
- reliability — *must be* — validated
- Assured test generation: keep an LLM test only if it builds — *is related to* — LLM-as-a-judge
- passes — *is related to* — LLM-as-a-judge
- and raises coverage — *is related to* — LLM-as-a-judge

%% ai-graph-end %%