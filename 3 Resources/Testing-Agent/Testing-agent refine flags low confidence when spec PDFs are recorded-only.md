---
title: "Testing-agent refine flags low confidence when spec PDFs are recorded-only"
created: 2026-09-16
type: lesson
status: seedling
source: "testing-agent run-e778a050, 2026-09-16"
tags: [testing-agent, refine, gotcha, knowledge-gathering]
---

# Testing-agent refine flags low confidence when spec PDFs are recorded-only

In the testing-agent pipeline, the `refine` interrogation attaches an AI self-assessment with a confidence score. When the source spec PDFs are present in the pack only as **recorded-only attachment links** (never parsed/ingested into the pack body), confidence drops to ~0.30 and the per-question recommendations become explicitly ungrounded — the engine is inferring from the ticket text + prior-run memory, not from the actual spec.

Signal to watch: the assessment says the field table is "truncated mid-row" and the PDFs are "only present as unresolved attachment links in recorded-only". 

Implication for the client: do not trust modeling/validation/atomicity recommendations at face value in that state — bring them to the human, and/or ingest the real PDF bodies (and linked sub-tickets / sample PRs) before finalizing. Concrete case: LUZ-158230, spec PDFs attachment 316500/316499/316992/317020 stayed recorded-only, so both the business (0.32) and technical (0.30) rounds self-flagged low confidence.

Related: [[LUZ-158230 ePost ZIP import - test scope decisions]]
