---
ai_hash: 924d68213e24c750
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-15
entities: []
source: LUZ-158230 run 2026-09-15
status: seedling
tags:
- testing-agent
- kga
- refine
- groundedness
- gotcha
title: Testing-Agent refine confidence is capped by un-ingested spec PDFs
type: lesson
---

# Testing-Agent refine confidence is capped by un-ingested spec PDFs

In the Testing-Agent (KGA) `refine` loop, the reported **confidence** (and the per-round AI-assessment score, ~0.30) is a **groundedness** measure — how much of the pack is backed by verifiable source text — NOT a function of how many interrogation rounds you run.

**Consequence:** if the real source-of-truth is a **binary attachment (PDF/DOCX)** on Jira/SharePoint, the crawler records it as a *recorded-only attachment* but does not extract its text, so it never enters the pack. Confidence then stays pinned at "low" no matter how many extra refine rounds you push. Observed on LUZ-158230: 5 rounds, growing the pack from 38 to 135 in-scope links (added sibling tickets + Confluence pages), left confidence at 0.30 — every rounds assessment repeated the same complaint ("field table cuts off mid-row at documentTitle").

**What actually raises it:** get the spec text INTO the pack — paste/gather the metadata.json field schema, or fetch the PDF via a connector that can read it (e.g. SharePoint via M365) and gather that text. Codegraph grounding does NOT help here: it adds STRUCTURE, not spec/behaviour depth (see [[KGA plan vs human plan grounding depth]] if present).

**Note:** low confidence does not mean the pack is wrong. On LUZ-158230 the Pack Quality Score was 0.75 with faithfulness=1.00 and ctx_recall=1.00 (precision=0.00 = complete-but-noisy) — the decisions were answer-grounded and safe to proceed on; "low" only flagged the missing spec text.

## Related

- [[LUZ-158230 Post Health ZIP import v1 scope decisions]]

## Addendum — the restatement drifts AGAINST fed corrections

Beyond the pinned confidence: the `refine` **restated understanding** is itself unreliable. Facts fed via `refine(answer=...)` do NOT anchor it — the engine regenerates the brief from the thin crawled Jira node plus its own priors and can **invert** a spec fact. Observed on LUZ-158230 after feeding the confirmed v1.0 spec: the brief claimed `documentTitle` is *mandatory with no fallback* (spec: fallback = filename without extension, no field required) and invented an *in-service SNOMED validation table* (spec: no validation, accept as-is). Lesson: do NOT let `define_plan` consume the server brief for substance when the real spec is a binary attachment — author the plan/scenarios CLIENT-SIDE from the spec (see [[LUZ-158230 ePost ZIP import spec v1.0 (authoritative)]]).

%% ai-graph-start %%

**Related notes:**
- [[Testing-agent refine loses confidence when source-of-truth PDF attachments are undistilled gaps]]
- [[Testing-agent refine flags low confidence when spec PDFs are recorded-only]]
- [[Deployed Testing-Agent refine recommendations are speculative until validated]]
- [[testing-agent implement_plan generates scenarios per pack-node x 4 kinds, amplifying pack noise and ignoring non-functional-kind guidance]]
- [[test-agent-v2 gather fixes read attachments (PDFOfficeimage) + cloud-discover relevance gate]]

%% ai-graph-end %%