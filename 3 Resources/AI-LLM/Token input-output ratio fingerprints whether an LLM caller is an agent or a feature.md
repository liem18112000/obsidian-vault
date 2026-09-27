---
ai_hash: dce65b0721670950
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-27
entities:
- Token input-output ratio
- LLM caller
- Agent
- Feature
- LLM spend
- Logs
- Token usage
- Investigation
- Tokens
- Invocations
- Time window
- gemini-3.8-flash
- gemini-3.7-flash
- gemini-3.1-pro-preview
- Agentic coding tool
- Product feature
- End users
- Inverse ratio
- Model ID
- Startup sweep
- Recurring daily pattern
- Burst
- Vertex AI usage
- Project
- Model-enumeration sweep
- Production code
- Attribution
- Fingerprinting
- Class of caller
- Person
- Identity
- Logging gap
- GCP Data Access logs
- Data-plane calls
- Aggregate metrics
- Token counts
- Model mix
- Call timing
- INC-2026-09-24 - posapp-android-ed1bd - Gemini consumption with no attributable
  user
- LUZ
- Confluence
- GCP Data Access logs are off by default, so data-plane calls are unattributable
- Exploratory behavior
source: 'Confluence: INC-2026-09-24 Gemini consumption (LUZ)'
status: seedling
tags:
- llm
- vertex-ai
- gemini
- forensics
- cost
- observability
- confluence-distilled
title: Token input-output ratio fingerprints whether an LLM caller is an agent or
  a feature
type: lesson
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

%% ai-graph-start %%

**Related notes:**
- [[INC-2026-09-24 - posapp-android-ed1bd - Gemini consumption with no attributable user]]
- [[GCP Data Access logs are off by default, so data-plane calls are unattributable]]
- [[Shared and personal accounts make attribution impossible by construction]]
- [[Vertex AI Claude usage query - klara-nonprod]]

**Relations:**
- Token input-output ratio — *fingerprints* — LLM caller
- LLM caller — *can_be* — Agent
- LLM caller — *can_be* — Feature
- Token usage — *reveals* — Class of caller
- Investigation — *analyzed* — Tokens
- Investigation — *analyzed* — Invocations
- Investigation — *occurred_in* — Time window
- gemini-3.8-flash — *has_input_output_ratio* — 31:1
- gemini-3.7-flash — *has_input_output_ratio* — 17:1
- gemini-3.1-pro-preview — *has_input_output_ratio* — 4:1
- Agentic coding tool — *is_a_type_of* — Agent
- Agentic coding tool — *has_input_output_ratio* — 30:1
- Product feature — *is_a_type_of* — Feature
- Product feature — *serves* — End users
- Product feature — *is_characterized_by* — Inverse ratio
- Product feature — *pins* — Model ID
- Product feature — *avoids* — Startup sweep
- Product feature — *is_characterized_by* — Recurring daily pattern
- Startup sweep — *is_a_type_of* — Model-enumeration sweep
- Model-enumeration sweep — *indicates* — Exploratory behavior
- Production code — *does_not_perform* — Model-enumeration sweep
- Project — *experienced* — Burst
- Project — *had_no* — Vertex AI usage
- Fingerprinting — *narrows* — Class of caller
- Fingerprinting — *does_not_identify* — Person
- Calls — *lack* — Identity
- Fingerprinting — *directs* — Investigation
- Logging gap — *prevents* — Attribution
- GCP Data Access logs — *affect* — Data-plane calls
- Data-plane calls — *are* — unattributable
- Aggregate metrics — *include* — Token counts
- Aggregate metrics — *include* — Model mix
- Aggregate metrics — *include* — Call timing
- Aggregate metrics — *identify* — Class of caller
- INC-2026-09-24 - posapp-android-ed1bd - Gemini consumption with no attributable user — *authored_by* — LUZ
- INC-2026-09-24 - posapp-android-ed1bd - Gemini consumption with no attributable user — *published_on* — Confluence
- INC-2026-09-24 - posapp-android-ed1bd - Gemini consumption with no attributable user — *references* — GCP Data Access logs are off by default, so data-plane calls are unattributable

%% ai-graph-end %%