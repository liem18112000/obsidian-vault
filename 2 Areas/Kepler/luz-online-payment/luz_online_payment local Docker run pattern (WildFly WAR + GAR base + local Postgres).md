---
ai_hash: 856b28768230d28e
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-07-29
entities:
- luz_online_payment
- Docker
- WildFly
- WAR
- GAR
- Postgres
- docker-compose
- Dockerfile
- luz-wildfly26-all
- gcloud auth configure-docker
- mvn clean install -Dmaven.test.skip=true
- Flyway
- src/main/resources/db/public
- db/migration
- V<version>__desc.sql
- WildFly datasource
- JWT/luzsec
- luz_compensation
- luz_online
- luz_eletter
- host.docker.internal:8080
- kubectl port-forward
- api-forwarder
- LUZ-157476
- luz_store
source: session 2026-07-29; docs/LOCAL-RUN.md
status: seedling
tags:
- luz
- docker
- wildfly
- local-run
- postgres
- flyway
title: luz_online_payment local Docker run pattern (WildFly WAR + GAR base + local
  Postgres)
type: howto
---

# luz_online_payment local Docker run pattern (WildFly WAR + GAR base + local Postgres)

luz WildFly services (e.g. luz_online_payment) run locally via docker-compose with this shape:

1. The Dockerfile is thin: it `COPY target/<artifact>.war` onto a private base image `europe-west6-docker.pkg.dev/klara-repo/.../luz-wildfly26-all`. So you must (a) `gcloud auth configure-docker europe-west6-docker.pkg.dev` to pull the base image, and (b) build the WAR first with `mvn clean install -Dmaven.test.skip=true` before `docker compose up --build`.
2. DB schema is NOT pre-baked: the app runs Flyway-style migrations from `src/main/resources/db/public` + `db/migration` (files named `V<version>__desc.sql`) against an EMPTY database at startup. So a plain empty Postgres with the right db name is enough.
3. WildFly datasource uses `prefill=true`, so it connects at boot — the DB must be up first. In compose, gate the app on a Postgres `healthcheck` via `depends_on: condition: service_healthy`.
4. Cross-service integrations point at `host.docker.internal:8080` (JWT/luzsec, luz_compensation, luz_online, luz_eletter). They are called lazily at runtime, so the app boots without them; run the real services or a `kubectl port-forward services/api-forwarder 8080` to the dev cluster for full functionality.
5. luz_online_payment local ports: app 8128->8080, debug 8788, bundled Postgres 6666->5432. App base path `/luz_online_payment/api`.

See [[LUZ-157476 decline-code flow luz-online-payment forwards, luz_store maps]] for the feature context.

## Related

- [[LUZ-157476 decline-code flow luz-online-payment forwards, luz_store maps]]

%% ai-graph-start %%

**Related notes:**
- [[Run luz_docs_statistic locally with docker-compose]]
- [[Port forward and Docker compose]]
- [[How to Start Invoice Run v2]]
- [[Run local luz-jsonstore against a real tenant GKE Mongo via port-forwards]]
- [[Build and roll out luz-jsonstore to dev (Cloud Build trigger + Deployment rollout)]]

**Relations:**
- luz_online_payment — *runs_via* — docker-compose
- luz_online_payment — *is_a_service_type* — WildFly
- Dockerfile — *copies* — WAR
- Dockerfile — *uses_base_image* — luz-wildfly26-all
- luz-wildfly26-all — *is_a* — GAR base
- gcloud auth configure-docker — *authenticates_for_pulling* — luz-wildfly26-all
- mvn clean install -Dmaven.test.skip=true — *builds* — WAR
- luz_online_payment — *uses* — Flyway
- Flyway — *migrates_from_path* — src/main/resources/db/public
- Flyway — *migrates_from_path* — db/migration
- Flyway — *uses_file_pattern* — V<version>__desc.sql
- Flyway — *targets_database* — Postgres
- WildFly datasource — *uses_setting* — prefill=true
- WildFly datasource — *connects_to* — Postgres
- docker-compose — *gates_app_on* — Postgres
- Postgres — *has_condition* — service_healthy
- luz_online_payment — *integrates_with* — JWT/luzsec
- luz_online_payment — *integrates_with* — luz_compensation
- luz_online_payment — *integrates_with* — luz_online
- luz_online_payment — *integrates_with* — luz_eletter
- integrations — *point_at* — host.docker.internal:8080
- kubectl port-forward — *forwards_to* — api-forwarder
- luz_online_payment — *app_port_mapping* — 8128->8080
- luz_online_payment — *debug_port* — 8788
- luz_online_payment — *bundled_Postgres_port_mapping* — 6666->5432
- luz_online_payment — *base_path* — /luz_online_payment/api
- LUZ-157476 — *provides_feature_context_for* — luz_online_payment
- LUZ-157476 — *involves* — luz_online_payment
- LUZ-157476 — *involves* — luz_store

%% ai-graph-end %%