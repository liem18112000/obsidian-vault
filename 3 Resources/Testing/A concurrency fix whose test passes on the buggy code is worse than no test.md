---
title: "A concurrency fix whose test passes on the buggy code is worse than no test"
created: 2026-09-27
type: lesson
status: seedling
source: "luz-hooks-plugin simplify-gate PR, 2026-09-27"
tags: [testing, concurrency, mutation-testing, honesty]
---

# A concurrency fix whose test passes on the buggy code is worse than no test

When a bug is a race, the test you can write from a shell script usually **passes on the broken code**, and shipping it converts "untested" into "falsely reported as covered" — strictly worse, because the next person reads green and stops looking.

Concretely: two hooks race to claim one event. I fixed it with an atomic exclusive create and wrote a test that spawned 8, then 12, concurrent processes and asserted exactly one fired. It passed. Then I ran it against a deliberately reverted read-then-write version — **it passed there too**. Process startup (~50ms of node boot, with jitter larger than the window being tested) serialises the racers: by the time #2 reaches the claim check, #1 has already written. A spin-barrier to release them simultaneously did not help either — it starved the box and *nothing* fired.

The general shape: **a race test is only meaningful if the contention window is wide relative to your scheduling jitter.** Sub-millisecond windows reached after tens of milliseconds of process startup are unreachable from the shell. Reaching them needs in-process threads, a fault-injection hook that widens the window on purpose, or a model checker — none of which is worth it for a 2-line fix.

So the honest move is to **delete the test and write down why**, in all three places a reader might look: the test header, the component README, and the PR description. State what IS covered (the logic — fires unclaimed, defers when claimed, mutation-verified), what is NOT (the interleaving), why it cannot be, and what the guarantee actually rests on (documented `O_EXCL` / `CREATE_NEW` semantics). Add the instruction that matters operationally: *if you change how the claim is taken, review it by hand — this script will not catch a regression.*

Mutation testing is what exposed all of this. A green test means nothing until you have watched it go red: revert the fix on a scratch copy and re-run. Do this for **each** assertion separately — mine caught "claim handling deleted entirely" but not "claim handling made non-atomic", and only checking both revealed which property was really covered.

Related: [[Cooperating Claude Code hooks on one event need a shared claim file]]

## Related

- [[Cooperating Claude Code hooks on one event need a shared claim file]]
