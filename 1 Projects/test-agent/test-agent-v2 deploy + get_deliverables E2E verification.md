---
title: "test-agent-v2 deploy + get_deliverables E2E verification"
created: 2026-09-21
type: reference
status: seedling
source: "session 2026-09-21"
tags: [test-agent, deploy, cloud-run, mcp, e2e]
---

# test-agent-v2 deploy + get_deliverables E2E verification

test-agent-v2 deploy E2E (2026-09-21): verified the new get_deliverables tool on klara-nonprod. Key facts:
- Deploy: bump the tfvars image tag to a FRESH tag (same tag = Cloud Run does not roll to new content), then `bash deployments/test-agent-v2/deploy.sh` (builds working tree via Cloud Build async+poll, then full terraform apply). Image ref pattern: europe-west6-docker.pkg.dev/klara-nonprod/kga-v2/test-agent-v2:<tag>. Deploy ~15-20min.
- New MCP tools on the gateway auto-surface because gateway/mcp_server.py splices **register_tpd(...) (the dict a TPD bridge register_tools returns). No gateway code change needed to add a TPD tool.
- Redeploying the gateway REFRESHED the running Claude Code MCP connection — the new get_deliverables + record_artifact tools appeared as deferred tools mid-session WITHOUT a manual /mcp-reconnect this time (contrast the older "reconnect to refresh" note; a gateway revision swap can trigger client re-list).
- To exercise a NEW deployed router command without waiting for client tool-refresh: use the existing send_raw_tpd("<command> <ctx>") escape-hatch tool — it routes a raw string to the deployed TPD agent through the gateway.
- Diagrams are written at IMPLEMENT time (pipeline._build_diagrams), so a run implemented BEFORE the deploy has feature+test-data in get_deliverables but NO diagrams until re-implemented on the new image.
- E2E PASS: get_deliverables returned .feature (gherkin), test-data (json), and architecture/scope/gaps (mermaid) fenced blocks.
