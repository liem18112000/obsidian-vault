---
ai_hash: 49934042c5e7ac00
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-27
entities: []
source: 'Confluence: API Gateway Evaluation Discussion (Arrow)'
status: seedling
tags:
- evaluation
- tool-selection
- api-gateway
- decision-making
- confluence-distilled
title: Fix evaluation criteria before looking at candidates, and say which one you
  weight
type: lesson
---

# Fix evaluation criteria before looking at candidates, and say which one you weight

Tool comparisons go wrong when they start with the tools. Whoever demos first sets the vocabulary, and every later candidate gets scored on how well it imitates the first one. Fix the **criteria** first, explicitly as *"something to serve as a baseline to compare against"*, and the comparison becomes decidable.

A criteria set used for an API-gateway evaluation, worth reusing as a template for any infrastructure component:

**Deployment**
- How easy is it to get running?
- How easy to maintain — add/remove/edit services and routing?
- **How easy to integrate with our platform** (here: GCP, Kubernetes, third-party integrations) — flagged as the one to weight most heavily
- Separate or integrated datastore?

**Community support**
- Enough tutorials and documentation?
- Many *unresolved* questions or bugs on GitHub/Stack Overflow?
- Easy to extend with plugins?

**Features** — load balancing · rate limiting · security (specifically: does it work with the identity provider we already run?) · monitoring and analytics, ideally with an admin dashboard · integrations we need

**Performance** — what is it built on top of? published benchmarks?

**Paid support** — is there a free version? what does the enterprise version add? what is the pricing?

**Two things this set does well:**

1. **It weights one criterion openly.** *"Need to focus on this point"* against platform integration. Most evaluations pretend all criteria are equal and then quietly decide on one — saying which one up front lets reviewers challenge the weighting rather than the conclusion.
2. **It names the incumbents.** "Security with support for Keycloak" is not a generic feature question; it asks whether the candidate fits the identity provider already in production. The right question is never "does it do auth?" but "does it do auth *with what we already run?*"

> [!tip] "Unresolved questions" beats "popularity"
> Counting stars measures adoption. Counting **unanswered issues and stale bugs** measures whether you will get help when you are stuck at 2am — a far better predictor of how the tool feels to own.

> [!warning] Write the criteria before the shortlist, and date them
> Criteria written after you have seen the candidates tend to describe the front-runner. If the list changes mid-evaluation, record why — that is usually the moment a real requirement was discovered, and it is worth more than the eventual score table.

Source: [[API Gateway Evaluation Discussion]] (Arrow, Confluence).

%% ai-graph-start %%

**Related notes:**
- [[API Gateway Evaluation Discussion]]
- [[API Gateway Evaluation]]

%% ai-graph-end %%