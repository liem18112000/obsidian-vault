---
title: "test-agent-v2 persists diagram-as-code + get_deliverables tool"
created: 2026-09-21
type: reference
status: seedling
source: "session 2026-09-21"
tags: [test-agent, mermaid, diagrams, deliverables, architecture]
---

# test-agent-v2 persists diagram-as-code + get_deliverables tool

test-agent-v2 (2026-09-21): after retiring agent-side HTML renderers (agent=data-only, client renders), the agent must still PERSIST the full downloadable deliverable set so the client can render + offer downloads. Implemented:
- common/testplan/diagrams.py: deterministic mermaid diagram-as-code from the persisted run — architecture (requirements->SUT->oracle from the coverage matrix), scope (in/out), gaps (dev-to-confirm). Raw .mmd source, no HTML, no LLM. build_diagrams(plan, coverage_dict) -> {name: mermaid}.
- Persisted at implement time (pipeline._build_diagrams, best-effort like _build_coverage) as memory/test-plan/<ctx>/diagrams.json via store.write_diagrams/read_diagrams.
- get_deliverables(context_id) MCP tool (+ router get-deliverables): returns the persisted .feature, the test-data fixtures JSON, and every diagram mermaid — EACH in its own fenced block (```gherkin / ```json / ```mermaid) so the client can split into downloadable files and render mermaid inline.
- Already-persisted-but-newly-exposed: the .feature (memory/.../features/<ctx>.feature via export_features) and test_data were persisted before but had NO retrieval tool; get_deliverables is that tool.
KEY DECISION: persist diagrams as ONE diagrams.json dict (MemoryBank has only get/put text+json, no listing) rather than per-file .mmd — client reconstructs downloadable files. Commit 2834eac. Bridge prompt step 7 now points at get_deliverables and requires feature/test-data/diagrams to be accessible AND downloadable when rendering.
