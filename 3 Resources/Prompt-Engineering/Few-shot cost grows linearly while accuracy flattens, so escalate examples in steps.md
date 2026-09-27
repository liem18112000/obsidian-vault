---
ai_hash: 4176a94b81958eb9
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-27
entities: []
source: 'Confluence: Manual Prompt Compression Techniques - Deep Dive (2026-04-13)'
status: seedling
tags:
- prompt-engineering
- few-shot
- llm
- cost-optimization
- token-budget
title: Few-shot cost grows linearly while accuracy flattens, so escalate examples
  in steps
type: lesson
---

# Few-shot cost grows linearly while accuracy flattens, so escalate examples in steps

Few-shot examples cost tokens **linearly** while accuracy gains **flatten** — so past a certain point every added example is pure cost. On strong models the point arrives much earlier than the folklore suggests:

- **Zero-shot often works** outright (Claude Sonnet 4, Gemini 2.5 Pro).
- **1–2 examples match or beat 5+** on many tasks.
- **Example quality > example quantity.**

The escalation ladder, cheapest rung first:

```
Can zero-shot handle it?
  YES → zero-shot
  NO  → 1–2 high-quality examples
          still failing? → 3–5 diverse examples
                still failing? → structured output / tool_use instead
```

That last rung is the one people skip. **If five examples have not taught the format, the problem is not the examples — it is that you are teaching a schema through prose.** A JSON schema or a tool definition states the constraint directly, and the model cannot drift from it the way it drifts from demonstrated patterns.

Examples themselves compress hard. Stripping verbose input text and the reasoning narrative — keeping only `In: … Out: …` — cut one example from **80 to 25 tokens (69%)** with the pattern still intact. The model infers the mapping from the pair; it does not need the prose explaining why.

The same principle applies to chain-of-thought: recent results show **zero-shot CoT ("think step by step") often matches few-shot CoT** on strong models, which saves every example token.

## Related

- [[Thinking tokens are billed as output, so effort level is a cost lever]]

## Related

- [[Thinking tokens are billed as output, so effort level is a cost lever]]

%% ai-graph-start %%

**Related notes:**
- [[Thinking tokens are billed as output, so effort level is a cost lever]]
- [[Route tool-less LLM passes to a cheaper model tier]]
- [[Tier LLM effort per pipeline stage - pay where quality compounds, cut where the task is bounded]]
- [[LLM query enrichment for a substring-OR matcher must contract, not expand, the token set]]
- [[Calibrate a cheap-model to LLM cascade threshold using the LLM judge as oracle]]

%% ai-graph-end %%