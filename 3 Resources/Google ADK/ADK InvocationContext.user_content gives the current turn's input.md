---
ai_hash: b1f6ee539b4e2865
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-08
entities: []
source: session 2026-09-08 test-agent-v2 reuse pass
status: seedling
tags:
- google-adk
- adk
- gotcha
- test-agent-v2
- reuse
title: ADK InvocationContext.user_content gives the current turn's input
type: lesson
---

# ADK InvocationContext.user_content gives the current turn's input

In an ADK custom `BaseAgent._run_async_impl(self, ctx)`, the current turn's user message is handed to you directly on the `InvocationContext` as `ctx.user_content` (a `google.genai.types.Content`). `CallbackContext` exposes the same `.user_content`. Do NOT hand-roll a reverse scan of `ctx.session.events` for the last user-role part — that reinvents what the framework already gives you, and it also breaks the moment a parent agent (router / SequentialAgent) delegates via `sub.run_async(ctx)`, because the shared context carries `user_content` but the sub-agent has emitted no new user event.

Gotcha: `types.Content` has no `.text` attribute — iterate `content.parts` and read `part.text`:

```python
def incoming_text(ctx) -> str:
    for part in getattr(getattr(ctx, "user_content", None), "parts", None) or []:
        if getattr(part, "text", None):
            return part.text
    return ""
```

Verified on google-adk 2.8.0 while refactoring test-agent-v2 to maximize framework reuse. The eval-side `Invocation` type also carries `.user_content`, so custom eval metrics recover the seed/context-id the same way.

## Related

- [[test-agent-v2 ADK migration]]

%% ai-graph-start %%

**Related notes:**
- [[BridgeSession turn drops the answer when the A2A task completes each turn]]
- [[ADK LlmAgent with output_schema cannot use tools or transfer to other agents]]
- [[ADK canonical orchestration SequentialAgent, LlmAgent+AgentTool, or callbacks — not custom BaseAgent]]
- [[Persist ADK session state from a custom agent via Event state_delta]]
- [[ADK modeltool callbacks only fire for LlmAgent-mediated calls; use a Runner Plugin for cross-cutting]]

%% ai-graph-end %%