---
ai_hash: 69baf3dd78955f28
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-07-20
entities:
- KoalaAI/Text-Moderation
- DeBERTa-v3 encoder
- OpenAI-style labels
- V
- V2
- HR
- Detoxify
- threat
- Llama-Guard-3-1B
- Llama-Guard-8B
- LLM-based safeguard
- MLCommons 13-hazard taxonomy
- OpenAI moderation endpoint
- Google Perspective API
- THREAT
- Azure Content Safety
- severity levels
- violence_detector.py
- Violence detection needs a trained classifier, not keyword lists
- Moderation taxonomies split violence into subtypes
- Violent Text Detection
- CPU
- English-only
- prompts
- responses
- target domain
- labeled examples
- threshold
- toxicity classifier
- violence taxonomy
- OpenAI
- Google
- Azure
- MLCommons
source: web research session 2026-07-20
status: seedling
tags:
- content-moderation
- huggingface
- llm-safety
- models
title: Model options for detecting violent text by weight class
type: howto
---

# Model options for detecting violent text by weight class

Options for classifying violent text, lightest to heaviest:

- **KoalaAI/Text-Moderation** — DeBERTa-v3 encoder, ~180MB, runs locally on CPU, OpenAI-style labels (V=violence, V2=graphic violence, HR=harassment/threat). English-only. Good minimal default.
- **Detoxify** — toxicity classifier with a `threat` label; narrower than a violence taxonomy.
- **Llama-Guard-3-1B / -8B** — LLM-based safeguard, MLCommons 13-hazard taxonomy, classifies both prompts and responses; heavier but more nuanced and policy-promptable.
- **Hosted**: OpenAI moderation endpoint (free, per-category scores), Google Perspective API (THREAT attribute), Azure Content Safety (severity levels).

Whichever is chosen, the threshold must be calibrated on labeled examples from the target domain — defaults transfer badly. Example implementation: C:\Users\dvtliem\AI\ai-test\violence_detector.py

## Related

- [[Violence detection needs a trained classifier, not keyword lists]]
- [[Moderation taxonomies split violence into subtypes]]

%% ai-graph-start %%

**Related notes:**
- [[Moderation taxonomies split violence into subtypes]]
- [[Violence detection needs a trained classifier, not keyword lists]]
- [[Property damage falls outside person-directed violence taxonomies]]

**Relations:**
- KoalaAI/Text-Moderation — *is an option for* — Violent Text Detection
- KoalaAI/Text-Moderation — *uses* — DeBERTa-v3 encoder
- KoalaAI/Text-Moderation — *has size* — ~180MB
- KoalaAI/Text-Moderation — *runs on* — CPU
- KoalaAI/Text-Moderation — *supports language* — English-only
- KoalaAI/Text-Moderation — *provides* — OpenAI-style labels
- OpenAI-style labels — *includes* — V
- OpenAI-style labels — *includes* — V2
- OpenAI-style labels — *includes* — HR
- KoalaAI/Text-Moderation — *is described as* — lightest
- KoalaAI/Text-Moderation — *is described as* — Good minimal default
- Detoxify — *is an option for* — Violent Text Detection
- Detoxify — *is a* — toxicity classifier
- Detoxify — *has label* — threat
- threat — *is* — narrower than a violence taxonomy
- Llama-Guard-3-1B — *is an option for* — Violent Text Detection
- Llama-Guard-3-1B — *is a type of* — LLM-based safeguard
- Llama-Guard-8B — *is an option for* — Violent Text Detection
- Llama-Guard-8B — *is a type of* — LLM-based safeguard
- LLM-based safeguard — *uses* — MLCommons 13-hazard taxonomy
- LLM-based safeguard — *classifies* — prompts
- LLM-based safeguard — *classifies* — responses
- LLM-based safeguard — *is described as* — heavier
- LLM-based safeguard — *is described as* — more nuanced
- LLM-based safeguard — *is described as* — policy-promptable
- MLCommons 13-hazard taxonomy — *is developed by* — MLCommons
- OpenAI moderation endpoint — *is an option for* — Violent Text Detection
- OpenAI moderation endpoint — *is provided by* — OpenAI
- OpenAI moderation endpoint — *is* — free
- OpenAI moderation endpoint — *provides* — per-category scores
- OpenAI moderation endpoint — *is described as* — hosted
- Google Perspective API — *is an option for* — Violent Text Detection
- Google Perspective API — *is provided by* — Google
- Google Perspective API — *has attribute* — THREAT
- Google Perspective API — *is described as* — hosted
- Azure Content Safety — *is an option for* — Violent Text Detection
- Azure Content Safety — *is provided by* — Azure
- Azure Content Safety — *provides* — severity levels
- Azure Content Safety — *is described as* — hosted
- threshold — *must be calibrated on* — labeled examples
- labeled examples — *from* — target domain
- violence_detector.py — *is an example implementation for* — Violent Text Detection
- Violent Text Detection — *is related to* — Violence detection needs a trained classifier, not keyword lists
- Violent Text Detection — *is related to* — Moderation taxonomies split violence into subtypes

%% ai-graph-end %%