---
ai_hash: 62bb6cb89be0dd8d
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
aliases:
- typesafe-sdk usage
- system_one
created: 2026-09-22
entities: []
source: session 2026-09-22, live SDK introspection
status: seedling
tags:
- typesafe
- sdk
- python
- decision-engine
- jev
title: TypeSafe SDK Python system_one usage (v0.7.1)
type: howto
---

# TypeSafe SDK Python system_one usage (v0.7.1)

The TypeSafe **System-1** SDK exposes typed, non-generative decisions. Install into a uv-managed venv with `uv pip install typesafe-sdk` (note: the `test-agent-v2` venv ships **no pip** — `python -m pip` fails; use `uv pip`). The client auto-reads `TYPESAFE_API_KEY` from the environment for auth — no explicit key argument.

```python
from typesafe_sdk import TypeSafeClient, Choice, Score, Noul
with TypeSafeClient() as client:                       # AsyncTypeSafeClient for async
    resp = client.system_one(
        state="free-text situation to judge",          # str (JSONContent)
        questions={
            "a": Noul(instructions="Is it overdue?"),                       # boolean
            "b": Choice(instructions="Next action?", criteria={"remind": None, "dunning": None}),
            "c": Score(instructions="Severity?", criteria=["low", "medium", "high"]),
        },
    )
```

Constructors (all keyword-only): `Choice(instructions=, criteria={option: None, ...})`, `Score(instructions=, criteria=[ordered levels])`, `Noul(instructions=)`. `system_one(state, questions, *, model=None, timeout=, response_model=None, ...)` returns a `SystemOneResponse`. Reading results and their per-answer fields has surprises — see [[TypeSafe SDK response shape gotcha - cached-property accessors and Noul has no confidence]].

Verified live end-to-end against v0.7.1 on 2026-09-22.

## Related

- [[TypeSafe SDK response shape gotcha - cached-property accessors and Noul has no confidence]]

%% ai-graph-start %%

**Related notes:**
- [[TypeSafe SDK response shape gotcha - cached-property accessors and Noul has no confidence]]
- [[System One Models (Jev) fast type-safe calibrated decision models, not chat LLMs]]

%% ai-graph-end %%