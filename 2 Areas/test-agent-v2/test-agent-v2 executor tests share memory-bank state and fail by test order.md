---
ai_hash: 3c91f0609753d162
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-08
entities:
- test-agent-v2 executor tests
- memory-bank state
- test order
- KGA/TPD/eval A2A executor tests
- test-agent-v2
- memory-bank fixture
- test_refine_multiturn_pauses_then_completes
- tests/test_refine_a2a.py
- multi-file pytest batch
- isolation
- state bleed
- test_executor_needs_a_context_id
- tests/eval/test_engine.py
- pre-existing failure
- test_executor_needs_a_context_id is a pre-existing failure in test-agent-v2
- test-agent common shared engine
- test-agent-v2 executor step handlers take the executor as first arg
- pytest
- executor-test failure
- ordering-only failures
source: session 2026-09-08 executor NO_BANK refactor
status: seedling
tags:
- test-agent-v2
- pytest
- gotcha
- flaky-tests
title: test-agent-v2 executor tests share memory-bank state and fail by test order
type: lesson
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

%% ai-graph-start %%

**Related notes:**
- [[test-agent-v2 executor step handlers take the executor as first arg]]
- [[Pipeline stages sharing a context_id need separate memory-bank path prefixes]]
- [[test-agent-v2 suite is slow from per-test ADK cold-start and serial execution, not hangs]]
- [[test-agent-v2 run benchmark — TEV ownership forced by layering]]
- [[test-agent-v2 test_evaluation restructure engine + config packages + merged golden set]]

**Relations:**
- test-agent-v2 executor tests — *share* — memory-bank state
- test-agent-v2 executor tests — *fail by* — test order
- KGA/TPD/eval A2A executor tests — *are in* — test-agent-v2
- KGA/TPD/eval A2A executor tests — *share* — memory-bank fixture
- memory-bank fixture — *causes* — order-dependent
- test_refine_multiturn_pauses_then_completes — *is in* — tests/test_refine_a2a.py
- test_refine_multiturn_pauses_then_completes — *fails in* — multi-file pytest batch
- test_refine_multiturn_pauses_then_completes — *passes in* — isolation
- test_refine_multiturn_pauses_then_completes — *affected by* — state bleed
- test_executor_needs_a_context_id — *is in* — tests/eval/test_engine.py
- test_executor_needs_a_context_id — *fails in* — isolation
- test_executor_needs_a_context_id — *is a* — pre-existing failure
- test_executor_needs_a_context_id — *is documented in* — test_executor_needs_a_context_id is a pre-existing failure in test-agent-v2
- test-agent common shared engine — *is related to* — test-agent-v2
- test-agent-v2 executor step handlers take the executor as first arg — *is related to* — test-agent-v2
- executor-test failure — *can be* — pre-existing failure
- executor-test failure — *can be* — ordering-only failures
- multi-file pytest batch — *uses* — pytest
- test-agent-v2 executor tests — *are affected by* — pre-existing failure
- test-agent-v2 executor tests — *are affected by* — ordering-only failures

%% ai-graph-end %%