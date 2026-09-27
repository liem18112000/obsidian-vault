---
ai_hash: a1014efc38dbebc7
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-03
entities:
- LLM-as-a-judge
- Position bias
- Verbosity bias
- Self-enhancement bias
- LLM
- Rubric grading
- G-Eval
- DeepEval
- promptfoo `llm-rubric`
- verifier
- randomize option order
- evaluate both orders and require agreement
- cap/normalize length
- score against explicit rubric criteria
- different model family as judge than as generator
- single blob judgment
- named criteria
- each scored
- chain-of-thought justification
- reliability validation
- 'Assured test generation: keep an LLM test only if it builds, passes, and raises
  coverage'
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
- [[AI self-critique loop - a post-generation critic pass rates the artifact and feeds the next run]]
- [[Assured test generation keep an LLM test only if it builds, passes, and raises coverage]]
- [[LLM query enrichment for a substring-OR matcher must contract, not expand, the token set]]
- [[A deterministic scorer is a negative case for LLM-agent-ification — reuse ADK via custom EvalMetric, not LlmAgent]]
- [[External LLM output is a lead generator, not a source of truth]]

**Relations:**
- LLM-as-a-judge — *exhibits bias* — Position bias
- LLM-as-a-judge — *exhibits bias* — Verbosity bias
- LLM-as-a-judge — *exhibits bias* — Self-enhancement bias
- Position bias — *mitigated by* — randomize option order
- Position bias — *mitigated by* — evaluate both orders and require agreement
- Verbosity bias — *mitigated by* — cap/normalize length
- Verbosity bias — *mitigated by* — score against explicit rubric criteria
- Self-enhancement bias — *mitigated by* — different model family as judge than as generator
- LLM-as-a-judge — *uses* — LLM
- Rubric grading — *is preferred over* — single blob judgment
- Rubric grading — *includes* — named criteria
- Rubric grading — *includes* — each scored
- Rubric grading — *includes* — chain-of-thought justification
- G-Eval — *is example of* — Rubric grading
- DeepEval — *is example of* — Rubric grading
- promptfoo `llm-rubric` — *is example of* — Rubric grading
- LLM-as-a-judge — *should be treated as* — verifier
- verifier — *requires* — reliability validation
- LLM-as-a-judge — *related to* — Assured test generation: keep an LLM test only if it builds, passes, and raises coverage

%% ai-graph-end %%