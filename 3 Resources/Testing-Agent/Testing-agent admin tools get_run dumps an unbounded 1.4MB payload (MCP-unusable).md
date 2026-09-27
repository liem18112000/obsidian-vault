---
title: "Testing-agent admin tools: get_run dumps an unbounded 1.4MB payload (MCP-unusable)"
created: 2026-09-14
type: lesson
status: seedling
source: "session 2026-09-14 admin test"
tags: [testing-agent, admin-tools, mcp, bug, memory, test-agent-v2]
---

# Testing-agent admin tools: get_run dumps an unbounded 1.4MB payload (MCP-unusable)

Thorough test of the deployed test-agent-v2 **admin agent** tools (surfaced on the MCP gateway as `list_runs / get_run / view_memory / backup_memory / list_backups / wipe_all` after an /mcp reconnect; the admin-agent-v2 Cloud Run service backs them). Findings on klara-nonprod, 2026-09-14:

- **get_run(context_id) is BROKEN for real runs** — it returns the ENTIRE run as one markdown blob: the understanding brief + every pack node's full body + all links. For run-cd156028 (61-node pack) that was **1,431,813 chars (~1.4 MB)**, which exceeds the MCP tool token limit (gets spilled to a file). Unusable interactively. Fix: paginate or return a summary + drill-down like list_runs does, or cap node bodies.
- **backup_memory is heavy/slow** — >120s (backgrounded), copied **558 blobs / ~227 MB** for an 874-node bank; pgvector is NOT copied (rebuild via pg/backfill.py). It DOES round-trip into list_backups correctly. memory-backups/** survives a wipe.
- **view_memory(semantic) shows pgvector rows = n/a** — the two-tier (GCS + pgvector) counts aren't surfaced in the admin view (only the GCS index: 874 nodes incl. 649 test-scenario, 1741 edges).
- **view_memory(procedural) is stale** — still labels the assured loop '(TPD_ASSURED)', but that opt-in flag was removed (assured is always-on now).
- **wipe_all is guarded twice**: (1) the Claude Code auto-mode CLASSIFIER blocks the call outright (even with a wrong token) unless the user approves, and (2) the server requires `confirm` == the GCS_BUCKET value (or literal 'WIPE' when unset) and echoes the expected token otherwise. memory-backups/** always survives it.
- list_runs / view_memory (working/episodic) / list_backups all work cleanly.

The admin tools are [ADMIN — not part of the testing pipeline] and live on their own admin-agent-v2 service.

Related: [[test-agent-v2 hardened deploy.sh flow and the unique image-tag bump that forces a new revision]]

## Related

- [[test-agent-v2 hardened deploy.sh flow and the unique image-tag bump that forces a new revision]]
