---
title: "Repeated compose up -d can corrupt the bridge network — down+up to rebuild it"
created: 2026-09-23
type: lesson
status: seedling
source: "session 2026-09-23"
tags: [docker, docker-compose, networking, rancher, wsl, gotcha]
---

# Repeated compose up -d can corrupt the bridge network — down+up to rebuild it

GOTCHA (Docker/Rancher-WSL): after MANY repeated `docker compose up -d` recreations in a session, the compose bridge network can silently CORRUPT — peer containers on the SAME network/subnet (172.24.0.x, resolvable by name) can no longer reach each other: `socket.create_connection(("db",5432),5)` from the tpd/kga containers TimeOuts even though the db container is healthy and serves psql internally. Symptom in the app: implement_plan (or any DB op) fails with asyncpg `TimeoutError` / SQLAlchemy pool connect timeout — looks like a DB problem but the DB is fine. ROOT: the bridge/iptables state for the persisted network `<project>_default` degraded; `docker compose up -d` recreates CONTAINERS but NOT the network, so it does not fix it. FIX: `docker compose down` (removes the network) + `docker compose up -d` (rebuilds a fresh bridge). Volumes persist across down/up (no -v), so DB data, MinIO object store, Ollama models, and any run checkpoint survive. If down/up alone does not fix it, escalate to `wsl --shutdown` + restart Rancher. DIAGNOSE: `docker inspect <ctr> --format "{{range .NetworkSettings.Networks}}...{{end}}"` (same net?) + a raw `socket.create_connection` peer test from inside a container. See [[v2 local docker-compose run]].
