---
title: "ADK built-in logging does not cover env-gated per-agent app logging"
created: 2026-09-08
type: lesson
status: seedling
source: "session 2026-09-08"
tags: [google-adk, logging, test-agent-v2, gotcha]
---

# ADK built-in logging does not cover env-gated per-agent app logging

ADK's built-in logging configures **its own** runtime, not your application loggers, and it has no on/off toggle — so a custom env-gated logger (like test-agent-v2's `LoggingToggle`) is complementary, not redundant.

What ADK actually ships:
- `cli/utils/logs.py` → `setup_adk_logger(level)` — a one-shot `basicConfig` that sets the level on ADK's own `google_adk` logger namespace. **No env toggle**; it never touches your app namespaces.
- `plugins/logging_plugin.py` (`LoggingPlugin`) + `debug_logging_plugin.py` — a Runner `BasePlugin` that traces the invocation lifecycle. Its docstring explicitly says *"not a replacement of existing logging in ADK."*
- `telemetry/*` — OpenTelemetry spans/metrics (Cloud Trace, sqlite exporter).

What it does **not** give you: a logger under your own app namespace (`knowledge_gathering.*`, `test_plan_definition.*`) and an env-gated per-namespace on/off switch (`KGA_LOG` / `TPD_LOG` / `TEV_LOG`, level → `CRITICAL+1` when off). That toggle is the entire reason a `LoggingToggle` shim exists.

Rule of thumb during an ADK migration: ADK logs its runtime under `google_adk`; your deterministic engine code keeps logging under your own namespaces via `get_logger()`. Deleting the app-side toggle to "let ADK handle it" silences your own code and gains nothing.

See [[ADK LoggingPlugin gives free invocation-lifecycle tracing via a Runner BasePlugin]] for the additive ADK-native option.

## Related

- [[ADK LoggingPlugin gives free invocation-lifecycle tracing via a Runner BasePlugin]]
- [[Adopt Google ADK only when the LLM drives the tool loop; else stay a2a-sdk-direct]]
