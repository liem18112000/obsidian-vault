---
title: "Parallelize TPD generation via 3 Redis-worker replicas, not _BATCH_CONCURRENCY"
created: 2026-09-23
type: howto
status: seedling
source: "session 2026-09-23"
tags: [testing-agent, parallelism, redis, workers, tpd, performance, docker-compose]
---

# Parallelize TPD generation via 3 Redis-worker replicas, not _BATCH_CONCURRENCY

HOW to parallelize the TPD scenario generator (and what NOT to do): the safe parallelism is the Phase-C WORKER POOL, not in-process concurrency. `test_plan_definition/implement/generate/llm.py` has `_BATCH_CONCURRENCY=1` with a standing warning — concurrency>1 there runs concurrent IN-PROCESS ADK Runners which returned empty/identical structured-output batches (a real bug, reverted). So DO NOT raise `_BATCH_CONCURRENCY`. Instead use `TPD_GEN_MODE=workers`: the coordinator (`workers.worker_scenarios`) splits the pack into per-batch jobs, XADDs them to a Redis Stream, and polls the ObjectStore (MinIO) for result blobs (budget `TPD_WORKER_BUDGET_S`, default 600s); each worker (`redis_worker.py`→`handle_job`) is a SEPARATE PROCESS using the direct `complete()` path (no ADK Runner) → no shared-state bug. To get N-way parallelism just run N worker replicas: `deploy: {replicas: 3}` on the compose `worker` service (honored by `docker compose up`, no swarm). Consumer name = `$HOSTNAME` (unique per container) so the consumer group `tpd-gen-workers` load-balances — each job handled once. Verify via `redis-cli XINFO GROUPS tpd-gen-batches` (consumers/pending/entries-read). GOTCHA found doing this: the `claude-proxy` service had NO `env_file`, so `CLAUDE_MODEL` never reached server.py and every `claude -p` used the Claude Code default (Opus 5.5, slowest); add `env_file: .env.compose` + set `CLAUDE_MODEL=claude-sonnet-5`. See [[Why local test-agent is slow: claude -p ships a 17.5K agent prompt on Opus, x many serial calls]].

## Related

- [[Why local test-agent is slow: claude -p ships a 17.5K agent prompt on Opus]]
- [[x many serial calls]]
