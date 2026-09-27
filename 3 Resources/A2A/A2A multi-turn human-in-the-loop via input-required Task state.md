---
ai_hash: 86344a16f2dd8836
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-27
entities:
- A2A
- Human-in-the-loop
- Task state
- input-required
- Agent
- Human
- Client
- message/artifact
- message/send
- taskId
- working
- contextId
- Session state
- Long-term memory
- GCS folder
- tasks/get
- tasks/resubscribe
- Server restart
- SSE reconnect
- A2A short-term memory
- Task history
- One-shot A2A skill
- Human judgement call
- Knowledge Refinement
- Testing Agent
- LUZ-159671
- Ground-then-refine gathering grounds, refinement interprets and confirms
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

- [[Ground-then-refine gathering grounds, refinement interprets and confirms]]

%% ai-graph-start %%

**Related notes:**
- [[A2A input-required tasks must be answered on the same taskId and contextId]]
- [[Ground-then-refine gathering grounds, refinement interprets and confirms]]
- [[a2a-sdk enqueue initial Task before any TaskStatusUpdateEvent]]
- [[BridgeSession turn drops the answer when the A2A task completes each turn]]
- [[Testing Agent builds each pipeline stage as a package mirroring the knowledge_gathering skeleton]]

**Relations:**
- A2A — *supports* — Human-in-the-loop
- Agent — *transitions* — Task state
- Task state — *becomes* — input-required
- Agent — *emits* — message/artifact
- Client — *relays to* — Human
- Client — *replies with* — message/send
- message/send — *carries* — taskId
- message/send — *moves Task to* — working
- Dialogue — *grouped under* — contextId
- contextId — *shares conversation with* — input
- Session state — *persists to* — Long-term memory
- Long-term memory — *is* — GCS folder
- Long-term memory — *keyed by* — contextId
- tasks/get — *resumes* — Dialogue
- tasks/resubscribe — *resumes* — Dialogue
- Server restart — *requires* — tasks/get
- Server restart — *requires* — tasks/resubscribe
- A2A short-term memory — *covers* — live turn
- A2A short-term memory — *includes* — Task history
- A2A short-term memory — *includes* — contextId
- Multi-turn shape — *fits* — Human judgement call
- Knowledge Refinement — *is* — Step 2
- Step 2 — *of* — Testing Agent
- Testing Agent — *is* — LUZ-159671
- Ground-then-refine gathering grounds, refinement interprets and confirms — *related to* — Knowledge Refinement

%% ai-graph-end %%