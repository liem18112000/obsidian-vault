---
ai_hash: 6b56a7c7c4489381
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-21
entities:
- test-agent-v2
- diagram-as-code
- get_deliverables tool
- agent-side HTML renderers
- agent
- client
- deliverables
- common/testplan/diagrams.py
- mermaid diagrams
- architecture
- scope
- gaps
- Raw .mmd source
- HTML
- LLM
- build_diagrams
- plan
- coverage_dict
- pipeline._build_diagrams
- memory/test-plan/<ctx>/diagrams.json
- store.write_diagrams
- store.read_diagrams
- MCP tool
- router get-deliverables
- .feature
- test-data
- fenced block
- gherkin
- json
- mermaid
- downloadable files
- memory/.../features/<ctx>.feature
- export_features
- diagrams.json
- MemoryBank
- Commit 2834eac
- Bridge prompt step 7
source: session 2026-09-21
status: seedling
tags:
- test-agent
- mermaid
- diagrams
- deliverables
- architecture
title: test-agent-v2 persists diagram-as-code + get_deliverables tool
type: reference
---

# test-agent-v2 persists diagram-as-code + get_deliverables tool

test-agent-v2 (2026-09-21): after retiring agent-side HTML renderers (agent=data-only, client renders), the agent must still PERSIST the full downloadable deliverable set so the client can render + offer downloads. Implemented:
- common/testplan/diagrams.py: deterministic mermaid diagram-as-code from the persisted run — architecture (requirements->SUT->oracle from the coverage matrix), scope (in/out), gaps (dev-to-confirm). Raw .mmd source, no HTML, no LLM. build_diagrams(plan, coverage_dict) -> {name: mermaid}.
- Persisted at implement time (pipeline._build_diagrams, best-effort like _build_coverage) as memory/test-plan/<ctx>/diagrams.json via store.write_diagrams/read_diagrams.
- get_deliverables(context_id) MCP tool (+ router get-deliverables): returns the persisted .feature, the test-data fixtures JSON, and every diagram mermaid — EACH in its own fenced block (```gherkin / ```json / ```mermaid) so the client can split into downloadable files and render mermaid inline.
- Already-persisted-but-newly-exposed: the .feature (memory/.../features/<ctx>.feature via export_features) and test_data were persisted before but had NO retrieval tool; get_deliverables is that tool.
KEY DECISION: persist diagrams as ONE diagrams.json dict (MemoryBank has only get/put text+json, no listing) rather than per-file .mmd — client reconstructs downloadable files. Commit 2834eac. Bridge prompt step 7 now points at get_deliverables and requires feature/test-data/diagrams to be accessible AND downloadable when rendering.

%% ai-graph-start %%

**Related notes:**
- [[test-agent-v2 always-enriched HTML report generator]]
- [[test-agent-v2 deploy + get_deliverables E2E verification]]
- [[test-agent-v2 retired agent-side HTML renderers — agent is data-only, client renders]]
- [[Testing Agent builds each pipeline stage as a package mirroring the knowledge_gathering skeleton]]
- [[Render a test-agent run in ePost design client-side from the bank]]

**Relations:**
- test-agent-v2 — *persists* — diagram-as-code
- test-agent-v2 — *uses* — get_deliverables tool
- test-agent-v2 — *retires* — agent-side HTML renderers
- agent — *is* — data-only
- client — *renders* — data
- agent — *persists* — deliverables
- client — *renders* — deliverables
- client — *offers downloads* — deliverables
- common/testplan/diagrams.py — *generates* — mermaid diagrams
- mermaid diagrams — *describe* — architecture
- mermaid diagrams — *describe* — scope
- mermaid diagrams — *describe* — gaps
- mermaid diagrams — *are* — Raw .mmd source
- mermaid diagrams — *exclude* — HTML
- mermaid diagrams — *exclude* — LLM
- build_diagrams — *is defined in* — common/testplan/diagrams.py
- build_diagrams — *takes* — plan
- build_diagrams — *takes* — coverage_dict
- build_diagrams — *returns* — mermaid diagrams
- diagram-as-code — *persisted at* — memory/test-plan/<ctx>/diagrams.json
- diagram-as-code — *persisted via* — store.write_diagrams
- diagram-as-code — *read via* — store.read_diagrams
- pipeline._build_diagrams — *persists* — diagram-as-code
- get_deliverables tool — *is a* — MCP tool
- get_deliverables tool — *is a* — router get-deliverables
- get_deliverables tool — *returns* — .feature
- get_deliverables tool — *returns* — test-data
- get_deliverables tool — *returns* — mermaid diagrams
- returned items — *are in* — fenced block
- fenced block — *uses format* — gherkin
- fenced block — *uses format* — json
- fenced block — *uses format* — mermaid
- client — *splits* — fenced block
- fenced block — *into* — downloadable files
- client — *renders* — mermaid
- mermaid — *inline* — client
- .feature — *persisted at* — memory/.../features/<ctx>.feature
- .feature — *persisted via* — export_features
- get_deliverables tool — *retrieves* — .feature
- get_deliverables tool — *retrieves* — test-data
- diagrams — *persisted as* — diagrams.json
- MemoryBank — *supports* — get/put text+json
- MemoryBank — *lacks* — listing
- client — *reconstructs* — downloadable files
- downloadable files — *from* — diagrams.json
- Commit 2834eac — *describes* — persist diagrams as ONE diagrams.json dict
- Bridge prompt step 7 — *points at* — get_deliverables tool
- Bridge prompt step 7 — *requires* — .feature
- .feature — *to be* — accessible
- .feature — *to be* — downloadable
- Bridge prompt step 7 — *requires* — test-data
- test-data — *to be* — accessible
- test-data — *to be* — downloadable
- Bridge prompt step 7 — *requires* — diagrams
- diagrams — *to be* — accessible
- diagrams — *to be* — downloadable

%% ai-graph-end %%