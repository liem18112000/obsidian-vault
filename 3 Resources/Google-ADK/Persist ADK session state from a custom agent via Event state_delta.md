---
title: "Persist ADK session state from a custom agent via Event state_delta"
created: 2026-09-07
type: howto
status: seedling
source: "A0 spike 2026-09-07, google-adk 2.8.0"
tags: [google-adk, sessions, hitl, state]
---

# Persist ADK session state from a custom agent via Event state_delta

Inside an ADK custom agent (`BaseAgent._run_async_impl`), mutating `ctx.session.state` directly does **not** reliably persist across invocations. To checkpoint state so a later turn (or a rebuilt Runner/SessionService after a restart) reads it back, **yield an Event carrying a state delta**:

```python
from google.adk.events import Event, EventActions
yield Event(author=self.name,
            content=types.Content(role='model', parts=[types.Part(text=msg)]),
            actions=EventActions(state_delta={'answered': n, 'awaiting': True, 'answers': answers}))
```

The Runner applies `state_delta` through the SessionService, so it survives with a DatabaseSessionService. This is the load-bearing mechanic for **HITL pause/resume as a custom agent** ("Option B"): run one step, write state_delta, emit the prompt, end the invocation; the next invocation reads state and advances. Keep the agent **stateless** (no pydantic instance fields — BaseAgent is a pydantic model; extra fields need declaring) so any instance can resume any session — all loop state lives in session state.

Empirically validated on google-adk 2.8.0 (3-round interrogation paused/resumed in order, survived a simulated restart, no step re-executed). Related: [[ADK DatabaseSessionService needs the db extra and an async SQLAlchemy driver]], [[ADK LongRunningFunctionTool HITL nested in SequentialAgent has resume bugs]], [[ADK workflow agents orchestrate deterministically without an LLM-driven loop]].

## Related

- [[ADK DatabaseSessionService needs the db extra and an async SQLAlchemy driver]]
- [[ADK LongRunningFunctionTool HITL nested in SequentialAgent has resume bugs]]
