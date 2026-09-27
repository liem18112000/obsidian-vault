---
ai_hash: 6783340a8aeb79c5
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-22
entities:
- loads_obj largest-span rule
- JudgeVerdict
- test-agent-v2
- common/llm/parse.loads_obj
- LLM reply
- JSON object
- Claude-on-Vertex
- tool-call envelope
- pydantic
- overall score
- assured-loop LLM judge
- JEV calibration oracle
- golden suite
- INFO log
- wrapper hit generation
- experiment/jev-decision-provider
- single-key envelope
- tool-call wrapper keys
- dict
- single-key list payload
- largest-valid-JSON heuristic
- defaulted schema
- JEV assured-gate errors
- confidence gate
- parameters
- arguments
- input
- output
- json
- result
- response
source: session 2026-09-22, common/llm/parse.py
status: seedling
tags:
- test-agent-v2
- bug
- json-parsing
- llm-judge
- pydantic-defaults
title: loads_obj largest-span rule returns the tool-call envelope so JudgeVerdict
  silently scored 0.0
type: lesson
---

# loads_obj largest-span rule returns the tool-call envelope so JudgeVerdict silently scored 0.0

In test-agent-v2, `common/llm/parse.loads_obj` recovers a JSON object from an LLM reply by scanning every balanced `{...}` span and returning the **largest** one that parses to a dict. Claude-on-Vertex sometimes wraps its structured answer in a tool-call envelope — `{"parameters": {<the real fields>}}` — and the largest span is then the ENVELOPE, not the payload. The caller does `JudgeVerdict(**that)`; pydantic ignores the unknown `parameters` key, every real field defaults, and `overall` = 0.0 → `score()` = 0.0.

Consequence: the assured-loop LLM judge returned **0.0 for well-formed suites — a silent false rejection**, not a genuine one. This also corrupted the JEV calibration oracle (it appeared to reject every golden suite; the "recovered structured output … shape={\"parameters\": 11}" INFO log was the tell — JudgeVerdict has exactly 11 fields). Same wrapper hit generation (`shape={\"$PARAMETER_VALUE\": 1}`).

Fix (commit on `experiment/jev-decision-provider`): unwrap a single-key envelope whose key is a known tool-call wrapper (`parameters/arguments/input/output/json/result/response`) and whose value is a dict — done once in `loads_obj` (root, where both the generator and judge recovery route), looped for nested wrappers. A single-key **list** payload like `{"scenarios": [...]}` is deliberately untouched. Gotcha for any largest-valid-JSON heuristic feeding a defaulted schema: a wrong-shape parse is invisible because the schema silently defaults instead of raising. Related: [[JEV assured-gate errors are low-confidence false-accepts so the confidence gate is the safety net]].

## Related

- [[JEV assured-gate errors are low-confidence false-accepts so the confidence gate is the safety net]]

%% ai-graph-start %%

**Related notes:**
- [[JEV assured-gate errors are low-confidence false-accepts so the confidence gate is the safety net]]
- [[JEV accept-side confidence is low and unreliable; the cascade win is reject-side]]
- [[TypeSafe SDK response shape gotcha - cached-property accessors and Noul has no confidence]]
- [[Test an LLM-vs-heuristic seam offline by monkeypatching complete() per module]]
- [[Calibrate a cheap-model to LLM cascade threshold using the LLM judge as oracle]]

**Relations:**
- loads_obj largest-span rule — *returns* — tool-call envelope
- tool-call envelope — *causes* — JudgeVerdict
- JudgeVerdict — *scored* — 0.0
- common/llm/parse.loads_obj — *is part of* — test-agent-v2
- common/llm/parse.loads_obj — *recovers* — JSON object
- JSON object — *from* — LLM reply
- common/llm/parse.loads_obj — *uses* — loads_obj largest-span rule
- Claude-on-Vertex — *wraps structured answer in* — tool-call envelope
- tool-call envelope — *contains* — parameters
- pydantic — *ignores* — unknown parameters key
- pydantic — *defaults* — real fields
- defaulted real fields — *lead to* — overall score
- overall score — *equals* — 0.0
- assured-loop LLM judge — *returned* — 0.0
- 0.0 — *for* — well-formed suites
- 0.0 — *is a* — silent false rejection
- silent false rejection — *corrupted* — JEV calibration oracle
- JEV calibration oracle — *rejected* — golden suite
- INFO log — *indicated* — shape={"parameters": 11}
- JudgeVerdict — *has* — 11 fields
- wrapper hit generation — *showed* — shape={"$PARAMETER_VALUE": 1}
- Fix — *is on* — experiment/jev-decision-provider
- Fix — *involves* — unwrap single-key envelope
- single-key envelope — *has* — tool-call wrapper keys
- tool-call wrapper keys — *include* — parameters
- tool-call wrapper keys — *include* — arguments
- tool-call wrapper keys — *include* — input
- tool-call wrapper keys — *include* — output
- tool-call wrapper keys — *include* — json
- tool-call wrapper keys — *include* — result
- tool-call wrapper keys — *include* — response
- single-key envelope — *value is* — dict
- unwrap — *done in* — common/llm/parse.loads_obj
- unwrap — *is* — looped for nested wrappers
- single-key list payload — *is* — deliberately untouched
- wrong-shape parse — *is* — invisible
- largest-valid-JSON heuristic — *feeds* — defaulted schema
- defaulted schema — *silently defaults instead of* — raising
- JEV assured-gate errors — *are* — Related
- JEV assured-gate errors — *are* — low-confidence false-accepts
- confidence gate — *is* — safety net

%% ai-graph-end %%