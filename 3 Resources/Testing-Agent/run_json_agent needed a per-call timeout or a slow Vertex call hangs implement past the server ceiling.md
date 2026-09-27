---
title: "run_json_agent needed a per-call timeout or a slow Vertex call hangs implement past the server ceiling"
created: 2026-09-14
type: lesson
status: seedling
source: "session 2026-09-14 run-cd156028"
tags: [testing-agent, timeout, asyncio, vertex, fallback, implement-plan, fix, test-agent-v2]
---

# run_json_agent needed a per-call timeout or a slow Vertex call hangs implement past the server ceiling

In test-agent-v2, every TPD implement generator (scenarios, the P4 judge, steps, test-data) runs through one shared driver, `common/testplan/llm/adk.py::run_json_agent`, which spins a throwaway ADK `Runner` around an `LlmAgent`. It originally had **no timeout** — it only caught exceptions (invalid/empty output → `None` → heuristic). A Vertex call that is *slow but not failing* therefore hung the whole `implement_plan` handler; Cloud Run / the MCP tool killed it at the ~900s ceiling **before** `store.write_scenarios` ran, so `get_scenarios` came back empty and `evaluate_plan` had nothing to score (observed on run-cd156028 / LUZ-158230).

**Fix (root cause, one guard for all four callers):** wrap the runner drive in `asyncio.wait_for(_drive(), timeout=_gen_timeout_s())` (default 180s, env `TPD_GEN_TIMEOUT_S`, floored at 1.0s). On `TimeoutError` return `None` → the caller already degrades to `heuristic_scenarios`. Belt-and-suspenders in `assured/loop.py`: a whole-loop wall-clock **budget** (`TPD_ASSURED_BUDGET_S`, default 540s) checked at the top of each round, and a **guaranteed non-empty return** (`best or scenarios or heuristic_scenarios(...)`) so scenarios are ALWAYS persisted — plus a 'degraded/stuck' note surfaced to the client.

**Design principle:** a best-effort LLM path must be *bounded*, not just *exception-guarded* — 'never raises' is not 'never hangs'. Any always-on generate→judge loop over an uncapped work set (here: 7 test kinds) needs a per-call timeout AND a whole-loop budget under the server's request ceiling, and must degrade to a deterministic fallback that still persists output.

**Test technique:** the offline `FakeGeneratorModel` got a `delay_s` field (`await asyncio.sleep`) to exercise the timeout; because the helper floors the timeout at 1.0s, the test uses `TPD_GEN_TIMEOUT_S=1` + `delay_s=1.2`.

Related: [[Deployed implement_plan P4 assured loop times out at 900s for broad features]]

## Related

- [[Deployed implement_plan P4 assured loop times out at 900s for broad features]]
