---
ai_hash: fdd828197f6aa57a
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-27
entities:
- A2A messagesend
- Message envelope
- Task envelope
- JSON-RPC message/send call
- A2A agent
- a2a-sdk 1.x
- proto v0.3 JSON
- result.kind
- result.parts[].text
- result.taskId
- result.contextId
- result.id (Task)
- result.status.state
- input-required state
- completed state
- result.status.message.parts[].text
- result.artifacts[].parts[]
- result.history[]
- client
- text extraction
- A2A input-required tasks must be answered on the same taskId and contextId
- A2A-to-MCP bridge is an MCP stdio server that is also an A2A client
- '{"kind":"text","text":...} (text format)'
source: session 2026-08-27
status: seedling
tags:
- a2a
- json-rpc
- agents
title: A2A message/send returns either a Message or a Task envelope
type: observation
---

# A2A message/send returns either a Message or a Task envelope

A JSON-RPC `message/send` call to an A2A agent (a2a-sdk 1.x, proto v0.3 JSON) comes back in **one of two shapes**, distinguished by `result.kind`:

- **Message** (`result.kind == "message"`) — a one-shot reply. Text lives in `result.parts[].text`. `result.taskId` and `result.contextId` are present. There is no status/state.
- **Task** (`result.kind == "task"`) — a longer-lived unit of work. The task id is `result.id` (not `taskId`), lifecycle is `result.status.state` (e.g. `input-required`, `completed`), and the agent's text is in `result.status.message.parts[].text`. There may also be `result.artifacts[].parts[]` and a `result.history[]`.

A robust client extracts text from **both** shapes: scan `parts`, then `status.message.parts`, then `artifacts[].parts`, then agent turns in `history`. Parts use `{"kind":"text","text":...}`.

See [[A2A input-required tasks must be answered on the same taskId and contextId]], [[A2A-to-MCP bridge is an MCP stdio server that is also an A2A client]].

## Related

- [[A2A input-required tasks must be answered on the same taskId and contextId]]
- [[A2A-to-MCP bridge is an MCP stdio server that is also an A2A client]]

%% ai-graph-start %%

**Related notes:**
- [[A2A input-required tasks must be answered on the same taskId and contextId]]
- [[a2a-sdk serves the agent card at agent-card.json (new) or agent.json (old)]]
- [[A2A-to-MCP bridge is an MCP stdio server that is also an A2A client]]
- [[A2A AgentSkill is advertisement metadata, not executor routing]]
- [[a2a-sdk enqueue initial Task before any TaskStatusUpdateEvent]]

**Relations:**
- A2A messagesend — *returns* — Message envelope
- A2A messagesend — *returns* — Task envelope
- JSON-RPC message/send call — *is a type of* — A2A messagesend
- JSON-RPC message/send call — *targets* — A2A agent
- A2A agent — *uses* — a2a-sdk 1.x
- A2A agent — *uses* — proto v0.3 JSON
- Message envelope — *distinguished by* — result.kind
- Task envelope — *distinguished by* — result.kind
- Message envelope — *has kind value* — message
- Message envelope — *is a* — one-shot reply
- Message envelope — *contains text in* — result.parts[].text
- Message envelope — *includes* — result.taskId
- Message envelope — *includes* — result.contextId
- Message envelope — *lacks* — status/state
- Task envelope — *has kind value* — task
- Task envelope — *is a* — longer-lived unit of work
- Task envelope — *has ID in* — result.id (Task)
- Task envelope — *has lifecycle in* — result.status.state
- result.status.state — *example* — input-required state
- result.status.state — *example* — completed state
- Task envelope — *contains agent text in* — result.status.message.parts[].text
- Task envelope — *may include* — result.artifacts[].parts[]
- Task envelope — *may include* — result.history[]
- client — *performs* — text extraction
- text extraction — *from* — Message envelope
- text extraction — *from* — Task envelope
- text extraction — *scans* — result.parts[].text
- text extraction — *scans* — result.status.message.parts[].text
- text extraction — *scans* — result.artifacts[].parts[]
- text extraction — *scans* — result.history[]
- result.parts[].text — *uses format* — {"kind":"text","text":...} (text format)
- result.status.message.parts[].text — *uses format* — {"kind":"text","text":...} (text format)
- result.artifacts[].parts[] — *uses format* — {"kind":"text","text":...} (text format)
- A2A messagesend — *references* — A2A input-required tasks must be answered on the same taskId and contextId
- A2A messagesend — *references* — A2A-to-MCP bridge is an MCP stdio server that is also an A2A client
- Task envelope — *is related to* — A2A input-required tasks must be answered on the same taskId and contextId

%% ai-graph-end %%