---
title: "Deployed Testing-Agent refine loop freezes after completion and drops corrections"
created: 2026-09-14
type: lesson
status: seedling
source: "session 2026-09-14 run-cd156028"
tags: [testing-agent, refine, gotcha, persistence, luz-158230]
---

# Deployed Testing-Agent refine loop freezes after completion and drops corrections

The deployed Testing-Agent `refine` loop is single-shot: once it reports `Refinement complete`, it is closed. Calling `refine(context_id, answer=...)` again does **not** reopen it or ingest the correction — it returns a generic `[state: completed] Provide a seed, e.g. 'gather ...'` message and the restated understanding brief stays frozen.

Worse, on run `run-cd156028` (LUZ-158230) the brief **contradicted a confirmed interrogation answer**: Q-tec-005 was answered *partial-import* (commit valid docs, report per-doc failures), but the brief asserted *all-or-nothing atomic batch*. And `approve` reported "No questions yet" — the Q&A was never recorded on the pack at all.

This persists on the live agent even though repo commit `40e3a89` ("persist interrogation answers + A2A task store on Cloud SQL") claims to fix persistence — that fix is either undeployed or doesn't cover the post-completion re-refine path.

**Workaround:** don't trust the brief to reflect your answers. Approve the skeleton and carry every correction downstream yourself when authoring the plan/scope/scenarios client-side. An empty "No questions" Q&A record is a false-negative smell, not a clean pack.

Related: [[define_plan free-text answer not persisted to structured brief]], [[Testing-Agent interrogation must run on rich input or it asks nothing]]

## Related

- [[define_plan free-text answer not persisted to structured brief]]
- [[Testing-Agent interrogation must run on rich input or it asks nothing]]
