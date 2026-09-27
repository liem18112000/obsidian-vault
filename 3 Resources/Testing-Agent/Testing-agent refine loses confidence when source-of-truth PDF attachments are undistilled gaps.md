---
ai_hash: 3f46a2b65abbab59
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-15
entities: []
source: testing-agent run run-87933300, 2026-09-15
status: seedling
tags:
- testing-agent
- refine
- jira
- attachments
- gotcha
- grounding
title: Testing-agent refine loses confidence when source-of-truth PDF attachments
  are undistilled gaps
type: lesson
---

# Testing-agent refine loses confidence when source-of-truth PDF attachments are undistilled gaps

When running the **testing-agent** pipeline (`gather_knowledge` → `refine` → `define_plan`) on a Jira ticket whose real spec lives in **PDF/Office attachments**, the `refine`/`define_plan` interrogation can come back at **low confidence (~0.30)** even though it produced questions and insights. Root cause: the source-of-truth attachments (for LUZ-158230: the ZIP-import spec PDF + "Health documents development handoff" PDF) were flagged as **gaps / not distilled** into the context pack, so the AI assessment explicitly warned its business/technical/scope recommendations were **ungrounded guesses** inferred from followed-but-unquoted links.

Signals to watch:
- `--- AI assessment (confidence 0.3x) ---` with lines like "the actual metadata.json schema … are not visible in the pack itself" and "focus next: retrieve and read the two attached PDFs in full."
- `evaluate_pack` can still report `recall=1.00, faithfulness=1.00, precision=0.00, leaked=[]` — that is "complete-but-noisy," a *retrieval* signal, and does **not** contradict the low *interrogation* confidence caused by undistilled attachments.

What to do: read the attachment PDFs and/or inspect the merged Bitbucket PRs before trusting the tool's recommendations; drive the real business/technical answers from that grounded reading (the user overrode several defaults on LUZ-158230). Even with the attachment-reading feature shipped, attachments still land as gaps sometimes.

## Related
[[LUZ-158230 is the ePost eArchive transfer.zip document-import feature]]

## Related

- [[LUZ-158230 is the ePost eArchive transfer.zip document-import feature]]

%% ai-graph-start %%

**Related notes:**
- [[Testing-agent refine flags low confidence when spec PDFs are recorded-only]]
- [[Testing-Agent refine confidence is capped by un-ingested spec PDFs]]
- [[Deployed Testing-Agent refine recommendations are speculative until validated]]
- [[testing-agent implement_plan generates scenarios per pack-node x 4 kinds, amplifying pack noise and ignoring non-functional-kind guidance]]
- [[test-agent-v2 gather fixes read attachments (PDFOfficeimage) + cloud-discover relevance gate]]

%% ai-graph-end %%