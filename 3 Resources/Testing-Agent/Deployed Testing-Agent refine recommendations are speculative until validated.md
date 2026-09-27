---
title: "Deployed Testing-Agent refine recommendations are speculative until validated"
created: 2026-09-16
type: lesson
status: seedling
source: "Testing-Agent run-188f96b8"
tags: [testing-agent, refine, gotcha, knowledge-gathering]
---

# Deployed Testing-Agent refine recommendations are speculative until validated

The deployed Testing-Agent `refine` (and `define_plan`) step interrogates off a **thin context pack** — spec PDFs attached to the Jira ticket are often *not* parsed into its interrogation context, so it reasons from a truncated Jira excerpt.

Symptom: refine returns questions with a low **AI self-assessment confidence** (seen at **0.28–0.30** on run-188f96b8) and a note that the recommendations are "authored in the prompt" rather than grounded in the source PDFs.

**Consequence:** its per-question *recommendations are speculative* and sometimes wrong. On LUZ-158230 the bot recommended "fail entire ZIP on any invalid doc" and "luz_docs_import owns dedup via a stable key" — both contradicted by the domain expert (missing metadata → import with defaults; dedup by import-success history).

**How to work with it:** treat refine recommendations as a *prompt*, not an answer. Ground the real answers in (a) the prior run's confirmed understanding, and (b) the domain-expert user. It also tends to **re-ask** in the technical round what was already answered in the business round — reuse the earlier answers instead of re-bugging the user.

Related project memory: "Interrogation loop asks nothing" and "define_plan answer not persisted".

## Related

- [[ePost ZIP import (LUZ-158230) behavior rules confirmed by domain expert]]
