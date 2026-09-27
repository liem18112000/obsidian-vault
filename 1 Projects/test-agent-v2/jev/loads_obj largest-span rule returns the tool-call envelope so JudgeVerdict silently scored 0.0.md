---
title: "loads_obj largest-span rule returns the tool-call envelope so JudgeVerdict silently scored 0.0"
created: 2026-09-22
type: lesson
status: seedling
source: "session 2026-09-22, common/llm/parse.py"
tags: [test-agent-v2, bug, json-parsing, llm-judge, pydantic-defaults]
---

# loads_obj largest-span rule returns the tool-call envelope so JudgeVerdict silently scored 0.0

In test-agent-v2, `common/llm/parse.loads_obj` recovers a JSON object from an LLM reply by scanning every balanced `{...}` span and returning the **largest** one that parses to a dict. Claude-on-Vertex sometimes wraps its structured answer in a tool-call envelope — `{"parameters": {<the real fields>}}` — and the largest span is then the ENVELOPE, not the payload. The caller does `JudgeVerdict(**that)`; pydantic ignores the unknown `parameters` key, every real field defaults, and `overall` = 0.0 → `score()` = 0.0.

Consequence: the assured-loop LLM judge returned **0.0 for well-formed suites — a silent false rejection**, not a genuine one. This also corrupted the JEV calibration oracle (it appeared to reject every golden suite; the "recovered structured output … shape={\"parameters\": 11}" INFO log was the tell — JudgeVerdict has exactly 11 fields). Same wrapper hit generation (`shape={\"$PARAMETER_VALUE\": 1}`).

Fix (commit on `experiment/jev-decision-provider`): unwrap a single-key envelope whose key is a known tool-call wrapper (`parameters/arguments/input/output/json/result/response`) and whose value is a dict — done once in `loads_obj` (root, where both the generator and judge recovery route), looped for nested wrappers. A single-key **list** payload like `{"scenarios": [...]}` is deliberately untouched. Gotcha for any largest-valid-JSON heuristic feeding a defaulted schema: a wrong-shape parse is invisible because the schema silently defaults instead of raising. Related: [[JEV assured-gate errors are low-confidence false-accepts so the confidence gate is the safety net]].

## Related

- [[JEV assured-gate errors are low-confidence false-accepts so the confidence gate is the safety net]]
