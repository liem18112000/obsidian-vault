---
title: "Why local test-agent is slow: claude -p ships a 17.5K agent prompt on Opus, x many serial calls"
created: 2026-09-23
type: lesson
status: seedling
source: "session 2026-09-23"
tags: [performance, claude-proxy, latency, opus, implement, testing-agent]
---

# Why local test-agent is slow: claude -p ships a 17.5K agent prompt on Opus, x many serial calls

ROOT CAUSE (measured) — why the local test-agent pipeline is slow, esp. implement_plan: it is NOT cold-start (claude binary starts in 43ms), NOT resource starvation (12 CPU, load 0.45, 16GB idle), NOT prompt re-processing (the system prompt is server-side CACHED: cache_read=17457, cache_creation=0 across calls). The cost is inherent to using `claude -p` (the subscription via claude-proxy) as an LLM backend: (1) EVERY call is a full Claude Code AGENT TURN carrying a ~17.5K-token system prompt (tools+instructions) — so even `claude -p "say ok"` costs ~2.2s API (in=2,out=4 tokens); a raw Anthropic API call sends ~100 tokens and returns <1s. (2) The default model is `claude-opus-5-5[1m]` (the slowest tier) — a real generation prompt is ~8.5s on opus vs ~6.5s haiku (only ~25% gain, so model is NOT the main lever). (3) The assured loop makes MANY such ~7s calls SERIALLY (generate per source-node → judge → reflect) → N×7s = minutes. gather/refine/define are fine because they make FEW calls; implement makes many. LEVERS by impact: parallelize the agents independent generation calls (claude-proxy is already ThreadingHTTPServer, but the tpd agent fires serially — biggest wall-clock win, needs code); OR a raw Anthropic API key via litellm anthropic/ (no 17.5K agent prompt, ~2-4x faster/call, batchable, but API billing not subscription); OR CLAUDE_MODEL=claude-haiku-4-5 (~25%, easy); OR fewer calls (TESTAGENT_TURBO=1 already on). Measure per-call split via `claude -p ... --output-format json` → duration_ms/duration_api_ms/usage. See [[Slow local implement_plan needs TWO timeouts raised: client MCP idle + gateway A2A_CLIENT_TIMEOUT]] and [[Use a local Claude subscription from Docker via a claude-CLI OpenAI proxy]].

## Related

- [[Use a local Claude subscription from Docker via a claude-CLI OpenAI proxy]]
