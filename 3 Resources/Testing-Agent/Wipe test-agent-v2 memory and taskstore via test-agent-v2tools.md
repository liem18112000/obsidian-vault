---
title: "Wipe test-agent-v2 memory and taskstore via test-agent-v2/tools"
created: 2026-09-13
type: howto
status: seedling
source: "session 2026-09-13"
tags: [testing-agent, gcs, cloudsql, gotcha, v2]
---

# Wipe test-agent-v2 memory and taskstore via test-agent-v2/tools

To wipe the v2 stack state, use **`test-agent-v2/tools/`** (mirrors `test-agent-v1/tools/` but with v2 credentials). Both clear scripts are **preview-first**: a plain run only reports counts; re-run with `CONFIRM=1` to actually delete/truncate (irreversible).

- `clear_memory.sh` — deletes every object under the GCS memory bank. It **auto-resolves `GCS_BUCKET` from `../.env`**, so inside `test-agent-v2/tools/` it correctly targets `klara-nonprod-kga-v2-memory` with no override needed.
- `clear_taskstore.sh` — TRUNCATEs the `tasks` table on Cloud SQL. Prints "table does not exist yet" when no task was ever persisted (nothing to do).
- `cloudsql_proxy.sh` — Cloud SQL Auth Proxy for a GUI client.

**Gotcha (the reason this folder exists):** the v1 tools hardcode v1 resource names — instance `kga-taskstore`, secret `kga-db-password` — which **no longer exist** in the current v2 deployment, so running them fails with `Secret ... NOT_FOUND` / would target the wrong (empty) v1 bucket `mt-receive-ai-agent-memory`. The v2 folder swaps those two literals to `kga-v2-taskstore` / `kga-v2-db-password`.

**Also:** the destructive `CONFIRM=1` runs get denied by Claude Code auto-mode classifier — the user must run them (e.g. `! cd test-agent-v2/tools && CONFIRM=1 bash clear_memory.sh`) or add a Bash permission rule.

## Related
[[test-agent-v2 cloud resource and credential map (klara-nonprod)]]

## Related

- [[test-agent-v2 cloud resource and credential map (klara-nonprod)]]
