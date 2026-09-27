---
ai_hash: 9c65a23cb8ef1089
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-27
entities:
- A2A multi-turn human-in-the-loop
- Agent2Agent (A2A) protocol
- Task
- '`input-required` state'
- Agent
- questions
- message/artifact
- Client
- human
- '`message/send`'
- '`taskId`'
- '`working` state'
- '`contextId`'
- dialogue
- session state
- long-term memory
- GCS folder
- '`tasks/get`'
- '`tasks/resubscribe`'
- server restart
- A2A short-term memory
- task history
- one-shot A2A skill
- human judgement call
- Step 2 (Knowledge Refinement)
- test-agent Testing Agent
- LUZ-159671
- 'Ground-then-refine: gathering grounds'
- refinement interprets and confirms
- answers
- running understanding
source: session 2026-08-27 test-agent
status: seedling
tags:
- a2a
- agents
- human-in-the-loop
- protocol
- memory
title: A2A multi-turn human-in-the-loop via input-required Task state
type: howto
---

# A2A multi-turn human-in-the-loop via input-required Task state

An interactive "agent asks clarifying questions → human answers → agent refines" dialogue can be modeled directly on the Agent2Agent (A2A) protocol without inventing new transport: the agent transitions the **Task to the `input-required` state** and emits its questions as the message/artifact; the client (e.g. local Claude relaying to a human) replies with `message/send` carrying the **same `taskId`**, moving the Task back to `working`. Group the whole dialogue under one **`contextId`** so it shares the conversation with whatever produced the input.

Key durability trick: persist the session state (questions/answers/running understanding) to **long-term memory (e.g. a GCS folder keyed by `contextId`)**, not just in-process. Then `tasks/get` / `tasks/resubscribe` resume the dialogue after **a full server restart**, not merely an SSE reconnect. A2A short-term memory (task history + contextId) covers the live turn; the persisted store covers durability.

Contrast with a one-shot A2A skill (single `message/send` → result artifacts): the multi-turn shape is the right fit whenever a human judgement call must be injected mid-task.

Source: designing Step 2 (Knowledge Refinement) of the test-agent Testing Agent (LUZ-159671).

## Related

- [[Ground-then-refine: gathering grounds]]
- [[refinement interprets and confirms]]

%% ai-graph-start %%

**Related notes:**
- [[A2A input-required tasks must be answered on the same taskId and contextId]]
- [[Ground-then-refine gathering grounds, refinement interprets and confirms]]
- [[a2a-sdk enqueue initial Task before any TaskStatusUpdateEvent]]
- [[BridgeSession turn drops the answer when the A2A task completes each turn]]
- [[Testing Agent builds each pipeline stage as a package mirroring the knowledge_gathering skeleton]]

**Relations:**
- A2A multi-turn human-in-the-loop — *uses* — Agent2Agent (A2A) protocol
- A2A multi-turn human-in-the-loop — *involves* — Agent
- A2A multi-turn human-in-the-loop — *involves* — human
- Agent — *transitions* — Task
- Task — *transitions to* — `input-required` state
- Agent — *emits* — questions
- questions — *are* — message/artifact
- Client — *replies with* — `message/send`
- `message/send` — *carries* — `taskId`
- Client — *relays to* — human
- human — *provides* — answers
- Task — *moves to* — `working` state
- dialogue — *grouped by* — `contextId`
- session state — *persisted to* — long-term memory
- session state — *includes* — questions
- session state — *includes* — answers
- session state — *includes* — running understanding
- long-term memory — *example* — GCS folder
- long-term memory — *keyed by* — `contextId`
- `tasks/get` — *resumes* — dialogue
- `tasks/resubscribe` — *resumes* — dialogue
- dialogue — *resumes after* — server restart
- A2A short-term memory — *covers* — live turn
- A2A short-term memory — *includes* — task history
- A2A short-term memory — *includes* — `contextId`
- long-term memory — *covers* — durability
- A2A multi-turn human-in-the-loop — *is fit for* — human judgement call
- A2A multi-turn human-in-the-loop — *contrasts with* — one-shot A2A skill
- A2A multi-turn human-in-the-loop — *designed for* — Step 2 (Knowledge Refinement)
- Step 2 (Knowledge Refinement) — *is part of* — test-agent Testing Agent
- test-agent Testing Agent — *has ID* — LUZ-159671
- A2A multi-turn human-in-the-loop — *related to* — Ground-then-refine: gathering grounds
- A2A multi-turn human-in-the-loop — *related to* — refinement interprets and confirms

%% ai-graph-end %%