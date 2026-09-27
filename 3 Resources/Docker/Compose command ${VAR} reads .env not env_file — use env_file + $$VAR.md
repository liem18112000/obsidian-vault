---
ai_hash: 78638971856b23bb
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-22
entities: []
source: session 2026-09-22
status: seedling
tags:
- docker-compose
- env-file
- interpolation
- gotcha
title: Compose command ${VAR} reads .env not env_file — use env_file + $$VAR
type: lesson
---

# Compose command ${VAR} reads .env not env_file — use env_file + $$VAR

GOTCHA: a `${VAR}` inside a docker-compose service `command`/`entrypoint` is interpolated by Compose at PARSE time from the shell env + the auto-loaded `.env` file — NOT from a services `env_file:`. So an init container `entrypoint: sh -c "ollama pull ${OLLAMA_CHAT_MODEL:-llama3.2}"` silently pulled the DEFAULT (llama3.2) even though `.env.compose` (the env_file) set a different model — the agents then ran configured for a model that was never pulled. FIX: give the service `env_file: .env.compose` AND escape the `$` as `$$` so Compose passes it through to the CONTAINER shell, which expands it from env_file: `sh -c "ollama pull \"$${OLLAMA_CHAT_MODEL:-llama3.2}\""`. Rule: `${}` = compose/.env interpolation; `$${}` = runtime container-shell (reads env_file). See [[Docker Compose v2 strips inline # comments in env_file values]].

## Related

- [[Docker Compose v2 strips inline # comments in env_file values]]

%% ai-graph-start %%

**Related notes:**
- [[Docker Compose v2 strips inline # comments in env_file values]]
- [[Docker Compose command blocks need $$ to defer variable expansion to the container shell]]
- [[Docker Compose path resolution env_file vs build context vs dockerfile]]
- [[Compose an inline comment on a BLANK env value becomes the value]]
- [[A failed docker compose --build leaves latest on the OLD image (silent stale run)]]

%% ai-graph-end %%