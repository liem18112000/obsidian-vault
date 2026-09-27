---
ai_hash: 87d5c3261b1f2e62
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-27
entities: []
source: 'Confluence: Test oracle - what a scenario asserts (2026-09-15)'
status: seedling
tags:
- testing
- test-oracle
- bdd
- gherkin
- quality
- istqb
title: A test oracle is what decides pass or fail, and without one a test is just
  a script
type: term
---

# A test oracle is what decides pass or fail, and without one a test is just a script

A **test oracle** is the mechanism that decides whether a test **passed or failed** — the source of the *expected* result the actual result is compared against. In a Gherkin scenario it is the `Then` step:

```gherkin
Given a credit-only invoice for an individual customer
When  the invoice-run charge job executes
Then  the tracking row reaches CREDIT_CARD_CHARGED_PENDING   ← the oracle
```

The definition earns its keep through one consequence: **without an oracle a test can run but cannot judge — it is a script, not a test.** Plenty of "tests" are scripts: they exercise a path, produce output, and never assert anything that could distinguish correct from broken.

The reframing that follows is the useful part — **a test plan is a set of oracles.** Grading a plan therefore means grading *what it would assert*, not how many scenarios it contains. Scenario count measures effort; oracle quality measures fault detection, and the two are almost uncorrelated.

Standards grounding: ISTQB Foundation and ISO/IEC/IEEE 29119 define the term; Barr et al., *"The Oracle Problem in Software Testing: A Survey"* (IEEE TSE, 2015) gives the taxonomy.

## Related

- [[Oracle strength, not coverage, decides whether a suite catches regressions]]
- [[Metamorphic and differential testing solve the oracle problem]]

## Related

- [[Oracle strength]]
- [[not coverage]]
- [[decides whether a suite catches regressions]]
- [[Metamorphic and differential testing solve the oracle problem]]

%% ai-graph-start %%

**Related notes:**
- [[Oracle strength, not coverage, decides whether a suite catches regressions]]
- [[Test oracle - what a scenario asserts, and why its strength decides everything]]
- [[Oracle strength can be graded statically from the expected-result text]]
- [[Metamorphic and differential testing solve the oracle problem]]
- [[Evaluating the Test-Plan-Definition Agent]]

%% ai-graph-end %%