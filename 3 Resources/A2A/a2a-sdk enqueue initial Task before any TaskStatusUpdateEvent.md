---
ai_hash: dff9ce8f1024a4b5
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-27
entities:
- a2a-sdk
- Task
- TaskStatusUpdateEvent
- AgentExecutor
- protobuf/v2 handlers
- TaskUpdater
- InvalidAgentResponseError
- new_task_from_user_message
- event_queue
- context.current_task
- context.message
- requires_input()
- complete()
- submit()
- new_agent_message()
- Message
- Knowledge-Refinement multi-turn dialogue
- test-agent
- LUZ-159671
- A2A multi-turn human-in-the-loop via input-required Task state
- a2a-sdk 1.x
source: session 2026-08-27 test-agent
status: seedling
tags:
- a2a
- a2a-sdk
- gotcha
- agents
- python
title: 'a2a-sdk: enqueue initial Task before any TaskStatusUpdateEvent'
type: lesson
---

# a2a-sdk: enqueue initial Task before any TaskStatusUpdateEvent

In a2a-sdk 1.x (protobuf/v2 handlers), an `AgentExecutor` that drives the Task lifecycle must **enqueue an initial `Task` object before emitting any `TaskStatusUpdateEvent`** (i.e. before `TaskUpdater.requires_input()` / `complete()` / even `submit()`). `submit()` is itself a status update, so it cannot be the first event — doing so raises `InvalidAgentResponseError: Agent should enqueue Task before TaskStatusUpdateEvent event`.

Fix on the first turn of a task:
```python
from a2a.helpers.proto_helpers import new_task_from_user_message
if context.current_task is None:  # turn 1
    await event_queue.enqueue_event(new_task_from_user_message(context.message))
updater = TaskUpdater(event_queue, context.task_id, context.context_id)
await updater.requires_input(updater.new_agent_message([...]))  # now legal
```
On a continuation turn the Task already exists (`context.current_task` is set), so skip the enqueue.

Contrast: enqueuing a plain **Message** (`updater.new_agent_message(...)` straight onto the queue, as a one-shot reply does) does NOT require a prior Task — only task **status** transitions do. So one-shot "reply and return" executors never hit this; only multi-turn ones using input-required/complete do.

Discovered wiring the Knowledge-Refinement multi-turn dialogue (test-agent, LUZ-159671). See [[A2A multi-turn human-in-the-loop via input-required Task state]].

## Related

- [[A2A multi-turn human-in-the-loop via input-required Task state]]

%% ai-graph-start %%

**Related notes:**
- [[BridgeSession turn drops the answer when the A2A task completes each turn]]
- [[A2A input-required tasks must be answered on the same taskId and contextId]]
- [[A2A multi-turn human-in-the-loop via input-required Task state]]
- [[ADK LongRunningFunctionTool HITL nested in SequentialAgent has resume bugs]]
- [[A2A to_a2a task_store and runner are separate persistence params]]

**Relations:**
- a2a-sdk — *HAS_VERSION* — a2a-sdk 1.x
- a2a-sdk 1.x — *USES* — protobuf/v2 handlers
- AgentExecutor — *DRIVES* — Task lifecycle
- AgentExecutor — *MUST_ENQUEUE* — Task
- Task — *MUST_BE_ENQUEUED_BEFORE* — TaskStatusUpdateEvent
- TaskUpdater — *HAS_METHOD* — requires_input()
- TaskUpdater — *HAS_METHOD* — complete()
- TaskUpdater — *HAS_METHOD* — submit()
- submit() — *IS_A* — TaskStatusUpdateEvent
- submit() — *CANNOT_BE* — first event
- submit() — *RAISES* — InvalidAgentResponseError
- InvalidAgentResponseError — *HAS_MESSAGE* — Agent should enqueue Task before TaskStatusUpdateEvent event
- new_task_from_user_message — *CREATES* — Task
- event_queue — *ENQUEUES* — Task
- event_queue — *ENQUEUES* — Message
- requires_input() — *REQUIRES* — context.current_task
- context.current_task — *REPRESENTS* — current Task
- Message — *DOES_NOT_REQUIRE* — context.current_task
- Task status transitions — *REQUIRE* — context.current_task
- new_agent_message() — *CREATES* — Message
- Knowledge-Refinement multi-turn dialogue — *DISCOVERED_ISSUE_IN* — a2a-sdk
- test-agent — *IS_PART_OF* — Knowledge-Refinement multi-turn dialogue
- LUZ-159671 — *RELATED_TO* — Knowledge-Refinement multi-turn dialogue
- A2A multi-turn human-in-the-loop via input-required Task state — *RELATED_TO* — a2a-sdk
- A2A multi-turn human-in-the-loop via input-required Task state — *IS_A* — related concept
- new_task_from_user_message — *USES* — context.message
- requires_input() — *USES* — new_agent_message()

%% ai-graph-end %%