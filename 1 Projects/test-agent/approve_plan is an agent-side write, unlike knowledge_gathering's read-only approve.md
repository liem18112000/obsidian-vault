---
ai_hash: c0cebb7ab8060033
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-28
entities:
- approve_plan
- knowledge_gathering
- approve
- agent-side write
- read-only
- MCP approve tool
- get-understanding
- get-questions
- hand-off
- refine loop
- client-side
- Claude
- state-changing A2A message
- test_plan_definition
- TestPlan.status = "confirmed"
- implement stage
- memory-bank WRITE
- bridge
- agent
- A2A
- agent-side operation
- run_approve
- define session
- confirmed brief
- executor router
- approve branch
- live-define-session
- wants_define branch
- A2ABridgeClient
- _BearerASGIMiddleware
- knowledge_gathering.bridge
- tool set
- instructions
- env prefixes (TPD_*)
- default port (8081)
- Testing Agent
- pipeline stage
- knowledge_gathering skeleton
source: session 2026-08-28, test_plan_definition M4
status: seedling
tags:
- test-agent
- a2a
- mcp
- bridge
- design-decision
title: approve_plan is an agent-side write, unlike knowledge_gathering's read-only
  approve
type: lesson
---

# approve_plan is an agent-side write, unlike knowledge_gathering's read-only approve

knowledge_gathering's MCP `approve` tool is READ-ONLY: it just reads get-understanding + get-questions and packages them as the hand-off; the refine loop already persisted everything. So approve lives purely client-side (in Claude) and sends no state-changing A2A message.

test_plan_definition's `approve_plan` is DIFFERENT: it must lock `TestPlan.status = "confirmed"` so the implement stage's gate passes. That is a memory-bank WRITE, and the bridge (client-side) can only reach the bank through the agent over A2A. Therefore approve is an **agent-side operation**: the bridge sends `approve <ctx>`, a new `run_approve` executor route flips the persisted plan to confirmed (+ closes the define session), and the tool returns the confirmed brief.

**Routing gotcha:** the `approve` branch must sit BEFORE the live-define-session / wants_define branch in the executor router, otherwise a still-open define session would swallow "approve <ctx>" as if it were an answer. Explicit gate wins over an in-flight session.

Reuse note: the generic `A2ABridgeClient` and the `_BearerASGIMiddleware` ASGI gate are reused verbatim from knowledge_gathering.bridge across both agents' bridges — only the tool set, instructions, env prefixes (TPD_*), and default port (8081) differ.

## Related

- [[Testing Agent builds each pipeline stage as a package mirroring the knowledge_gathering skeleton]]

%% ai-graph-start %%

**Related notes:**
- [[Testing Agent builds each pipeline stage as a package mirroring the knowledge_gathering skeleton]]
- [[TPD agentic loop single-pass DEFINE to APPROVE to IMPLEMENT]]
- [[Pipeline stages sharing a context_id need separate memory-bank path prefixes]]
- [[test-agent-v2 TPD has five raw-Vertex generators — the ADK LlmAgent conversion targets]]
- [[A2A-to-MCP bridge is an MCP stdio server that is also an A2A client]]

**Relations:**
- approve_plan — *is a* — agent-side write
- approve — *is a* — read-only
- approve — *is part of* — knowledge_gathering
- MCP approve tool — *is* — approve
- MCP approve tool — *reads* — get-understanding
- MCP approve tool — *reads* — get-questions
- MCP approve tool — *packages as* — hand-off
- refine loop — *persisted* — everything
- approve — *lives* — client-side
- approve — *is in* — Claude
- approve — *sends no* — state-changing A2A message
- approve_plan — *is part of* — test_plan_definition
- approve_plan — *must lock* — TestPlan.status = "confirmed"
- TestPlan.status = "confirmed" — *enables* — implement stage
- approve_plan — *is a* — memory-bank WRITE
- bridge — *reaches bank through* — agent
- bridge — *communicates over* — A2A
- approve_plan — *is an* — agent-side operation
- bridge — *sends* — approve <ctx>
- run_approve — *flips* — persisted plan to confirmed
- run_approve — *closes* — define session
- run_approve — *returns* — confirmed brief
- approve branch — *sits before* — live-define-session
- approve branch — *sits before* — wants_define branch
- A2ABridgeClient — *reused from* — knowledge_gathering.bridge
- _BearerASGIMiddleware — *reused from* — knowledge_gathering.bridge
- A2ABridgeClient — *is used across* — both agents' bridges
- _BearerASGIMiddleware — *is used across* — both agents' bridges
- tool set — *differs for* — agents' bridges
- instructions — *differ for* — agents' bridges
- env prefixes (TPD_*) — *differ for* — agents' bridges
- default port (8081) — *differs for* — agents' bridges
- Testing Agent — *builds* — pipeline stage
- pipeline stage — *mirrors* — knowledge_gathering skeleton

%% ai-graph-end %%