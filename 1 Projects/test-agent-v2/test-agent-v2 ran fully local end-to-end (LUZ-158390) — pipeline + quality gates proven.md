---
ai_hash: d6a43f6a450c6cb5
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-23
entities:
- test-agent-v2
- LUZ-158390
- pipeline
- quality gates
- docker-compose stack
- MinIO
- Postgres
- pgvector
- Redis
- Ollama-embeddings
- Claude-subscription LLM
- claude-proxy
- gather
- codegraph grounding
- axonivy-prod/luz_docs_import
- graphify
- refine
- approve
- define_plan
- approve_plan
- implement
- evaluate_plan
- P4 assured loop
- judge
- cross-source DUPLICATION
- TPS
- trajectory
- scope recall
- ac_recall
- oracle_strength
- precision
- stack
- two-timeout fix
- client MCP idle
- gateway A2A_CLIENT_TIMEOUT
- Full local-parity stack for test-agent-v2 (MinIO PG Redis PubSub Ollama laya)
- low scores
- '2026-09-23'
- Atlassian crawl
- memory recall
- BDD .feature
- PubSub
- Ollama
- laya
- PG
source: session 2026-09-23
status: seedling
tags:
- testing-agent
- local
- pipeline
- milestone
- luz-158390
- evaluation
title: test-agent-v2 ran fully local end-to-end (LUZ-158390) — pipeline + quality
  gates proven
type: project
---

# test-agent-v2 ran fully local end-to-end (LUZ-158390) — pipeline + quality gates proven

MILESTONE (2026-09-23): the test-agent-v2 pipeline ran FULLY LOCAL end-to-end for the first time (LUZ-158390, context run-3be0f1e0) on the docker-compose stack — MinIO/Postgres+pgvector/Redis/Ollama-embeddings + Claude-subscription LLM via claude-proxy. Every stage worked: gather (40 nodes, real Atlassian crawl + memory recall) → codegraph grounding (axonivy-prod/luz_docs_import, graphify in-container) → refine (26 insights, business+technical rounds) → approve → define_plan (48 decisions: methodology/scope/metrics/test-design rounds) → approve_plan → implement (117 scenarios / 435 steps / 43 fixtures / a BDD .feature via the P4 assured loop) → evaluate_plan. RESULTS: assured loop BELOW BAR 0.58<0.70 (judge correctly caught cross-source DUPLICATION — behaviours repeated 3-4x, generated per-source-node without dedup); TPS 0.576 (trajectory 1.0, scope recall 1.0/leaked [], but ac_recall 0.32, oracle_strength 0.20 = 320 weak steps, precision 0.00 = complete-but-noisy). Interpretation: the stack + quality gates WORK; low scores = a first-pass suite needing a dedup re-run, not a stack failure. The two-timeout fix ([[Slow local implement_plan needs TWO timeouts raised client MCP idle + gateway A2A_CLIENT_TIMEOUT|Slow local implement_plan needs TWO timeouts raised: client MCP idle + gateway A2A_CLIENT_TIMEOUT]]) was required to get implement through. See [[Full local-parity stack for test-agent-v2 (MinIO PG Redis PubSub Ollama laya)]].

## Related

- [[Full local-parity stack for test-agent-v2 (MinIO PG Redis PubSub Ollama laya)]]

%% ai-graph-start %%

**Related notes:**
- [[Full local-parity stack for test-agent-v2 (MinIO PG Redis PubSub Ollama laya)]]
- [[test-agent-v2 fixes implement_plan predictive budget guard + gather explore opt-in gate]]
- [[run_json_agent needed a per-call timeout or a slow Vertex call hangs implement past the server ceiling]]
- [[Why local test-agent is slow claude -p ships a 17.5K agent prompt on Opus, x many serial calls]]
- [[Run test-agent-v2 locally with docker-compose (no GCP)]]

**Relations:**
- test-agent-v2 — *ran* — fully local end-to-end
- test-agent-v2 — *is associated with* — LUZ-158390
- pipeline — *is proven* — true
- quality gates — *is proven* — true
- test-agent-v2 — *has component* — pipeline
- pipeline — *ran on* — docker-compose stack
- docker-compose stack — *includes* — MinIO
- docker-compose stack — *includes* — Postgres
- Postgres — *uses* — pgvector
- docker-compose stack — *includes* — Redis
- docker-compose stack — *includes* — Ollama-embeddings
- docker-compose stack — *includes* — Claude-subscription LLM
- Claude-subscription LLM — *accessed via* — claude-proxy
- pipeline — *has stage* — gather
- pipeline — *has stage* — codegraph grounding
- pipeline — *has stage* — refine
- pipeline — *has stage* — approve
- pipeline — *has stage* — define_plan
- pipeline — *has stage* — approve_plan
- pipeline — *has stage* — implement
- pipeline — *has stage* — evaluate_plan
- gather — *processed* — 40 nodes
- gather — *used* — Atlassian crawl
- gather — *used* — memory recall
- codegraph grounding — *used* — axonivy-prod/luz_docs_import
- codegraph grounding — *used* — graphify
- refine — *produced* — 26 insights
- define_plan — *produced* — 48 decisions
- implement — *produced* — 117 scenarios
- implement — *produced* — 435 steps
- implement — *produced* — 43 fixtures
- implement — *produced* — BDD .feature
- implement — *via* — P4 assured loop
- P4 assured loop — *score* — 0.58
- P4 assured loop — *benchmark* — 0.70
- judge — *caught* — cross-source DUPLICATION
- cross-source DUPLICATION — *is* — behaviours repeated 3-4x
- cross-source DUPLICATION — *generated* — per-source-node without dedup
- TPS — *value* — 0.576
- TPS — *includes metric* — trajectory
- trajectory — *value* — 1.0
- TPS — *includes metric* — scope recall
- scope recall — *value* — 1.0
- TPS — *includes metric* — ac_recall
- ac_recall — *value* — 0.32
- TPS — *includes metric* — oracle_strength
- oracle_strength — *value* — 0.20
- TPS — *includes metric* — precision
- precision — *value* — 0.00
- stack — *status* — WORK
- quality gates — *status* — WORK
- low scores — *indicate* — first-pass suite needing a dedup re-run
- low scores — *are not* — stack failure
- implement — *required* — two-timeout fix
- two-timeout fix — *includes* — client MCP idle
- two-timeout fix — *includes* — gateway A2A_CLIENT_TIMEOUT
- test-agent-v2 — *has related document* — Full local-parity stack for test-agent-v2 (MinIO PG Redis PubSub Ollama laya)
- Full local-parity stack for test-agent-v2 (MinIO PG Redis PubSub Ollama laya) — *describes component* — MinIO
- Full local-parity stack for test-agent-v2 (MinIO PG Redis PubSub Ollama laya) — *describes component* — PG
- Full local-parity stack for test-agent-v2 (MinIO PG Redis PubSub Ollama laya) — *describes component* — Redis
- Full local-parity stack for test-agent-v2 (MinIO PG Redis PubSub Ollama laya) — *describes component* — PubSub
- Full local-parity stack for test-agent-v2 (MinIO PG Redis PubSub Ollama laya) — *describes component* — Ollama
- Full local-parity stack for test-agent-v2 (MinIO PG Redis PubSub Ollama laya) — *describes component* — laya
- test-agent-v2 ran fully local end-to-end — *achieved on* — 2026-09-23

%% ai-graph-end %%