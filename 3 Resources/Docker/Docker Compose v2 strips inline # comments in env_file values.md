---
ai_hash: 85892154ae58deb3
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-22
entities: []
source: session 2026-09-22
status: seedling
tags:
- docker
- docker-compose
- env-file
- gotcha
title: 'Docker Compose v2 strips inline # comments in env_file values'
type: lesson
---

# Docker Compose v2 strips inline # comments in env_file values

Docker Compose v2 (verified v2.39) **strips inline `# comments`** from `env_file` value lines: `LITELLM_MODEL=ollama/qwen2.5:3b   # note` resolves to exactly `ollama/qwen2.5:3b` (confirm with `docker compose config` and grep the service env). GOTCHA/history: older Compose (and `docker run --env-file`) did NOT strip inline comments — everything after `=` became the value, silently corrupting it (a model name with a trailing `# ...`). So inline comments in an env_file are safe on modern Compose but NOT portable to `docker run --env-file` or old versions; when in doubt put comments on their OWN line. Always verify with `docker compose config`. See [[Full local-parity stack for test-agent-v2 (MinIO PG Redis PubSub Ollama laya)]].

## Related

- [[Full local-parity stack for test-agent-v2 (MinIO PG Redis PubSub Ollama laya)]]

%% ai-graph-start %%

**Related notes:**
- [[Compose command ${VAR} reads .env not env_file — use env_file + $$VAR]]
- [[Compose an inline comment on a BLANK env value becomes the value]]
- [[A failed docker compose --build leaves latest on the OLD image (silent stale run)]]
- [[Relocating docker-compose.yml renames the Compose project and orphans volumes]]
- [[Full local-parity stack for test-agent-v2 (MinIO PG Redis PubSub Ollama laya)]]

%% ai-graph-end %%