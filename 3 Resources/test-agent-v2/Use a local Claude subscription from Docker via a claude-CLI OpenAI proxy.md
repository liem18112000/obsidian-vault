---
ai_hash: 1ce218910e7e2c7a
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-23
entities: []
source: session 2026-09-23
status: seedling
tags:
- claude
- subscription
- docker
- litellm
- proxy
- claude-code
title: Use a local Claude subscription from Docker via a claude-CLI OpenAI proxy
type: howto
---

# Use a local Claude subscription from Docker via a claude-CLI OpenAI proxy

TECHNIQUE: to let Dockerised services use a local Claude **subscription** (not an API key), run a sidecar that wraps the `claude` CLI behind an OpenAI-compatible endpoint, then point litellm at it. Why: the subscription OAuth token only works via the `claude` CLI / Claude Code, NOT the raw Anthropic API — so litellm/anthropic with an API key cant use it. And a Windows-host `claude.exe` cant run in a Linux container, so you install the LINUX `claude` (`npm i -g @anthropic-ai/claude-code`) INSIDE the sidecar.

Shape (test-agent-v2 `claude_proxy/`): node:20-slim + global claude + a ~50-line Node http shim serving `POST /v1/chat/completions` → flatten messages → `execFile("claude", ["-p", prompt, "--output-format","json"])` → return the `.result` as an OpenAI choice. Compose: mount a `claude_config:/root/.claude` volume so the ONE-TIME login persists (`docker compose exec claude-proxy claude` → /login, or `claude setup-token`). Agents: `LITELLM_MODEL=openai/claude-local`, `LITELLM_API_BASE=http://claude-proxy:8088/v1`, dummy key. This routes BOTH the engine complete() path AND ADK LlmAgent calls through the subscription via the existing litellm provider (no new provider needed). CAVEATS: each call spins up Claude Code (SLOW, seconds of startup — pair with TESTAGENT_TURBO=1) and consumes subscription quota; text-only (no tool/streaming). "NON-LOCAL disable" = config swap: comment the LITELLM_* lines, use Ollama/Anthropic-API/Vertex instead. See [[Pluggable LLM via the litellm ModelProvider backend]].

## Related

- [[Pluggable LLM via the litellm ModelProvider backend]]

%% ai-graph-start %%

**Related notes:**
- [[Pluggable LLM via the litellm ModelProvider backend]]
- [[claude-code npm install breaks in slim Docker — use the official install.sh]]
- [[Why local test-agent is slow claude -p ships a 17.5K agent prompt on Opus, x many serial calls]]
- [[claude-proxy login expires mid-session - implement silently degrades to heuristic stubs (score 0.00)]]
- [[vinnstack spawns the local claude CLI for subscription-authenticated automation]]

%% ai-graph-end %%