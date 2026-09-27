---
title: "Oracle strength, not coverage, decides whether a suite catches regressions"
created: 2026-09-27
type: argument
status: seedling
source: "Confluence: Test oracle - what a scenario asserts (2026-09-15)"
tags: [testing, test-oracle, quality, coverage, bdd, test-design]
---

# Oracle strength, not coverage, decides whether a suite catches regressions

Two scenarios can execute **the identical code path** and one catches a regression while the other never will. The difference is not coverage — it is what the `Then` step asserts.

| | Oracle | When the charge silently fails |
|---|---|---|
| **Weak** | `Then the response status is 2xx` | still 2xx → **test passes** → bug ships |
| **Strong** | `Then the tracking row reaches CREDIT_CARD_CHARGED_PENDING` | state never reached → **test fails** → bug caught |

This is why a suite full of `assert 200` looks complete, runs green, and catches almost nothing. Line coverage counts the first column and the second column is where the value is. **Coverage tells you the code ran; the oracle tells you whether running it was correct.**

The practical test for any assertion: **"what regression would make this fail?"** If the honest answer is "a crash or a 500", the oracle is weak. Strong oracles name a **concrete, resolvable end state** — an enum, a status token, a specific value — because a silent failure still produces a 2xx but cannot produce the right end state.

This was the measured gap when an agent-authored test plan was compared against a human-authored one: comparable scenario counts, comparable code paths, far weaker assertions. Hence *oracle strength* being scored explicitly rather than assumed — it stands in for fault-detection when no execution stage exists yet.

## Related

- [[A test oracle is what decides pass or fail, and without one a test is just a script]]
- [[Oracle strength can be graded statically from the expected-result text]]

## Related

- [[A test oracle is what decides pass or fail, and without one a test is just a script]]
- [[Oracle strength can be graded statically from the expected-result text]]
