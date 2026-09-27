---
ai_hash: 05135af7b143a716
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-16
entities: []
source: test-agent-v2 deploy f65c3cc, LUZ-158230, 2026-09-16
status: seedling
tags:
- testing-agent
- cloud-run
- liveness
- vertex
- event-loop
- gotcha
title: Deployed TPD implement_plan trips Cloud Run liveness (event-loop blocked by
  Vertex gen)
type: lesson
---

# Deployed TPD implement_plan trips Cloud Run liveness (event-loop blocked by Vertex gen)

Deployed test-plan-definition-agent-v2 dies during implement_plan generation: Cloud Run logs show "LIVENESS HTTP probe failed 3 times consecutively for container agent on port 8080 path /livez ... instance has been shut down" + "request failed because either the HTTP response was malformed or connection to the instance had an error" (ERROR_TIMEOUT). The MCP gateway surfaces this to the client as "Error executing tool implement_plan" (fast, synchronous).

Cause: the scenario generation (a Vertex/Claude call, up to TPD_GEN_TIMEOUT_S=180s, 16000 max_tokens) blocks the single uvicorn event loop, so the /livez health endpoint cannot respond within the probe window → Cloud Run kills the instance mid-generation. Reads (get_plan/get_scenarios) still work because they do not block.

Why it did not bite the first (pre-fix) run: that generation degraded to the heuristic FAST (LLM timed out/errored quickly → None → fast heuristic), so no long block. When generation actually engages the LLM (e.g. a rich guidance re-run), it blocks long enough to trip liveness.

This is the recurrence of the known "implement serial Vertex calls → Cloud Run timeout" family. The chunking (max_rounds=1 per MCP call) and the asyncio.wait_for(timeout) guard do NOT prevent liveness death, because a single blocking round still starves the loop for the whole timeout.

Proper fix: run the blocking model call OFF the event loop (asyncio.to_thread / a real async client) so /livez stays responsive during generation; or raise the liveness probe timeout/failureThreshold; lowering TPD_GEN_TIMEOUT_S alone is insufficient (even ~10-30s of blocking trips the probe). Diagnose via: gcloud logging read service_name=test-plan-definition-agent-v2 severity>=ERROR.

Related: [[implement_plan heuristic-fallback emits one performance stub per node]] · [[TPD test_kinds must be additive over the base four, not replace them]]

## Related

- [[implement_plan heuristic-fallback emits one performance stub per node]]

%% ai-graph-start %%

**Related notes:**
- [[run_json_agent needed a per-call timeout or a slow Vertex call hangs implement past the server ceiling]]
- [[Fix TPD scenario generator truncation — raise max_tokens, keep one call]]
- [[Testing-Agent implement_plan assured loop times out at 900s MCP ceiling]]
- [[When agent-ifying LLM calls, preserve the per-call latency budget by gating non-essential generators behind a flag]]
- [[implement_plan heuristic-fallback emits one performance stub per node]]

%% ai-graph-end %%