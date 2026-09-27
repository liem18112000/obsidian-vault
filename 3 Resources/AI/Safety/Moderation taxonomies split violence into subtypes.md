---
ai_hash: 589341c4dbe7565c
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-07-20
entities:
- Moderation taxonomies
- Violence
- Subtypes
- OpenAI moderation API
- violence (OpenAI)
- violence/graphic (OpenAI)
- harassment/threatening (OpenAI)
- Llama Guard 3
- MLCommons 13-hazard taxonomy
- S1 (MLCommons)
- violent crimes
- terrorism
- murder
- assault
- kidnapping
- violence toward animals
- Perspective API
- THREAT attribute
- Detector
- per-subtype scores
- general violence
- graphic violence
- direct threat
- Violence detection
- trained classifier
- keyword lists
source: web research session 2026-07-20
status: seedling
tags:
- content-moderation
- taxonomy
- llm-safety
title: Moderation taxonomies split violence into subtypes
type: concept
---

# Moderation taxonomies split violence into subtypes

Serious moderation systems never treat "violence" as one label — they split it into subtypes because the right action differs per subtype. OpenAI moderation API distinguishes `violence` (glorification/support of violent acts, threats) from `violence/graphic` (explicit gore/injury detail); it also has harassment/threatening. Llama Guard 3 follows the MLCommons 13-hazard taxonomy where S1 = violent crimes (terrorism, murder, assault, kidnapping, violence toward animals). Perspective API exposes a THREAT attribute.

When building a detector, mirror this: report per-subtype scores (e.g. general violence vs graphic violence vs direct threat) so callers can apply different thresholds/policies per subtype.

## Related

- [[Violence detection needs a trained classifier, not keyword lists]]

%% ai-graph-start %%

**Related notes:**
- [[Violence detection needs a trained classifier, not keyword lists]]
- [[Model options for detecting violent text by weight class]]
- [[Property damage falls outside person-directed violence taxonomies]]

**Relations:**
- Moderation taxonomies — *split* — Violence
- Violence — *has* — Subtypes
- OpenAI moderation API — *distinguishes* — violence (OpenAI)
- OpenAI moderation API — *distinguishes* — violence/graphic (OpenAI)
- OpenAI moderation API — *includes* — harassment/threatening (OpenAI)
- Llama Guard 3 — *follows* — MLCommons 13-hazard taxonomy
- MLCommons 13-hazard taxonomy — *defines* — S1 (MLCommons)
- S1 (MLCommons) — *is* — violent crimes
- violent crimes — *includes* — terrorism
- violent crimes — *includes* — murder
- violent crimes — *includes* — assault
- violent crimes — *includes* — kidnapping
- violent crimes — *includes* — violence toward animals
- Perspective API — *exposes* — THREAT attribute
- Detector — *reports* — per-subtype scores
- per-subtype scores — *include* — general violence
- per-subtype scores — *include* — graphic violence
- per-subtype scores — *include* — direct threat
- Violence detection — *needs* — trained classifier
- Violence detection — *does not need* — keyword lists

%% ai-graph-end %%