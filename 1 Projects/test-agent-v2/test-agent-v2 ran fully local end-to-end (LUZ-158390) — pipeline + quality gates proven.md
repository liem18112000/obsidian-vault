---
title: "test-agent-v2 ran fully local end-to-end (LUZ-158390) — pipeline + quality gates proven"
created: 2026-09-23
type: project
status: seedling
source: "session 2026-09-23"
tags: [testing-agent, local, pipeline, milestone, luz-158390, evaluation]
---

# test-agent-v2 ran fully local end-to-end (LUZ-158390) — pipeline + quality gates proven

MILESTONE (2026-09-23): the test-agent-v2 pipeline ran FULLY LOCAL end-to-end for the first time (LUZ-158390, context run-3be0f1e0) on the docker-compose stack — MinIO/Postgres+pgvector/Redis/Ollama-embeddings + Claude-subscription LLM via claude-proxy. Every stage worked: gather (40 nodes, real Atlassian crawl + memory recall) → codegraph grounding (axonivy-prod/luz_docs_import, graphify in-container) → refine (26 insights, business+technical rounds) → approve → define_plan (48 decisions: methodology/scope/metrics/test-design rounds) → approve_plan → implement (117 scenarios / 435 steps / 43 fixtures / a BDD .feature via the P4 assured loop) → evaluate_plan. RESULTS: assured loop BELOW BAR 0.58<0.70 (judge correctly caught cross-source DUPLICATION — behaviours repeated 3-4x, generated per-source-node without dedup); TPS 0.576 (trajectory 1.0, scope recall 1.0/leaked [], but ac_recall 0.32, oracle_strength 0.20 = 320 weak steps, precision 0.00 = complete-but-noisy). Interpretation: the stack + quality gates WORK; low scores = a first-pass suite needing a dedup re-run, not a stack failure. The two-timeout fix ([[Slow local implement_plan needs TWO timeouts raised: client MCP idle + gateway A2A_CLIENT_TIMEOUT]]) was required to get implement through. See [[Full local-parity stack for test-agent-v2 (MinIO PG Redis PubSub Ollama laya)]].

## Related

- [[Full local-parity stack for test-agent-v2 (MinIO PG Redis PubSub Ollama laya)]]
