---
ai_hash: 1c1367b8656b7db5
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-27
entities:
- A2A
- Task
- taskId
- contextId
- agent
- status.state
- client
- conversation
- bridge/adapter
- MCP tool
- mapping `context_id -> task_id`
- A2A message/send returns either a Message or a Task envelope
- A2A-to-MCP bridge is an MCP stdio server that is also an A2A client
- Message
- Task envelope
- MCP stdio server
- A2A client
- new task
- fresh context
- new interrogation
- task never advances
- lack of persistence
source: session 2026-08-27
status: seedling
tags:
- a2a
- agents
- state
- gotcha
title: A2A input-required tasks must be answered on the same taskId and contextId
type: lesson
---

# A2A input-required tasks must be answered on the same taskId and contextId

A2A models a multi-turn (human-in-the-loop) exchange as a **paused Task**: the agent replies with `status.state == "input-required"` and waits. To resume it, the client must send its next message on the **same `taskId` AND the same `contextId`** — a new task or a fresh context starts a different conversation instead of answering the pending one.

Implication for a bridge/adapter whose own calls are stateless (e.g. an MCP tool invoked independently each time): it must **persist the mapping `context_id -> task_id`** across calls, reuse the stored `task_id` when forwarding an answer, and clear it once `status.state == "completed"`. Without that, every answer starts a new interrogation and the task never advances.

See [[A2A messagesend returns either a Message or a Task envelope|A2A message/send returns either a Message or a Task envelope]], [[A2A-to-MCP bridge is an MCP stdio server that is also an A2A client]].

## Related

- [[A2A messagesend returns either a Message or a Task envelope|A2A message/send returns either a Message or a Task envelope]]
- [[A2A-to-MCP bridge is an MCP stdio server that is also an A2A client]]

%% ai-graph-start %%

**Related notes:**
- [[A2A multi-turn human-in-the-loop via input-required Task state]]
- [[A2A messagesend returns either a Message or a Task envelope]]
- [[BridgeSession turn drops the answer when the A2A task completes each turn]]
- [[a2a-sdk enqueue initial Task before any TaskStatusUpdateEvent]]
- [[A2A-to-MCP bridge is an MCP stdio server that is also an A2A client]]

**Relations:**
- A2A — *requires* — taskId
- A2A — *requires* — contextId
- A2A — *models* — Task
- Task — *is* — paused
- agent — *replies with* — status.state
- status.state — *is* — input-required
- client — *sends message on* — taskId
- client — *sends message on* — contextId
- new task — *starts* — conversation
- fresh context — *starts* — conversation
- bridge/adapter — *has property* — stateless calls
- bridge/adapter — *must persist* — mapping `context_id -> task_id`
- bridge/adapter — *reuses* — taskId
- bridge/adapter — *clears* — mapping `context_id -> task_id`
- mapping `context_id -> task_id` — *cleared when* — status.state is "completed"
- lack of persistence — *leads to* — new interrogation
- lack of persistence — *leads to* — task never advances
- A2A — *references* — A2A message/send returns either a Message or a Task envelope
- A2A — *references* — A2A-to-MCP bridge is an MCP stdio server that is also an A2A client
- A2A message/send returns either a Message or a Task envelope — *returns* — Message
- A2A message/send returns either a Message or a Task envelope — *returns* — Task envelope
- A2A-to-MCP bridge is an MCP stdio server that is also an A2A client — *is a type of* — bridge/adapter
- A2A-to-MCP bridge is an MCP stdio server that is also an A2A client — *is a* — MCP stdio server
- A2A-to-MCP bridge is an MCP stdio server that is also an A2A client — *is an* — A2A client

%% ai-graph-end %%