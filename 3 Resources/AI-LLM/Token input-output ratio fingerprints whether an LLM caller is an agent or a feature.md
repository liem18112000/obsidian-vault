---
title: "Token input-output ratio fingerprints whether an LLM caller is an agent or a feature"
created: 2026-09-27
type: lesson
status: seedling
source: "Confluence: INC-2026-09-24 Gemini consumption (LUZ)"
tags: [llm, vertex-ai, gemini, forensics, cost, observability, confluence-distilled]
---

# Token input-output ratio fingerprints whether an LLM caller is an agent or a feature

When you find unexplained LLM spend and the logs do not say who made the calls, the **shape** of the token usage still tells you what kind of caller it was. The input:output ratio is the strongest signal.

From an investigation into 118.7 M tokens across 28,034 invocations in one seven-hour window:

| Model | Input tokens | Output tokens | Ratio |
|---|--:|--:|--:|
| `gemini-3.8-flash` | 63,627,458 | 2,076,433 | ~31:1 |
| `gemini-3.7-flash` | 19,245,448 | 1,132,497 | ~17:1 |
| `gemini-3.1-pro-preview` | 14,056,857 | 3,625,021 | ~4:1 |

**An agentic coding tool** re-sends a large context on every turn to get a short answer — roughly thirty times more input than output. That is the signature above.

**A product feature serving end users** looks the opposite, and the report lists four discriminators worth memorising:

1. **Inverse ratio** — user-facing generation produces proportionally more output.
2. **One model, not twelve** — a shipped feature pins a model id; it does not enumerate the catalogue.
3. **No startup sweep** — the window opened with one to two calls against each of twelve model ids. That is something *probing what is available*, not something serving traffic.
4. **Recurring daily pattern, not one burst** — a feature used by customers shows up every day. This project had **zero** Vertex AI usage on any of the preceding 29 days.

> [!tip] Read the sweep as intent
> A model-enumeration sweep at the start of a window is close to conclusive: production code does not go looking for which models it can reach. Combined with "no usage for 29 days, then a seven-hour burst", the traffic is interactive and exploratory rather than programmatic and scheduled.

> [!warning] Fingerprinting narrows the class of caller, never the person
> This technique told the investigators *what kind of thing* made the calls. It cannot tell them *who*, and the report is careful about that distinction — the calls carried no identity at all. Use the fingerprint to direct the investigation, then fix the logging gap that made attribution impossible. See [[GCP Data Access logs are off by default, so data-plane calls are unattributable]].

The reusable habit: **before asking "who did this", ask "what shape is this".** Aggregate metrics you already have — token counts, model mix, call timing — often identify the tool class even when per-call identity is missing entirely.

Source: [[INC-2026-09-24 - posapp-android-ed1bd - Gemini consumption with no attributable user]] (LUZ, Confluence).

## Related

- [[GCP Data Access logs are off by default, so data-plane calls are unattributable]]
