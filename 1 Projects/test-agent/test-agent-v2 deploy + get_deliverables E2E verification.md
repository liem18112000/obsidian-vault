---
ai_hash: c52ac0c76515d81f
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-21
entities:
- test-agent-v2
- get_deliverables
- E2E verification
- klara-nonprod
- Deploy process
- tfvars image tag
- FRESH tag
- Cloud Run
- new content
- deployments/test-agent-v2/deploy.sh
- Cloud Build
- terraform apply
- Image reference pattern
- europe-west6-docker.pkg.dev/klara-nonprod/kga-v2/test-agent-v2:<tag>
- MCP tools
- gateway
- mcp_server.py
- register_tpd(...)
- TPD bridge
- register_tools
- TPD tool
- Claude Code MCP connection
- record_artifact
- /mcp-reconnect
- gateway revision swap
- client re-list
- router command
- send_raw_tpd("<command> <ctx>")
- Escape-hatch tool
- TPD agent
- Diagrams
- IMPLEMENT time
- pipeline._build_diagrams
- .feature (gherkin)
- test-data (json)
- architecture/scope/gaps (mermaid)
- run implemented BEFORE deploy
- new image
- deferred tools
source: session 2026-09-21
status: seedling
tags:
- test-agent
- deploy
- cloud-run
- mcp
- e2e
title: test-agent-v2 deploy + get_deliverables E2E verification
type: reference
---

# test-agent-v2 deploy + get_deliverables E2E verification

test-agent-v2 deploy E2E (2026-09-21): verified the new get_deliverables tool on klara-nonprod. Key facts:
- Deploy: bump the tfvars image tag to a FRESH tag (same tag = Cloud Run does not roll to new content), then `bash deployments/test-agent-v2/deploy.sh` (builds working tree via Cloud Build async+poll, then full terraform apply). Image ref pattern: europe-west6-docker.pkg.dev/klara-nonprod/kga-v2/test-agent-v2:<tag>. Deploy ~15-20min.
- New MCP tools on the gateway auto-surface because gateway/mcp_server.py splices **register_tpd(...) (the dict a TPD bridge register_tools returns). No gateway code change needed to add a TPD tool.
- Redeploying the gateway REFRESHED the running Claude Code MCP connection — the new get_deliverables + record_artifact tools appeared as deferred tools mid-session WITHOUT a manual /mcp-reconnect this time (contrast the older "reconnect to refresh" note; a gateway revision swap can trigger client re-list).
- To exercise a NEW deployed router command without waiting for client tool-refresh: use the existing send_raw_tpd("<command> <ctx>") escape-hatch tool — it routes a raw string to the deployed TPD agent through the gateway.
- Diagrams are written at IMPLEMENT time (pipeline._build_diagrams), so a run implemented BEFORE the deploy has feature+test-data in get_deliverables but NO diagrams until re-implemented on the new image.
- E2E PASS: get_deliverables returned .feature (gherkin), test-data (json), and architecture/scope/gaps (mermaid) fenced blocks.

%% ai-graph-start %%

**Related notes:**
- [[test-agent-v2 hardened deploy.sh flow and the unique image-tag bump that forces a new revision]]
- [[Deploying the test-agent-v2 Cloud Run stack (names, tags, plan)]]
- [[test-agent-v2 Cloud Run services use a -v2 name suffix]]
- [[Testing-agent admin tools get_run dumps an unbounded 1.4MB payload (MCP-unusable)]]
- [[test-agent-v2 persists diagram-as-code + get_deliverables tool]]

**Relations:**
- test-agent-v2 — *undergoes* — E2E verification
- E2E verification — *includes* — get_deliverables
- E2E verification — *performed on* — klara-nonprod
- Deploy process — *targets* — test-agent-v2
- Deploy process — *updates* — tfvars image tag
- tfvars image tag — *must be* — FRESH tag
- tfvars image tag — *affects* — Cloud Run
- Cloud Run — *does not roll to* — new content
- Deploy process — *executes* — deployments/test-agent-v2/deploy.sh
- deployments/test-agent-v2/deploy.sh — *uses* — Cloud Build
- deployments/test-agent-v2/deploy.sh — *performs* — terraform apply
- Deploy process — *takes* — ~15-20min
- Image reference pattern — *is* — europe-west6-docker.pkg.dev/klara-nonprod/kga-v2/test-agent-v2:<tag>
- MCP tools — *auto-surface on* — gateway
- gateway — *uses* — mcp_server.py
- mcp_server.py — *splices* — register_tpd(...)
- register_tpd(...) — *returned by* — TPD bridge register_tools
- TPD tool — *is a type of* — MCP tools
- TPD tool — *added without* — gateway code change
- get_deliverables — *is a* — TPD tool
- record_artifact — *is a* — TPD tool
- get_deliverables — *is a* — new tool
- record_artifact — *is a* — new tool
- Redeploying the gateway — *refreshed* — Claude Code MCP connection
- Claude Code MCP connection — *displayed* — get_deliverables
- Claude Code MCP connection — *displayed* — record_artifact
- get_deliverables — *appeared as* — deferred tools
- record_artifact — *appeared as* — deferred tools
- gateway revision swap — *can trigger* — client re-list
- send_raw_tpd("<command> <ctx>") — *is an* — Escape-hatch tool
- send_raw_tpd("<command> <ctx>") — *routes to* — TPD agent
- TPD agent — *accessed via* — gateway
- router command — *exercised by* — send_raw_tpd("<command> <ctx>")
- Diagrams — *written at* — IMPLEMENT time
- IMPLEMENT time — *is* — pipeline._build_diagrams
- get_deliverables — *returned* — .feature (gherkin)
- get_deliverables — *returned* — test-data (json)
- get_deliverables — *returned* — architecture/scope/gaps (mermaid)
- E2E verification — *passed with* — get_deliverables
- run implemented BEFORE deploy — *has* — feature+test-data
- run implemented BEFORE deploy — *lacks* — Diagrams
- Diagrams — *require* — re-implementation on new image

%% ai-graph-end %%