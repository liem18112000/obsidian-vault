---
title: "ADK LongRunningFunctionTool HITL nested in SequentialAgent has resume bugs"
created: 2026-09-07
type: lesson
status: seedling
source: "adk-transform export 2026-09-07; adk-python issues #3348/#5349/#3184/#5064"
tags: [google-adk, hitl, gotcha, sequential-agent]
---

# ADK LongRunningFunctionTool HITL nested in SequentialAgent has resume bugs

ADK's human-in-the-loop primitive is **LongRunningFunctionTool** (a tool that returns a *pending* status; the runner pauses; a later turn supplies the tool result — also reachable via `tool_context.request_confirmation()`). But as of adk-python ~1.22 there are **known resume bugs when a long-running/HITL tool is nested inside a SequentialAgent**: sub-agents re-execute on resume, and sub-agent LRO resume can fail outright (adk-python issues #3348, #5349, #3184, #5064).

**Why it matters:** a multi-round human-in-the-loop interrogation (ask → pause → ingest answer → next round) is *exactly* "HITL inside a sequence," so it hits this bug class head-on. 

**Mitigation / safer default:** instead of LRO-in-Sequential, use a **custom BaseAgent that checkpoints its loop state into ADK session `state`, emits the question round, and ends the invocation**; the next turn is a fresh invocation that reads state back and advances. This reproduces the proven "stateless request + rehydrate" shape, sidesteps the resume bugs, and is a smaller conceptual leap. De-risk with a spike before committing, and re-verify against the pinned ADK version — these bugs are version-sensitive.

Related: [[ADK workflow agents orchestrate deterministically without an LLM-driven loop]], [[ADK DatabaseSessionService can subsume a separate A2A task store and state-file rehydration]].

## Related

- [[ADK workflow agents orchestrate deterministically without an LLM-driven loop]]
- [[ADK DatabaseSessionService can subsume a separate A2A task store and state-file rehydration]]

## Update (2026-09-07) — validated on google-adk 2.8.0

The pin `>=1.22` now resolves to **2.8.0** (ADK is on 2.x). The safer default — a custom `BaseAgent`
that checkpoints loop state via `Event(state_delta=…)` and ends the invocation to pause — was run and
**passed**: a 3-round HITL interrogation paused/resumed in order, survived a simulated
`DatabaseSessionService` restart, and re-executed nothing. So this stays the default; the
LongRunningFunctionTool-in-SequentialAgent probe was left model-gated (needs an LLM) and is only worth
adopting if it comes back clean on 2.x. See [[Persist ADK session state from a custom agent via Event state_delta]].
