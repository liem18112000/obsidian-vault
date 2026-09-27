---
ai_hash: f38e34f519835450
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-21
entities: []
source: session 2026-09-21 (agent live-eval)
status: seedling
tags:
- python
- mocking
- unittest
- monkeypatch
- recursion
- gotcha
title: Monkeypatching a function that calls itself recurses — capture the original
  first
type: gotcha
---

# Monkeypatching a function that calls itself recurses — capture the original first

When you `unittest.mock.patch` (or otherwise monkeypatch) a function and your replacement needs to call the ORIGINAL, you must capture a reference to the original **before** the patch is installed. Calling the function by its patched name from inside the replacement re-enters the replacement — infinite recursion, surfacing as `RecursionError: maximum recursion depth exceeded`.

Concrete case: a usage-recording wrapper around `litellm.completion`, installed via `patch("litellm.completion", recorder)`. Inside `recorder.__call__` the live branch did `litellm.completion(*a, **k)` — but that name now resolves to `recorder` itself. Fix: in `recorder.__init__` (constructed before the `with patch(...)` block) do `import litellm; self._real = litellm.completion`, then call `self._real(*a, **k)`.

General rule: wrapper holds its own handle to the real callable, grabbed at construction time; never reach through the name it is patching. Same trap applies to decorators that re-lookup a global, and to `functools.wraps`-style shims that forget to bind the wrapped target.

## Related

- [[Hermetic E2E test of an LLM agent: mock only the SDK boundary]]
- [[inject the prompt snapshot]]

%% ai-graph-start %%

**Related notes:**
- [[Hermetic E2E test of an LLM agent mock only the SDK boundary, inject the prompt snapshot]]
- [[Test an LLM-vs-heuristic seam offline by monkeypatching complete() per module]]

%% ai-graph-end %%