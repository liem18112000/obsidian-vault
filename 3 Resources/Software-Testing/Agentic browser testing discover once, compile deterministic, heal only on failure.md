---
ai_hash: 19cc47ca09576e42
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-03
entities: []
source: deep research 2026-09-03
status: seedling
tags:
- testing
- browser-automation
- playwright
- self-healing
- agentic
title: 'Agentic browser testing: discover once, compile deterministic, heal only on
  failure'
type: concept
---

# Agentic browser testing: discover once, compile deterministic, heal only on failure

The winning production pattern for agentic UI/browser testing separates two phases instead of letting an LLM drive every run:

1. **Discover (agentic, once)** — an LLM/vision agent explores the app and finds a working path (browser-use, Skyvern, Playwright Planner/Generator).
2. **Compile** — emit that path as **deterministic** Playwright/Selenium code, or cache the resolved actions.
3. **Replay (deterministic, many)** — run the compiled test cheaply in CI with auto-waiting, retries, and traces.
4. **Heal (agentic, on failure only)** — when a deterministic step breaks, re-engage the agent to inspect the current UI, patch the locator/wait/data, and re-compile.

**Why it matters:** pure-agentic execution is adaptive but slow, costly, and non-deterministic; pure-deterministic is fast but brittle. This pattern keeps AI out of the hot path (Octomind: "AI doesn`t belong in test runtime") while gaining adaptivity exactly where it pays — authoring and healing. **Guardrail:** surface every self-heal for human review; a silent retarget to the wrong element can mask a real regression. Grounding choices: accessibility-tree (compact, stable) vs DOM-indexed vs vision (canvas/cross-origin).

## Related

- [[Metamorphic and differential testing solve the oracle problem]]

%% ai-graph-start %%

**Related notes:**
- [[Assured test generation keep an LLM test only if it builds, passes, and raises coverage]]
- [[Metamorphic and differential testing solve the oracle problem]]
- [[AI as an accelerator with a human review gate]]
- [[LLM-picked UI actions can be verified mechanically but not semantically]]
- [[Self-healing scraper selectors — LLM fallback only on verified failure, then cache]]

%% ai-graph-end %%