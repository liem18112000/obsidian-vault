---
ai_hash: 954ae0b25da1b28c
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-28
entities:
- LLM-vs-heuristic seam
- monkeypatching
- complete()
- generator
- make_*() factory
- Claude-on-Vertex path
- vertex_config()
- VERTEX_PROJECT
- VERTEX_LOCATION
- VERTEX_MODEL
- deterministic heuristic
- monkeypatch.delenv
- monkeypatch.setenv
- monkeypatch.setattr
- test_plan_definition.llm.questions.complete
- CANNED_JSON
- Claude branch
- prompt-build
- parse
- dataclass-construction
- Vertex
- MODULE THAT IMPORTED IT
- knowledge_gathering.llm.vertex
- local binding
- source module
- fallback
- non-JSON
- caller
- parser
- None
- non-array
- knowledge_gathering
- refine LLM seam
- Testing Agent
- pipeline stage
- package
- knowledge_gathering skeleton
source: session 2026-08-28, test_plan_definition M6
status: seedling
tags:
- test-agent
- testing
- llm
- vertex
- pytest
title: Test an LLM-vs-heuristic seam offline by monkeypatching complete() per module
type: lesson
---

# Test an LLM-vs-heuristic seam offline by monkeypatching complete() per module

Each generator has a `make_*()` factory that returns a Claude-on-Vertex path when `vertex_config()` (VERTEX_PROJECT/LOCATION/MODEL) is set, else a deterministic heuristic. To test both paths with NO credentials and NO network:

- **Heuristic default:** `monkeypatch.delenv("VERTEX_PROJECT")` → the factory returns the heuristic; assert its deterministic output.
- **Claude path:** `monkeypatch.setenv` the three VERTEX_* vars, THEN `monkeypatch.setattr("test_plan_definition.llm.questions.complete", lambda *a, **k: CANNED_JSON)`. The factory picks the Claude branch, and the canned JSON exercises the real prompt-build + parse + dataclass-construction without hitting Vertex.

Key detail: patch `complete` in the MODULE THAT IMPORTED IT (`...llm.questions.complete`, `...llm.plan.complete`, `...llm.scenarios.complete`) — because it was pulled in via `from knowledge_gathering.llm.vertex import complete`, so the name to patch is the local binding, not the source module. Also test the fallback: feed non-JSON and assert the caller drops back to the heuristic (the parser returns None on a non-array). This mirrors how knowledge_gathering tests its refine LLM seam.

## Related

- [[Testing Agent builds each pipeline stage as a package mirroring the knowledge_gathering skeleton]]

%% ai-graph-start %%

**Related notes:**
- [[Testing Agent builds each pipeline stage as a package mirroring the knowledge_gathering skeleton]]
- [[TPD IMPLEMENT makes one LLM call by default (scenarios only)]]
- [[Hermetic E2E test of an LLM agent mock only the SDK boundary, inject the prompt snapshot]]
- [[Pluggable LLM via the litellm ModelProvider backend]]
- [[test-agent-v2 TPD has five raw-Vertex generators — the ADK LlmAgent conversion targets]]

**Relations:**
- LLM-vs-heuristic seam — *tested by* — monkeypatching
- monkeypatching — *targets* — complete()
- generator — *has* — make_*() factory
- make_*() factory — *returns* — Claude-on-Vertex path
- make_*() factory — *returns* — deterministic heuristic
- Claude-on-Vertex path — *requires* — vertex_config() is set
- deterministic heuristic — *requires* — vertex_config() is not set
- vertex_config() — *includes* — VERTEX_PROJECT
- vertex_config() — *includes* — VERTEX_LOCATION
- vertex_config() — *includes* — VERTEX_MODEL
- monkeypatch.delenv — *removes* — VERTEX_PROJECT
- monkeypatch.delenv — *leads to* — deterministic heuristic
- monkeypatch.setenv — *sets* — VERTEX_PROJECT
- monkeypatch.setenv — *sets* — VERTEX_LOCATION
- monkeypatch.setenv — *sets* — VERTEX_MODEL
- monkeypatch.setattr — *patches* — test_plan_definition.llm.questions.complete
- test_plan_definition.llm.questions.complete — *uses* — CANNED_JSON
- CANNED_JSON — *exercises* — prompt-build
- CANNED_JSON — *exercises* — parse
- CANNED_JSON — *exercises* — dataclass-construction
- CANNED_JSON — *avoids hitting* — Vertex
- factory — *picks* — Claude branch
- complete() — *imported from* — knowledge_gathering.llm.vertex
- complete() — *patched in* — MODULE THAT IMPORTED IT
- name to patch — *is* — local binding
- local binding — *differs from* — source module
- fallback — *tested with* — non-JSON
- non-JSON — *causes* — parser returns None
- parser returns None — *on* — non-array
- parser returns None — *causes* — caller drops back to heuristic
- knowledge_gathering — *tests* — refine LLM seam
- Testing Agent — *builds* — pipeline stage
- pipeline stage — *is a* — package
- package — *mirrors* — knowledge_gathering skeleton

%% ai-graph-end %%