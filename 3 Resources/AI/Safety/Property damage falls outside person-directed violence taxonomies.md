---
ai_hash: 218eab369afa82dd
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-07-20
entities:
- Property damage
- Person-directed violence taxonomies
- Violence detector
- Violence
- Moderation taxonomies
- OpenAI violence taxonomy
- MLCommons S1 taxonomy
- Person
- Group
- Tire-slashing
- Vandalism
- Claude-judge test
- Taxonomy-definition gap
- Model error
- Test set
- Ground truth
- Prompt
- Spec
- Accuracy
- Taxonomy boundary
- Label ambiguity
- Moderation taxonomies split violence into subtypes
- Violence detection needs a trained classifier, not keyword lists
source: session 2026-07-20
status: seedling
tags:
- content-moderation
- taxonomy
- llm-judge
- gotcha
title: Property damage falls outside person-directed violence taxonomies
type: lesson
---

# Property damage falls outside person-directed violence taxonomies

When building a violence detector, decide up front whether property destruction counts as "violent". Standard moderation taxonomies (OpenAI violence, MLCommons S1) define violence as directed at a **person or group** — so "slit someone's tires" or vandalism is classified NOT violent by a faithful judge, even though it is actionable wrongdoing.

This surfaced as the single disagreement in a 20-case Claude-judge test (C:\Users\dvtliem\AI\ai-test\test_results.md, case #12): the judge returned isViolent=false for tire-slashing, reasoning it targets property not a person. That is defensible, not a bug. Fix by either (a) adding a property-destruction clause to the taxonomy if your policy needs it, or (b) relabeling the ground truth.

Lesson: label ambiguity in a test set is often a taxonomy-definition gap, not a model error — pin the boundary in the prompt/spec before measuring accuracy.

## Related

- [[Moderation taxonomies split violence into subtypes]]
- [[Violence detection needs a trained classifier, not keyword lists]]

%% ai-graph-start %%

**Related notes:**
- [[Moderation taxonomies split violence into subtypes]]
- [[Violence detection needs a trained classifier, not keyword lists]]
- [[Model options for detecting violent text by weight class]]

**Relations:**
- Property damage — *falls outside* — Person-directed violence taxonomies
- Violence detector — *considers* — Property damage
- Moderation taxonomies — *define* — Violence
- Violence — *is directed at* — Person
- Violence — *is directed at* — Group
- OpenAI violence taxonomy — *is a type of* — Moderation taxonomies
- MLCommons S1 taxonomy — *is a type of* — Moderation taxonomies
- Tire-slashing — *is classified as NOT violent by* — Moderation taxonomies
- Vandalism — *is classified as NOT violent by* — Moderation taxonomies
- Tire-slashing — *targets* — Property damage
- Claude-judge test — *revealed disagreement on* — Tire-slashing
- Taxonomy-definition gap — *causes* — Label ambiguity
- Label ambiguity — *occurs in* — Test set
- Taxonomy-definition gap — *is not* — Model error
- Taxonomy boundary — *should be defined in* — Prompt
- Taxonomy boundary — *should be defined in* — Spec
- Defining Taxonomy boundary — *precedes* — Measuring accuracy
- Moderation taxonomies — *can include* — Property damage
- Ground truth — *can be relabeled* — null
- Moderation taxonomies split violence into subtypes — *is related to* — Moderation taxonomies
- Violence detection needs a trained classifier, not keyword lists — *is related to* — Violence detector

%% ai-graph-end %%