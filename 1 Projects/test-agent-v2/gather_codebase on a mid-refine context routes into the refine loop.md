---
ai_hash: c231589314451098
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-13
entities:
- gather_codebase
- refine loop
- test-agent-v2
- MCP gateway
- context_id
- interactive refine session
- KGA refine loop
- refine message
- codegraph build result
- codegraph
- gather_knowledge
- LUZ-156281 dunning demo
- run-88ec187f
- axonivy-prod/luz_store
- luz_store
- axonivy-prodrepo workspace slug
- refine
- codegraph grounding
source: session 2026-09-13 LUZ-156281 demo
status: seedling
tags:
- kga
- refine
- codegraph
- routing
- test-agent-v2
- gotcha
title: gather_codebase on a mid-refine context routes into the refine loop
type: lesson
---

# gather_codebase on a mid-refine context routes into the refine loop

In test-agent-v2, the MCP gateway routes tools by `context_id`. If you call `gather_codebase(context_id=X, repo=...)` while an interactive **refine** session is already active on context X, the call is routed into the KGA refine loop instead of building the codegraph — the response comes back as a refine message (e.g. "Refinement complete (confidence: low). N insights, M gaps") rather than a codegraph build result.

**Consequence:** you cannot interleave a fresh `gather_codebase` into a context that is mid-refine; the codegraph attach silently does not happen. Do codegraph grounding BEFORE starting refine (pass `repo=` on the initial `gather_knowledge`, or call `gather_codebase` before the first `refine`), or use a separate context for the codegraph.

Seen 2026-09-13 running the LUZ-156281 dunning demo: after several refine rounds, `gather_codebase(run-88ec187f, axonivy-prod/luz_store)` returned the refine completion text and luz_store was not grounded.

## Related

- [[gather_codebase needs axonivy-prodrepo workspace slug|gather_codebase needs axonivy-prod/<repo> workspace slug]]

%% ai-graph-start %%

**Related notes:**
- [[gather_codebase needs axonivy-prodrepo workspace slug]]
- [[KGA gather exclude= takes node ids, not keywords]]
- [[Pipeline stages sharing a context_id need separate memory-bank path prefixes]]
- [[Testing Agent builds each pipeline stage as a package mirroring the knowledge_gathering skeleton]]
- [[Deployed Testing-Agent refine loop freezes after completion and drops corrections]]

**Relations:**
- gather_codebase — *routes into* — refine loop
- test-agent-v2 — *uses* — MCP gateway
- MCP gateway — *routes tools by* — context_id
- gather_codebase — *takes parameter* — context_id
- gather_codebase — *takes parameter* — repo
- gather_codebase — *routes into* — KGA refine loop
- KGA refine loop — *produces* — refine message
- gather_codebase — *normally produces* — codegraph build result
- codegraph attach — *does not happen during* — refine
- codegraph grounding — *should happen before* — refine
- gather_knowledge — *takes parameter* — repo
- LUZ-156281 dunning demo — *observed issue with* — gather_codebase
- gather_codebase — *called with* — run-88ec187f
- gather_codebase — *called with* — axonivy-prod/luz_store
- gather_codebase — *returned* — refine message
- luz_store — *was not* — codegraph grounded
- gather_codebase — *needs* — axonivy-prodrepo workspace slug

%% ai-graph-end %%