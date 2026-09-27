---
ai_hash: 24076e7b8670ae76
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-16
entities: []
source: Testing-Agent run-188f96b8
status: seedling
tags:
- testing-agent
- implement-plan
- assured-loop
- gotcha
- scenario-generator
title: Testing-Agent implement_plan silent heuristic fallback = per-node x kind empty-step
  scenarios
type: lesson
---

# Testing-Agent implement_plan silent heuristic fallback = per-node x kind empty-step scenarios

On Testing-Agent run **run-188f96b8** (LUZ-158230), the deployed `implement_plan` assured loop produced **164 scenarios but scored 0.08 vs bar 0.70** and stayed frozen across two rounds. Inspecting the output revealed it was a **silent heuristic fallback**, not real generation:

- 164 = **41 pack nodes × 4 kinds** (happy/negative/boundary/error, all `[.../api]`). Every scenario is titled by a PACK NODE — e.g. "Self-check — happy path", "Verify on DEV — boundary", "ZIP import specification.pdf — error handling" — including subtask tickets and PDF attachments.
- Each scenario's **`Steps:` section is EMPTY**; the `data:` field just dumps *every* pack node as `mock` test-data.
- **None** of the requested extra kinds (security / concurrency / i18n) were generated, and **none** of the client-side steering `guidance=` (per-behavior grounding, end-state oracles, fixture shapes) shaped the output. The LLM generation did not run — consistent with transient `Error executing tool` calls + a likely max_tokens/heuristic fallback.

**Smell test:** scenario titles following a `<pack-node> — <kind>` pattern + empty Steps = the silent fallback. The 0.08 score is accurate; more assured rounds won't move it (root cause is the generator, not steering). 

**Action:** treat the deployed generator output as unusable in this mode; author the real behavior×kind scenarios client-side from the confirmed plan (same conclusion as the "thin pack / refine is speculative" lessons). Relates to the known "scenario-generator overran Vertex max_tokens -> silent heuristic fallback" gotcha.

## Related

- [[Deployed Testing-Agent refine recommendations are speculative until validated]]

%% ai-graph-start %%

**Related notes:**
- [[implement_plan heuristic-fallback emits one performance stub per node]]
- [[testing-agent implement_plan generates scenarios per pack-node x 4 kinds, amplifying pack noise and ignoring non-functional-kind guidance]]
- [[Deployed implement_plan P4 assured loop times out at 900s for broad features]]
- [[Testing-Agent implement_plan assured loop times out at 900s MCP ceiling]]
- [[Fix TPD scenario generator truncation — raise max_tokens, keep one call]]

%% ai-graph-end %%