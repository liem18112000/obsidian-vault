---
ai_hash: 061fb03a3eba1316
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-21
entities:
- test-agent-v2 artifact registry
- record_artifact
- common/admin/runs.py
- ePost artifacts
- run-7bcfe335
- artifacts.json
- LOST-UPDATE RACE
- ObjectStore CAS
- if_generation_match
- upload_from_string
- memory bank
- MCP tool
- router
- SPACE-IN-ARG PARSE BREAK
- admin_agent/agent.py._record_artifact
- tpd/admin bridge forwarder
- commit 77237ad
- CAS retry loop
- CASConflict
- admin bridge
- JSON
- positional split
- store
- image build
source: session 2026-09-21
status: seedling
tags:
- test-agent
- bug
- registry
- gotcha
- concurrency
title: 'test-agent-v2 artifact registry: lost-update race + space-arg parse break'
type: lesson
---

# test-agent-v2 artifact registry: lost-update race + space-arg parse break

Two bugs in the deployed test-agent-v2 artifact registry (record_artifact / common/admin/runs.py), found rendering ePost artifacts for run-7bcfe335 (2026-09-21):
1. LOST-UPDATE RACE: two record_artifact calls issued in parallel both read the same artifacts.json, append, and write back — the second overwrites the first (classic RMW lost update). Symptom: one call reported "already recorded" and its row was missing. The registry write is NOT CAS-guarded. Fix: use the ObjectStore CAS (if_generation_match / upload_from_string generation) with a retry loop, like the memory bank does elsewhere. Workaround: record serially.
2. SPACE-IN-ARG PARSE BREAK: the MCP tool record_artifact(context_id, kind, url, title="") forwards to the router as a raw string "record-artifact <ctx> <kind> <url> [title...]" parsed with split(maxsplit=3). A kind (or url) containing a SPACE mis-splits — e.g. kind="knowledge (ePost)" stored url="(ePost)" and shoved the real url into title. Fix: the router handler should not positionally split when the bridge already has structured args — pass a delimiter or JSON, or the bridge tool should reject/replace spaces in kind. Workaround: use space-free kinds (knowledge-epost, report-epost).
Both are follow-up fixes in common/admin/runs.py + admin_agent/agent.py._record_artifact + the tpd/admin bridge forwarder.


## Resolved (commit 77237ad, 2026-09-21)
- Race → CAS retry loop in `record_artifact` (runs.py): read (items, generation) → append → `upload_from_string(if_generation_match=generation)`, retry on `CASConflict` (reuses the store CAS contract).
- Space parse → the admin bridge now JSON-encodes the args (`record-artifact {json}`); the router `_record_artifact` parses JSON, with positional split kept only as a backward-compat fallback for manual `send_raw`.
- Tests: conflict-injecting store proves retry; router JSON args preserve spaces in kind/url/title. Committed + pushed; NOT yet deployed (rides the next image build).

%% ai-graph-start %%

**Related notes:**
- [[Testing-agent admin tools get_run dumps an unbounded 1.4MB payload (MCP-unusable)]]
- [[test-agent-v2 executor tests share memory-bank state and fail by test order]]
- [[test-agent-v2 run benchmark — TEV ownership forced by layering]]
- [[test-agent-v2 always-enriched HTML report generator]]
- [[test-agent-v2 hardened deploy.sh flow and the unique image-tag bump that forces a new revision]]

**Relations:**
- test-agent-v2 artifact registry — *uses* — record_artifact
- record_artifact — *is defined in* — common/admin/runs.py
- ePost artifacts — *found for* — run-7bcfe335
- record_artifact — *accesses* — artifacts.json
- LOST-UPDATE RACE — *affects* — record_artifact
- ObjectStore CAS — *uses* — if_generation_match
- ObjectStore CAS — *uses* — upload_from_string
- memory bank — *uses* — ObjectStore CAS
- MCP tool — *calls* — record_artifact
- record_artifact — *forwards to* — router
- router — *parses* — record-artifact <ctx> <kind> <url> [title...]
- SPACE-IN-ARG PARSE BREAK — *affects* — router
- admin_agent/agent.py._record_artifact — *is involved in fix for* — SPACE-IN-ARG PARSE BREAK
- tpd/admin bridge forwarder — *is involved in fix for* — SPACE-IN-ARG PARSE BREAK
- commit 77237ad — *resolved* — LOST-UPDATE RACE
- commit 77237ad — *resolved* — SPACE-IN-ARG PARSE BREAK
- CAS retry loop — *is fix for* — LOST-UPDATE RACE
- CAS retry loop — *uses* — upload_from_string
- CAS retry loop — *retries on* — CASConflict
- admin bridge — *JSON-encodes* — args
- router — *parses* — JSON
- router — *uses* — positional split
- store — *proves* — CAS retry loop
- router JSON args — *preserve* — spaces
- commit 77237ad — *awaiting* — image build

%% ai-graph-end %%