---
title: "implement_plan heuristic-fallback emits one performance stub per node"
created: 2026-09-16
type: lesson
status: seedling
source: "testing-agent run-e778a050, LUZ-158230, 2026-09-16"
tags: [testing-agent, implement-plan, assured-loop, vertex, gotcha, fallback]
---

# implement_plan heuristic-fallback emits one performance stub per node

Symptom: the deployed testing-agent `implement_plan` returns `[state: done]` BELOW BAR with a tiny assured-loop score (~0.04-0.05 vs 0.70 threshold) and the note "generation degraded to the heuristic fallback (LLM timed out / unconfigured)".

What the fallback produces: instead of real Given/When/Then scenarios, the heuristic generator emits **one generic scenario per pack node**, all labelled `[performance]` with the body "Exercise the performance case for X via api" - no preconditions, no actions, no oracle. Because it iterates pack nodes, it also pulls in OUT-OF-SCOPE / unrelated nodes (e.g. the Agentic-Framework S1/S2/S3 tickets, attachment images, whole PDFs) as fake test targets. Coverage matrix can still show 100% AC x kind (39/39 cells) because the stub touches every cell nominally - do NOT trust that number when the fallback fired.

Why re-running does not help: the assured loop (generate -> judge -> gate -> reflect -> regenerate) cannot recover, because the DEFECT is the generator running its no-LLM heuristic path, not the guidance. The judge critique will be accurate but the next round degrades identically (saw 2 consecutive identical degradations on run-e778a050, LUZ-158230). Sending guidance= only helps when the LLM is actually generating.

Root cause = the LLM/Vertex generation path is unavailable on the deployed implement (times out or unconfigured). Related known trigger: overrunning Vertex max_tokens, and serial blocking Vertex calls hitting Cloud Run timeouts.

Fix / workaround when it fires: (1) author the real BDD scenarios client-side from the confirmed plan and render to a versioned HTML artifact (the reliable path), and/or (2) fix the deployed generation (Vertex config / max_tokens / async) and redeploy. A single retry is worth trying only to rule out a transient Vertex timeout; beyond that, stop retrying.

Related: [[LUZ-158230 ePost ZIP import - test scope decisions]] · [[Testing-agent refine flags low confidence when spec PDFs are recorded-only]]

## Related

- [[LUZ-158230 ePost ZIP import - test scope decisions]]
- [[Testing-agent refine flags low confidence when spec PDFs are recorded-only]]
