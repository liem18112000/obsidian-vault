---
ai_hash: 349799d8ade959f3
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-08
entities: []
source: session 2026-09-08 executor NO_BANK refactor
status: seedling
tags:
- test-agent-v2
- a2a
- executor
- refactor
- SOLID
title: test-agent-v2 executor step handlers take the executor as first arg
type: lesson
---

# test-agent-v2 executor step handlers take the executor as first arg

In `test-agent-v2`, every executor **step handler** takes the executor instance (`ex`) as its **first positional argument** — `run_search_lessons`, `run_veto_lesson`, `run_search_memory`, `run_get_note`, `run_refine`, `run_read_helper`, `run_gather`, and TPD`s `run_define` / `run_approve` / `run_implement`. Any dispatch wrapper must forward `self` as that first arg; a `@staticmethod` that drops it is a silent bug (the linter flags it only as a duplicated-code hint, not an error).

The shared config-error string now lives as one constant:

```python
# src/common/executor.py
NO_BANK = "Config error: memory bank unavailable."
```

re-exported through each agent`s `executor/common.py` seam (KGA, TPD), while `test_evaluation` imports it directly from `common.executor`. It previously appeared as a literal 8x across the three executors.

KGA`s `execute()` dispatch was made SOLID: a data-driven `_MEMORY_COMMANDS` prefix -> handler table replaces the repeated `startswith` branches, and a single `_with_bank(self, ctx, eq, bank, handler, *args)` guard replaces the repeated `if bank is None: reply(NO_BANK)` checks and forwards `self`.

Related: [[test-agent common shared engine]]

## Related

- [[test-agent common shared engine]]

%% ai-graph-start %%

**Related notes:**
- [[test-agent-v2 executor tests share memory-bank state and fail by test order]]
- [[test-agent-v2 test_evaluation restructure engine + config packages + merged golden set]]
- [[Pipeline stages sharing a context_id need separate memory-bank path prefixes]]
- [[Testing Agent builds each pipeline stage as a package mirroring the knowledge_gathering skeleton]]
- [[test-agent-v2 KGA has no live LlmAgent — explore steps are the first ADK LlmAgent target]]

%% ai-graph-end %%