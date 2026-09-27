---
title: "gather_codebase on a mid-refine context routes into the refine loop"
created: 2026-09-13
type: lesson
status: seedling
source: "session 2026-09-13 LUZ-156281 demo"
tags: [kga, refine, codegraph, routing, test-agent-v2, gotcha]
---

# gather_codebase on a mid-refine context routes into the refine loop

In test-agent-v2, the MCP gateway routes tools by `context_id`. If you call `gather_codebase(context_id=X, repo=...)` while an interactive **refine** session is already active on context X, the call is routed into the KGA refine loop instead of building the codegraph — the response comes back as a refine message (e.g. "Refinement complete (confidence: low). N insights, M gaps") rather than a codegraph build result.

**Consequence:** you cannot interleave a fresh `gather_codebase` into a context that is mid-refine; the codegraph attach silently does not happen. Do codegraph grounding BEFORE starting refine (pass `repo=` on the initial `gather_knowledge`, or call `gather_codebase` before the first `refine`), or use a separate context for the codegraph.

Seen 2026-09-13 running the LUZ-156281 dunning demo: after several refine rounds, `gather_codebase(run-88ec187f, axonivy-prod/luz_store)` returned the refine completion text and luz_store was not grounded.

## Related

- [[gather_codebase needs axonivy-prod/<repo> workspace slug]]
