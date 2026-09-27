---
ai_hash: 32317dbdf56b77d2
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-23
entities:
- test-agent-v2
- LUZ-158390
- test-agent-v2 pipeline
- quality gates
- MILESTONE
- '2026-09-23'
- docker-compose stack
- MinIO
- Postgres
- pgvector
- Redis
- Ollama-embeddings
- Claude-subscription LLM
- claude-proxy
- gather
- 40 nodes
- Atlassian crawl
- memory recall
- codegraph grounding
- axonivy-prod/luz_docs_import
- graphify
- in-container
- refine
- 26 insights
- business rounds
- technical rounds
- approve
- define_plan
- 48 decisions
- methodology
- scope
- metrics
- test-design rounds
- approve_plan
- implement
- 117 scenarios
- 435 steps
- 43 fixtures
- BDD .feature
- P4 assured loop
- evaluate_plan
- RESULTS
- assured loop score
- '0.58'
- '0.70'
- judge
- cross-source DUPLICATION
- behaviours
- source-node
- TPS
- '0.576'
- trajectory
- 1.0 (trajectory)
- scope recall
- 1.0 (scope recall)
- leaked []
- ac_recall
- '0.32'
- oracle_strength
- '0.20'
- 320 weak steps
- precision
- '0.00'
- complete-but-noisy
- system stack
- dedup re-run
- stack failure
- two-timeout fix
- Slow local implement_plan needs TWO timeouts raised client MCP idle + gateway A2A_CLIENT_TIMEOUT
- client MCP idle
- gateway A2A_CLIENT_TIMEOUT
- Full local-parity stack for test-agent-v2 (MinIO PG Redis PubSub Ollama laya)
- PG
- PubSub
- Ollama
- laya
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
- [[run_json_agent needed a per-call timeout or a slow Vertex call hangs implement past the server ceiling]]
- [[test-agent-v2 fixes implement_plan predictive budget guard + gather explore opt-in gate]]
- [[Why local test-agent is slow claude -p ships a 17.5K agent prompt on Opus, x many serial calls]]
- [[Testing-Agent implement_plan assured loop times out at 900s MCP ceiling]]

**Relations:**
- test-agent-v2 — *ran* — fully local end-to-end
- test-agent-v2 — *associated_with* — LUZ-158390
- test-agent-v2 pipeline — *proven* — true
- quality gates — *proven* — true
- MILESTONE — *date* — 2026-09-23
- MILESTONE — *describes* — test-agent-v2 pipeline ran FULLY LOCAL end-to-end
- test-agent-v2 pipeline — *ran_on* — docker-compose stack
- docker-compose stack — *includes* — MinIO
- docker-compose stack — *includes* — Postgres
- Postgres — *uses* — pgvector
- docker-compose stack — *includes* — Redis
- docker-compose stack — *includes* — Ollama-embeddings
- docker-compose stack — *includes* — Claude-subscription LLM
- Claude-subscription LLM — *via* — claude-proxy
- test-agent-v2 pipeline — *has_stage* — gather
- test-agent-v2 pipeline — *has_stage* — codegraph grounding
- test-agent-v2 pipeline — *has_stage* — refine
- test-agent-v2 pipeline — *has_stage* — approve
- test-agent-v2 pipeline — *has_stage* — define_plan
- test-agent-v2 pipeline — *has_stage* — approve_plan
- test-agent-v2 pipeline — *has_stage* — implement
- test-agent-v2 pipeline — *has_stage* — evaluate_plan
- gather — *processed* — 40 nodes
- gather — *involved* — Atlassian crawl
- gather — *involved* — memory recall
- codegraph grounding — *used* — axonivy-prod/luz_docs_import
- codegraph grounding — *used* — graphify
- graphify — *ran_in* — in-container
- refine — *produced* — 26 insights
- refine — *involved* — business rounds
- refine — *involved* — technical rounds
- define_plan — *produced* — 48 decisions
- define_plan — *involved* — methodology
- define_plan — *involved* — scope
- define_plan — *involved* — metrics
- define_plan — *involved* — test-design rounds
- implement — *produced* — 117 scenarios
- implement — *produced* — 435 steps
- implement — *produced* — 43 fixtures
- implement — *produced* — BDD .feature
- implement — *via* — P4 assured loop
- RESULTS — *include* — assured loop score
- assured loop score — *value* — 0.58
- assured loop score — *below_threshold* — 0.70
- judge — *caught* — cross-source DUPLICATION
- cross-source DUPLICATION — *description* — behaviours repeated 3-4x
- behaviours — *generated_per* — source-node
- RESULTS — *include* — TPS
- TPS — *value* — 0.576
- TPS — *has_component* — trajectory
- trajectory — *value* — 1.0 (trajectory)
- TPS — *has_component* — scope recall
- scope recall — *value* — 1.0 (scope recall)
- scope recall — *status* — leaked []
- TPS — *has_component* — ac_recall
- ac_recall — *value* — 0.32
- TPS — *has_component* — oracle_strength
- oracle_strength — *value* — 0.20
- oracle_strength — *implies* — 320 weak steps
- TPS — *has_component* — precision
- precision — *value* — 0.00
- precision — *description* — complete-but-noisy
- system stack — *status* — WORK
- quality gates — *status* — WORK
- low scores — *implies* — needing a dedup re-run
- low scores — *not_imply* — stack failure
- two-timeout fix — *required_for* — implement
- two-timeout fix — *details* — Slow local implement_plan needs TWO timeouts raised client MCP idle + gateway A2A_CLIENT_TIMEOUT
- Slow local implement_plan needs TWO timeouts raised client MCP idle + gateway A2A_CLIENT_TIMEOUT — *includes* — client MCP idle
- Slow local implement_plan needs TWO timeouts raised client MCP idle + gateway A2A_CLIENT_TIMEOUT — *includes* — gateway A2A_CLIENT_TIMEOUT
- test-agent-v2 — *related_to* — Full local-parity stack for test-agent-v2 (MinIO PG Redis PubSub Ollama laya)
- Full local-parity stack for test-agent-v2 (MinIO PG Redis PubSub Ollama laya) — *includes* — MinIO
- Full local-parity stack for test-agent-v2 (MinIO PG Redis PubSub Ollama laya) — *includes* — PG
- Full local-parity stack for test-agent-v2 (MinIO PG Redis PubSub Ollama laya) — *includes* — Redis
- Full local-parity stack for test-agent-v2 (MinIO PG Redis PubSub Ollama laya) — *includes* — PubSub
- Full local-parity stack for test-agent-v2 (MinIO PG Redis PubSub Ollama laya) — *includes* — Ollama
- Full local-parity stack for test-agent-v2 (MinIO PG Redis PubSub Ollama laya) — *includes* — laya

%% ai-graph-end %%