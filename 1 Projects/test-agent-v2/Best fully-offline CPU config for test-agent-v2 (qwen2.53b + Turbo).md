---
title: "Best fully-offline CPU config for test-agent-v2 (qwen2.5:3b + Turbo)"
created: 2026-09-22
type: howto
status: seedling
source: "session 2026-09-22"
tags: [test-agent, ollama, cpu, offline, qwen, laya]
---

# Best fully-offline CPU config for test-agent-v2 (qwen2.5:3b + Turbo)

For a fully-offline, CPU-only, ~2-4GB (model budget) run of test-agent-v2, the tuned `.env.compose`: `LITELLM_MODEL=ollama/qwen2.5:3b` (best ~3B for JSON/instructions at ~2GB q4; qwen2.5:1.5b for more speed), `TESTAGENT_TURBO=1` (fewer/faster LLM gates — critical on CPU where long generations are slow), leave `TPD_LLM_DETAIL` unset (1 LLM call). laya (`TPD_DECISION_BACKEND=laya`) is KEPT even on CPU: it is a small (~300-420M) NON-autoregressive model doing one forward pass per decision, so it is CPU-viable (~100s of ms), unlike the autoregressive LLM.

HONEST CEILING: this only makes the *model* fit ~2-4GB and only proves wiring / gives weak plans; it does NOT approach Sonnet-5 quality (see [[A 2-4GB local model cannot match Sonnet 5 — plug the real API instead]]). And the *whole containerized stack* (Postgres+MinIO+Redis+Ollama+laya-torch+5 agents+worker) needs ~8-12GB TOTAL machine RAM regardless of the model — 2-4GB TOTAL will not boot it. See [[Local LLM choice for the test-agent workload (Ollama)]].

## Related

- [[A 2-4GB local model cannot match Sonnet 5 — plug the real API instead]]
- [[Local LLM choice for the test-agent workload (Ollama)]]
