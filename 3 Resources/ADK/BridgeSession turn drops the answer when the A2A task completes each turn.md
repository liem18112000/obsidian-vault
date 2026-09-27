---
title: "BridgeSession turn drops the answer when the A2A task completes each turn"
created: 2026-09-14
type: lesson
status: seedling
source: "test-agent-v2, session 2026-09-14"
tags: [adk, a2a, bridge, interrogation, hitl, gotcha, test-agent]
---

# BridgeSession turn drops the answer when the A2A task completes each turn

A custom ADK agent served over `to_a2a` **finishes its invocation every turn** (it yields its output and returns), so the A2A layer reports task `state="completed"` on EACH turn — it never signals `input-required` unless you use long-running-tool semantics.

Bug this caused (test-agent-v2 `common/bridge/session.py`): `BridgeSession.turn` decided start-vs-answer by whether a live A2A `task_id` existed, and popped the task_id whenever `state=="completed"`. Because every turn completed, the task_id was always dropped, so the NEXT `refine`/`define_plan` answer turn found no task_id and fell into the else-branch — **re-sending the start text and silently discarding the humans answer**. The interrogation advanced rounds but persisted nothing (thin brief, low confidence, answered questions re-listed as gaps).

Fix: route by `answer is not None`, NOT by a live task_id. The A2A `context_id` is the durable thread — the interrogation agent rehydrates its own per-round state from the bank keyed on context_id, so the answer just needs to reach it. One-line-ish change; fixes both refine and define_plan (both go through `turn`).

Diagnostic that pinned it: drive the real agent through the real bridge (`adk_a2a_app` + `A2ABridgeClient` + `BridgeSession`) two turns and assert the answer was ingested — the loop-level machinery tested fine in isolation, so only the bridge round-trip exposed it.

## Related
[[A2A to_a2a task_store and runner are separate persistence params]]
