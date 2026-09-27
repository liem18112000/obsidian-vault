---
ai_hash: c2d286d58e01ff87
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-27
entities: []
source: 'Confluence: Test oracle - what a scenario asserts (2026-09-15)'
status: seedling
tags:
- testing
- test-oracle
- metrics
- ai-agents
- test-agent-v2
- static-analysis
title: Oracle strength can be graded statically from the expected-result text
type: howto
---

# Oracle strength can be graded statically from the expected-result text

You can score how good an assertion is **without executing anything**, by classifying the `expected` text into three tiers. The Test-Plan-Definition agent does exactly this (`metrics/oracle.py`), and it is deterministic:

| Tier | Weight | Detection rule |
|---|---|---|
| **strong** | 1.0 | an enum-shaped token matching `[A-Z][A-Z0-9_]{3,}`, **or** any substring of the plan's own `pass_criteria` |
| **weak** | 0.0 | contains a weak word: `2xx` `3xx` `4xx` `5xx` `status` `accepted` `succeeds` `is returned` `is processed` `is ready` `unchanged` `without error` `is rejected` |
| **medium** | 0.5 | neither of the above |

The insight that makes it work: **weak assertions have a recognisable vocabulary.** Language about *acceptance* ("succeeds", "is processed", "without error") describes the transport, not the outcome. Language naming a *state* (`CREDIT_CARD_CHARGED_PENDING`) describes the outcome. A regex over that vocabulary gets you a usable signal for ~30 lines of code.

Why bother with a crude static proxy instead of measuring real fault detection: mutation testing is the rigorous answer but needs a working execution stage and is orders of magnitude more expensive. This runs at plan-authoring time, before any code exists, which is **when the feedback can still change the plan**. It carries `0.15` of the Test-Plan Score as a stand-in for fault detection.

Its limits, which matter if you copy it: it rewards enum-shaped tokens, so a plan can game it by naming constants without asserting anything meaningful, and it cannot see whether the asserted state is the *right* one. It is a floor on assertion quality, not a measure of correctness.

## Related

- [[Oracle strength, not coverage, decides whether a suite catches regressions]]

## Related

- [[Oracle strength]]
- [[not coverage]]
- [[decides whether a suite catches regressions]]

%% ai-graph-start %%

**Related notes:**
- [[Test oracle - what a scenario asserts, and why its strength decides everything]]
- [[Oracle strength, not coverage, decides whether a suite catches regressions]]
- [[A test oracle is what decides pass or fail, and without one a test is just a script]]
- [[Evaluating the Test-Plan-Definition Agent]]
- [[Assured test generation keep an LLM test only if it builds, passes, and raises coverage]]

%% ai-graph-end %%