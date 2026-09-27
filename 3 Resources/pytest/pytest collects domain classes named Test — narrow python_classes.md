---
ai_hash: 427f65d6d9cf03e9
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-28
entities: []
source: session 2026-08-28, test_plan_definition M0 scaffold
status: seedling
tags:
- pytest
- python
- gotcha
- testing
title: pytest collects domain classes named Test* — narrow python_classes
type: lesson
---

# pytest collects domain classes named Test* — narrow python_classes

pytest's default class-collection heuristic treats any class whose name starts with `Test` as a test class. If your **domain model** is named `Test*` (e.g. `TestPlan`, `TestData`, `TestScenario`, `TestStep`), pytest tries to collect it and emits `PytestCollectionWarning: cannot collect test class 'TestPlan' because it has a __init__ constructor`. It is only a warning (the class is skipped, no phantom tests run), but it is recurring noise and would become fragile if such a class ever lacked an `__init__`.

**Fix (project-wide, when the suite uses only function-style tests):** narrow the collection pattern in `pyproject.toml` so classes need a distinctive suffix, not the `Test` prefix:
```
[tool.pytest.ini_options]
python_classes = ["*Tests"]
```
Verify first that no existing test relies on a `Test`-prefixed class being collected (`grep -rnE "^class Test" tests/`). Alternatives: set `__test__ = False` on each model (pollutes domain code), or just accept the warning.

Applies whenever a testing domain legitimately owns `Test*` type names — common in QA/test-tooling code.

%% ai-graph-start %%

**Related notes:**
- [[pytest imports all test modules before applying -m deselection]]

%% ai-graph-end %%