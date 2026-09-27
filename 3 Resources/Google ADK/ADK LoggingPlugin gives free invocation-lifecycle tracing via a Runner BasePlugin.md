---
title: "ADK LoggingPlugin gives free invocation-lifecycle tracing via a Runner BasePlugin"
created: 2026-09-08
type: howto
status: seedling
source: "session 2026-09-08"
tags: [google-adk, logging, plugins, test-agent-v2]
---

# ADK LoggingPlugin gives free invocation-lifecycle tracing via a Runner BasePlugin

ADK ships `LoggingPlugin` (and a richer `debug_logging_plugin`) — a Runner `BasePlugin` that logs the whole invocation lifecycle to the console for free: user message, agent flow, LLM request/response, tool calls with args + results, events, and model/tool errors. You get it by registering it on the Runner's `plugins=[...]` list.

Why this matters: it is the ADK-idiomatic way to see agent/LLM/tool flow without hand-writing trace logs at every callback. But it is **additive** — it traces the ADK runtime; it does not configure or replace your app's own `logger.info()` calls (see [[ADK built-in logging does not cover env-gated per-agent app logging]]).

Low-friction to adopt if you already use ADK plugins. In test-agent-v2 the codebase already subclasses `BasePlugin` in `common/adk/plugins.py` (`LearnDrainPlugin`, `LessonRecallPlugin`), so adding ADK's `LoggingPlugin` to the same plugin list is a one-line change — keep the app-side `LoggingToggle` alongside it, not instead of it.

```python
from google.adk.plugins import LoggingPlugin
runner = Runner(..., plugins=[LoggingPlugin(), LearnDrainPlugin(), LessonRecallPlugin()])
```

## Related

- [[ADK built-in logging does not cover env-gated per-agent app logging]]
