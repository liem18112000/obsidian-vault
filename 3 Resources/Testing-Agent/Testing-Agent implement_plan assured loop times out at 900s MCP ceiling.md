---
title: "Testing-Agent implement_plan assured loop times out at 900s MCP ceiling"
created: 2026-09-15
type: lesson
status: seedling
source: "LUZ-158230 run 2026-09-15"
tags: [testing-agent, tpd, implement_plan, assured-loop, timeout, gotcha]
---

# Testing-Agent implement_plan assured loop times out at 900s MCP ceiling

The Testing-Agent `implement_plan` tool always runs the **P4 assured generation loop** (generate -> judge -> gate -> reflect -> regenerate), a series of serial Vertex LLM calls. On a rich ticket (LUZ-158230) it **timed out at the 900s MCP tool ceiling** ("tool implement_plan timed out after 900s"), so no scenarios were produced. `detail=True` adds MORE LLM (test-data + steps) and makes it worse; there is no flag on the MCP surface to skip the assured loop.

Mitigations: the loop is **GCS-checkpointed**, so a plain retry (`implement_plan(context_id)`) can RESUME and may finish within 900s. If it keeps timing out, fall back to authoring the BDD scenario suite CLIENT-SIDE from the confirmed spec and rendering it as an HTML artifact (the reliable path) — see [[LUZ-158230 ePost ZIP import spec v1.0 (authoritative)]] for the oracle. Related root cause: [[Testing-Agent refine confidence is capped by un-ingested spec PDFs]].

## Related

- [[LUZ-158230 ePost ZIP import spec v1.0 (authoritative)]]
- [[Testing-Agent refine confidence is capped by un-ingested spec PDFs]]

## Addendum — checkpoint-resume does NOT reliably rescue it

A second `implement_plan(context_id)` retry ALSO timed out at 900s (back-to-back on LUZ-158230). So the "GCS-checkpointed, just retry" mitigation is unreliable — a resume can still exceed 900s. Practical rule: after ONE timeout, do NOT keep retrying the tool; author the scenario suite CLIENT-SIDE from the spec and publish it. Consequence: `evaluate_plan` (Test-Plan Score) cannot run for such a ticket because it requires generated scenarios (implement first), so that server metric is simply unavailable when implement times out.
