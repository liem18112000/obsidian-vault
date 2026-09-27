---
title: "test-agent-v2 executor tests share memory-bank state and fail by test order"
created: 2026-09-08
type: lesson
status: seedling
source: "session 2026-09-08 executor NO_BANK refactor"
tags: [test-agent-v2, pytest, gotcha, flaky-tests]
---

# test-agent-v2 executor tests share memory-bank state and fail by test order

The KGA / TPD / eval A2A executor tests in `test-agent-v2` share a common memory-bank fixture, so a passing test is **order-dependent**. Prove causation before blaming your own edit: `git stash push` your change, re-run the failing test, and compare.

Two concrete cases (2026-09-08):

- `tests/test_refine_a2a.py::test_refine_multiturn_pauses_then_completes` — **fails in a multi-file pytest batch, passes in isolation** (state bleed from earlier tests writing the shared bank).
- `tests/eval/test_engine.py::test_executor_needs_a_context_id` — **fails even in isolation on clean/unmodified code** (a genuine pre-existing failure, see [[test_executor_needs_a_context_id is a pre-existing failure in test-agent-v2]]).

Lesson: an executor-test failure here is not evidence your change broke something. Reproduce on a stashed tree first; do not chase pre-existing or ordering-only failures.

Related: [[test-agent common shared engine]] · [[test-agent-v2 executor step handlers take the executor as first arg]]

## Related

- [[test-agent common shared engine]]
- [[test-agent-v2 executor step handlers take the executor as first arg]]
