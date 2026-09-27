---
ai_hash: 558c83edc688ec72
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-14
entities:
- BridgeSession
- A2A task
- ADK agent
- to_a2a
- A2A layer
- task state completed
- input-required
- long-running-tool semantics
- test-agent-v2
- common/bridge/session.py
- BridgeSession.turn
- task_id
- refine
- define_plan
- start text
- human's answer
- interrogation
- answer
- A2A context_id
- durable thread
- interrogation agent
- bank
- adk_a2a_app
- A2ABridgeClient
- A2A to_a2a task_store
- runner
- persistence params
- Bug
- Fix
- nothing
source: test-agent-v2, session 2026-09-14
status: seedling
tags:
- adk
- a2a
- bridge
- interrogation
- hitl
- gotcha
- test-agent
title: BridgeSession turn drops the answer when the A2A task completes each turn
type: lesson
---

# BridgeSession turn drops the answer when the A2A task completes each turn

A custom ADK agent served over `to_a2a` **finishes its invocation every turn** (it yields its output and returns), so the A2A layer reports task `state="completed"` on EACH turn — it never signals `input-required` unless you use long-running-tool semantics.

Bug this caused (test-agent-v2 `common/bridge/session.py`): `BridgeSession.turn` decided start-vs-answer by whether a live A2A `task_id` existed, and popped the task_id whenever `state=="completed"`. Because every turn completed, the task_id was always dropped, so the NEXT `refine`/`define_plan` answer turn found no task_id and fell into the else-branch — **re-sending the start text and silently discarding the humans answer**. The interrogation advanced rounds but persisted nothing (thin brief, low confidence, answered questions re-listed as gaps).

Fix: route by `answer is not None`, NOT by a live task_id. The A2A `context_id` is the durable thread — the interrogation agent rehydrates its own per-round state from the bank keyed on context_id, so the answer just needs to reach it. One-line-ish change; fixes both refine and define_plan (both go through `turn`).

Diagnostic that pinned it: drive the real agent through the real bridge (`adk_a2a_app` + `A2ABridgeClient` + `BridgeSession`) two turns and assert the answer was ingested — the loop-level machinery tested fine in isolation, so only the bridge round-trip exposed it.

## Related
[[A2A to_a2a task_store and runner are separate persistence params]]

%% ai-graph-start %%

**Related notes:**
- [[A2A to_a2a task_store and runner are separate persistence params]]
- [[a2a-sdk enqueue initial Task before any TaskStatusUpdateEvent]]
- [[A2A input-required tasks must be answered on the same taskId and contextId]]
- [[ADK LongRunningFunctionTool HITL nested in SequentialAgent has resume bugs]]
- [[A2A multi-turn human-in-the-loop via input-required Task state]]

**Relations:**
- BridgeSession.turn — *drops_answer_when* — A2A task completes each turn
- A2A task — *completes_each_turn* — 
- ADK agent — *served_over* — to_a2a
- ADK agent — *finishes_invocation* — every turn
- A2A layer — *reports_task_state* — task state completed
- A2A layer — *reports_task_state_for* — A2A task
- A2A layer — *signals* — input-required
- A2A layer — *signals_conditionally_on* — long-running-tool semantics
- Bug — *occurred_in* — test-agent-v2
- Bug — *occurred_in* — common/bridge/session.py
- BridgeSession.turn — *decided_start_vs_answer_by* — task_id
- BridgeSession.turn — *popped* — task_id
- BridgeSession.turn — *popped_on_state* — task state completed
- task_id — *was_dropped_by* — BridgeSession.turn
- refine — *expected* — task_id
- define_plan — *expected* — task_id
- BridgeSession.turn — *re-sent* — start text
- BridgeSession.turn — *discarded* — human's answer
- interrogation — *advanced_rounds_but_persisted* — nothing
- Fix — *routes_by* — answer
- Fix — *replaces_routing_by* — task_id
- A2A context_id — *is_a* — durable thread
- interrogation agent — *rehydrates_state_from* — bank
- bank — *keyed_on* — A2A context_id
- answer — *needs_to_reach* — interrogation agent
- Fix — *applies_to* — refine
- Fix — *applies_to* — define_plan
- refine — *goes_through* — BridgeSession.turn
- define_plan — *goes_through* — BridgeSession.turn
- Diagnostic — *involved* — adk_a2a_app
- Diagnostic — *involved* — A2ABridgeClient
- Diagnostic — *involved* — BridgeSession
- A2A to_a2a task_store — *is_related_to* — runner
- A2A to_a2a task_store — *has_property* — persistence params
- runner — *has_property* — persistence params

%% ai-graph-end %%