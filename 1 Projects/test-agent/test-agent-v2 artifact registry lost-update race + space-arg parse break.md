---
title: "test-agent-v2 artifact registry: lost-update race + space-arg parse break"
created: 2026-09-21
type: lesson
status: seedling
source: "session 2026-09-21"
tags: [test-agent, bug, registry, gotcha, concurrency]
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
