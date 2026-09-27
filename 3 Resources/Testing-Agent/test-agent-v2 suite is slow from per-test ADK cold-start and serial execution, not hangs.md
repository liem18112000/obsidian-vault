---
ai_hash: d1569099670a57b8
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-14
entities: []
source: session 2026-09-14
status: seedling
tags:
- testing-agent
- pytest
- performance
- adk
- test-agent-v2
title: test-agent-v2 suite is slow from per-test ADK cold-start and serial execution,
  not hangs
type: lesson
---

# test-agent-v2 suite is slow from per-test ADK cold-start and serial execution, not hangs

The test-agent-v2 pytest suite feels like it 'hangs' but it does not — it is ~503 tests run **serially** with heavy per-test overhead:

- **Collection alone ~18s** — importing google-adk + google-genai + litellm + pydantic across every module.
- **Building an ADK agent/Runner costs multiple seconds PER test.** Measured: `test_root_agents_are_discoverable` = 5.83s for one test (vs 0.01s for non-agent tests beside it); the KGA+TPD ADK files ran ~9s/test. Each test that spins up a Runner re-pays ADK + google-genai telemetry/tracing cold-start.
- **No parallelism** — `pytest-xdist` is not installed, so all 503 run on one core.
- A few tests wait deliberately (e.g. the run_json_agent timeout test ~20s for asyncio.wait_for cancellation + cleanup; the .env-Vertex offline guard; any pgvector/eval integration).

Net: a full serial run measured **9048s (2h30m)** for 489 passed / 14 skipped — dominated by ADK/genai build cost × serial count, NOT network hangs. That figure was inflated by CPU contention (I had 3 overlapping full-suite runs going — never launch more than one). A clean single run is less but still long.

**Fastest win:** `pip install pytest-xdist` then `pytest -n auto` — the 503 independent tests parallelize across cores (typ. 4-8x wall-clock). Second-order: a session-scoped fixture that builds the root agents once (instead of per-test) would remove the repeated cold-start.

Related: [[run_json_agent needed a per-call timeout or a slow Vertex call hangs implement past the server ceiling]]

## Related

- [[run_json_agent needed a per-call timeout or a slow Vertex call hangs implement past the server ceiling]]

%% ai-graph-start %%

**Related notes:**
- [[run_json_agent needed a per-call timeout or a slow Vertex call hangs implement past the server ceiling]]
- [[When agent-ifying LLM calls, preserve the per-call latency budget by gating non-essential generators behind a flag]]
- [[test-agent-v2 KGA has no live LlmAgent — explore steps are the first ADK LlmAgent target]]
- [[Why local test-agent is slow claude -p ships a 17.5K agent prompt on Opus, x many serial calls]]
- [[test-agent-v2 executor tests share memory-bank state and fail by test order]]

%% ai-graph-end %%