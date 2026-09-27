---
ai_hash: cf44d56cd0f7aac2
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
- codegraph attach
- gather_knowledge
- LUZ-156281 dunning demo
- axonivy-prod/luz_store
- luz_store
- gather_codebase needs axonivy-prod/<repo> workspace slug
- codegraph
- repo parameter
- refine
- tools
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
- MCP gateway — *routes* — tools
- MCP gateway — *routes by* — context_id
- gather_codebase — *is routed to KGA refine loop when* — interactive refine session is active on context_id
- gather_codebase — *returns* — refine message
- gather_codebase — *does not return* — codegraph build result
- codegraph attach — *fails during* — interactive refine session
- codegraph grounding — *should happen before* — refine
- gather_knowledge — *can include* — repo parameter
- gather_codebase — *can be called before* — refine
- codegraph — *can use* — separate context_id
- gather_codebase — *returned refine completion text in* — LUZ-156281 dunning demo
- luz_store — *was not grounded in* — LUZ-156281 dunning demo
- gather_codebase — *is related to* — gather_codebase needs axonivy-prod/<repo> workspace slug

%% ai-graph-end %%