---
ai_hash: 62f1f8743a0e6658
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-06-06
entities:
- LEO CDP GHCR image
- ghcr.io/trieu/leo-cdp-framework
- /app
- leo-main-starter-docker.jar
- observer JAR
- scheduler JAR
- data-processing JAR
- deps/
- resources/
- public/
- configs/
- leocdp-metadata.properties
- Gradle
- devops-script/shell-script-starter/start-*.sh
- VM-based deployment
- docker-compose
- HTTP 200
- /
- /login
- /ping
- 0.0.0.0:9070
- Database credentials
- Environment variables
- mainDatabaseConfig
- SYSTEM_ENV_VARS
- ARANGODB_HOST
- ARANGODB_PORT
- ARANGODB_USERNAME
- ARANGODB_PASSWORD
- ARANGODB_DATABASE
- configs/[PRO-]database-configs.json
- server.listen(port, host)
- configured host
- cdpsys.admin
- 0.0.0.0
- http-routing-configs.json
- Redis
- configs/redis-configs.json
- configs/redis-connection-pool-configs.json
- rfx-core
- UA parser
- configs/regexes.yaml
- HTTP 000
- curl
- setup-system-with-password command
- collections
- super-admin
- :latest tag
- main release
- commit SHA
- feature-branch builds
- image `5f688f0`
- JourneyMapManagement.initDefaultSystemData()
- JourneyMap.setTouchpointHubsForJourneyMap
- touchpoint hub
- default journey map
- core-leo-cdp/devops-script/docker-leocdp/
- Same-repo branch push fires both push and pull_request events (duplicate CI runs)
- LEO CDP CI provisions deps CI-natively, pinned to devops-script versions for parity
source: leo-cdp-framework docker-leocdp 2026-06-06
status: seedling
tags:
- leo-cdp
- docker
- docker-compose
- deployment
- gotcha
title: Running the LEO CDP GHCR image needs mounted configs (image ships JARs only)
type: howto
---

# Running the LEO CDP GHCR image needs mounted configs (image ships JARs only)

The published `ghcr.io/trieu/leo-cdp-framework` image is **not runnable standalone** — `/app` contains only the four starter JARs (`leo-main-starter-docker.jar`, observer, scheduler, data-processing) + `deps/` + `resources/` + `public/`. It has **no `configs/` and no `leocdp-metadata.properties`** (the Gradle config-copy excludes env-specific files and the metadata file is gitignored). The repos intended deployment was VM-based (`devops-script/shell-script-starter/start-*.sh` reading configs on the host), so there was no app docker-compose.

To run it via docker-compose (verified working — admin serves HTTP 200 on `/`, `/login`, `/ping` at `0.0.0.0:9070`), mount a runtime config set over `/app` and inject DB creds via env. Gotchas, each found by iterating on a real boot error:
1. **DB creds via env:** set `mainDatabaseConfig=SYSTEM_ENV_VARS` in `leocdp-metadata.properties`, pass `ARANGODB_HOST/PORT/USERNAME/PASSWORD/DATABASE`. (A `configs/[PRO-]database-configs.json` must still *exist* — the loader reads the file before applying env.)
2. **Host bind:** `server.listen(port, host)` binds the configured host; the repo sample uses `cdpsys.admin` (wont bind in a container) → set the worker host to `0.0.0.0` in `http-routing-configs.json`.
3. **Redis:** the app needs BOTH `configs/redis-configs.json` (host/port/auth) AND `configs/redis-connection-pool-configs.json` (rfx-core pool tuning) — point redis-configs at the redis service, empty `auth` for a no-auth redis.
4. **UA parser:** every HTTP request parses the user-agent via rfx-core, which reads `configs/regexes.yaml` — missing it makes each request throw and reset the connection (curl shows HTTP 000 even though the server is listening). Ship the repos `configs/` wholesale to avoid one-missing-file-at-a-time.
5. **First run:** `docker compose run --rm <svc> setup-system-with-password <pw>` creates collections + super-admin.
6. **Image tag:** `:latest` only exists after a `main` release; feature-branch builds are tagged with the commit SHA only.

Bug observed in image `5f688f0`: `setup-system-with-password` exits 1 (after creating collections + admin) because `JourneyMapManagement.initDefaultSystemData()` seeds 1 touchpoint hub but `JourneyMap.setTouchpointHubsForJourneyMap` requires >=2. Non-fatal to running; default journey map just isnt seeded.

Deploy folder created: `core-leo-cdp/devops-script/docker-leocdp/`.

## Related
- [[Same-repo branch push fires both push and pull_request events (duplicate CI runs)]]
- [[LEO CDP CI provisions deps CI-natively, pinned to devops-script versions for parity]]

## Related

- [[LEO CDP CI provisions deps CI-natively, pinned to devops-script versions for parity]]

%% ai-graph-start %%

**Related notes:**
- [[LEO CDP CI provisions deps CI-natively, pinned to devops-script versions for parity]]
- [[leo-customer360 CD builds images on the VM instead of pulling from GHCR (CICD gap)]]
- [[LEO CDP SYSTEM_ENV_VARS still requires database-configs.json to exist first]]
- [[CD deploy can transiently 404 on a just-built GHCR digest - re-run the failed job]]
- [[CI-driven CD cannot resolve local gitignored Terraform state — needs remote backend or IPs via secrets]]

**Relations:**
- ghcr.io/trieu/leo-cdp-framework — *IS_A* — LEO CDP GHCR image
- LEO CDP GHCR image — *IS_NOT* — runnable standalone
- /app — *CONTAINS* — leo-main-starter-docker.jar
- /app — *CONTAINS* — observer JAR
- /app — *CONTAINS* — scheduler JAR
- /app — *CONTAINS* — data-processing JAR
- /app — *CONTAINS* — deps/
- /app — *CONTAINS* — resources/
- /app — *CONTAINS* — public/
- LEO CDP GHCR image — *LACKS* — configs/
- LEO CDP GHCR image — *LACKS* — leocdp-metadata.properties
- Gradle — *EXCLUDES* — env-specific files
- Gradle — *EXCLUDES* — leocdp-metadata.properties
- leocdp-metadata.properties — *IS* — gitignored
- devops-script/shell-script-starter/start-*.sh — *READS* — configs
- devops-script/shell-script-starter/start-*.sh — *USED_FOR* — VM-based deployment
- docker-compose — *RUNS* — LEO CDP GHCR image
- docker-compose — *REQUIRES* — mounted configs
- docker-compose — *REQUIRES* — injected Database credentials
- admin — *SERVES* — HTTP 200
- admin — *SERVES_ON* — / HTTP 200
- admin — *SERVES_ON* — /login HTTP 200
- admin — *SERVES_ON* — /ping HTTP 200
- admin — *LISTENS_AT* — 0.0.0.0:9070
- mainDatabaseConfig — *SET_TO* — SYSTEM_ENV_VARS
- SYSTEM_ENV_VARS — *IS_IN* — leocdp-metadata.properties
- Database credentials — *PASSED_VIA* — Environment variables
- Environment variables — *INCLUDE* — ARANGODB_HOST
- Environment variables — *INCLUDE* — ARANGODB_PORT
- Environment variables — *INCLUDE* — ARANGODB_USERNAME
- Environment variables — *INCLUDE* — ARANGODB_PASSWORD
- Environment variables — *INCLUDE* — ARANGODB_DATABASE
- configs/[PRO-]database-configs.json — *MUST* — exist
- configs/[PRO-]database-configs.json — *READ_BY* — loader
- loader — *APPLIES* — Environment variables
- server.listen(port, host) — *BINDS* — configured host
- cdpsys.admin — *IS_A* — repo sample
- cdpsys.admin — *WILL_NOT* — bind in a container
- worker host — *SHOULD_BE* — 0.0.0.0
- worker host — *IS_IN* — http-routing-configs.json
- Redis — *NEEDS* — configs/redis-configs.json
- Redis — *NEEDS* — configs/redis-connection-pool-configs.json
- configs/redis-configs.json — *CONTAINS* — host
- configs/redis-configs.json — *CONTAINS* — port
- configs/redis-configs.json — *CONTAINS* — auth
- configs/redis-connection-pool-configs.json — *FOR* — rfx-core pool tuning
- configs/redis-configs.json — *POINTS_AT* — redis service
- auth — *SHOULD_BE* — empty
- UA parser — *PARSES* — user-agent
- UA parser — *USES* — rfx-core
- rfx-core — *READS* — configs/regexes.yaml
- missing configs/regexes.yaml — *CAUSES* — request throw
- missing configs/regexes.yaml — *CAUSES* — connection reset
- missing configs/regexes.yaml — *SHOWS* — HTTP 000
- repos configs/ — *SHOULD_BE* — shipped wholesale
- setup-system-with-password command — *CREATES* — collections
- setup-system-with-password command — *CREATES* — super-admin
- :latest tag — *EXISTS_AFTER* — main release
- feature-branch builds — *TAGGED_WITH* — commit SHA
- image `5f688f0` — *HAS_BUG* — setup-system-with-password exits 1
- setup-system-with-password — *EXITS_BECAUSE* — JourneyMap.setTouchpointHubsForJourneyMap requires >=2 touchpoint hubs
- JourneyMapManagement.initDefaultSystemData() — *SEEDS* — 1 touchpoint hub
- core-leo-cdp/devops-script/docker-leocdp/ — *IS_A* — deploy folder
- LEO CDP GHCR image — *RELATED_TO* — Same-repo branch push fires both push and pull_request events (duplicate CI runs)
- LEO CDP GHCR image — *RELATED_TO* — LEO CDP CI provisions deps CI-natively, pinned to devops-script versions for parity

%% ai-graph-end %%