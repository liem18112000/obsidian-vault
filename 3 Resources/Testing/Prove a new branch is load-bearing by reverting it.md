---
title: "Prove a new branch is load-bearing by reverting it"
created: 2026-09-25
type: howto
status: seedling
source: "session 2026-09-25"
tags: [testing, mutation-testing, verification]
---

# Prove a new branch is load-bearing by reverting it

A passing test does not prove your new code ran. To prove a branch is load-bearing: disable it, confirm the new test FAILS, then restore and confirm it passes.

Three seconds of work, and it is the only thing that distinguishes "my feature works" from "my test would pass without my feature".

```
# 1. neuter the new branch
if False and rigor:
    max_iters = min(rigor, MAX)
# 2. run the new tests -> they MUST fail
# 3. restore, run again -> they MUST pass
```

Why it matters more than it sounds: a test written alongside a feature tends to set up exactly the conditions the feature handles, then assert the outcome the feature produces. If the feature silently never executes — wrong predicate, unreachable branch, a default that already gave the expected answer — the assertion can still hold for the wrong reason.

I shipped a metric in one commit that could never fire in production, then in the very next commit used this technique and immediately confirmed the new override WAS load-bearing. Same session, same hands; the difference was thirty seconds of deliberately breaking it.

Same idea as mutation testing, but manual, targeted and instant — you mutate the one line you just wrote instead of the whole file.

Related: [[A read filtered on a value no writer produces fails by returning empty]] · [[Implementation is the best reviewer a design doc gets]]

## Related

- [[A read filtered on a value no writer produces fails by returning empty]]
- [[Implementation is the best reviewer a design doc gets]]
