---
ai_hash: a27237a012af24b5
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-06-08
entities:
- LEO CDP
- SYSTEM_ENV_VARS
- ArangoDB
- DatabaseConfigs.loadFromFile()
- IllegalArgumentin('File is not found')
- ARANGODB_* env vars
- runtimeEnvironment
- PRO
- leocdp-metadata.properties
- integration tests
- ARANGODB_HOST
- ARANGODB_PORT
- ARANGODB_USERNAME
- ARANGODB_PASSWORD
- ARANGODB_DATABASE
- Wall of NoClassDefFoundError
- static-init IO
- unit tests
- live DB
- configs/database-configs.json
- configs/PRO-database-configs.json
- mainDatabaseConfig
- env-var connection
source: LEO CDP local integration-test wiring, 2026-06-08
status: seedling
tags:
- leo-cdp
- arangodb
- config
- integration-tests
- gotcha
title: LEO CDP SYSTEM_ENV_VARS still requires database-configs.json to exist first
type: gotcha
---

# LEO CDP SYSTEM_ENV_VARS still requires database-configs.json to exist first

LEO CDP gotcha: even with mainDatabaseConfig=SYSTEM_ENV_VARS (which builds the ArangoDB connection from ARANGODB_* env vars), DatabaseConfigs.loadFromFile() still reads configs/database-configs.json FIRST and throws IllegalArgumentin('File is not found') if absent - the SYSTEM_ENV_VARS env-var branch sits AFTER the file read, so it is unreachable when the file is missing. Also runtimeEnvironment=PRO in leocdp-metadata.properties rewrites the path to configs/PRO-database-configs.json. To run integration tests against a live DB with env-var connection: (1) set runtimeEnvironment= empty, (2) drop a minimal configs/database-configs.json = {"configs":{}} (content irrelevant - the env branch overrides), (3) export ARANGODB_HOST/PORT/USERNAME/PASSWORD/DATABASE. Both files are gitignored. Design smell: a config 'use env vars' mode that still hard-requires the file it is meant to replace.

## Related

- [[Wall of NoClassDefFoundError on first test run = static-init IO, split unit from integration]]

%% ai-graph-start %%

**Related notes:**
- [[Running the LEO CDP GHCR image needs mounted configs (image ships JARs only)]]
- [[LEO CDP CI provisions deps CI-natively, pinned to devops-script versions for parity]]
- [[Wall of NoClassDefFoundError on first test run = static-init IO, split unit from integration]]
- [[CI-driven CD cannot resolve local gitignored Terraform state — needs remote backend or IPs via secrets]]
- [[LEO CDP schema changes must go in both database-schema.sql and a migrations file]]

**Relations:**
- LEO CDP — *uses* — SYSTEM_ENV_VARS
- SYSTEM_ENV_VARS — *is a setting for* — mainDatabaseConfig
- SYSTEM_ENV_VARS — *requires* — configs/database-configs.json
- SYSTEM_ENV_VARS — *builds* — env-var connection
- env-var connection — *for* — ArangoDB
- env-var connection — *from* — ARANGODB_* env vars
- DatabaseConfigs.loadFromFile() — *reads* — configs/database-configs.json
- DatabaseConfigs.loadFromFile() — *throws* — IllegalArgumentin('File is not found')
- runtimeEnvironment — *is set to* — PRO
- PRO — *in* — leocdp-metadata.properties
- PRO — *rewrites path to* — configs/PRO-database-configs.json
- ARANGODB_* env vars — *includes* — ARANGODB_HOST
- ARANGODB_* env vars — *includes* — ARANGODB_PORT
- ARANGODB_* env vars — *includes* — ARANGODB_USERNAME
- ARANGODB_* env vars — *includes* — ARANGODB_PASSWORD
- ARANGODB_* env vars — *includes* — ARANGODB_DATABASE
- integration tests — *run against* — live DB
- live DB — *uses* — env-var connection
- configs/database-configs.json — *is* — gitignored
- configs/PRO-database-configs.json — *is* — gitignored
- Wall of NoClassDefFoundError — *is related to* — static-init IO
- Wall of NoClassDefFoundError — *is related to* — split unit from integration
- LEO CDP — *has related issue* — Wall of NoClassDefFoundError
- SYSTEM_ENV_VARS — *overrides content of* — configs/database-configs.json
- unit tests — *are distinct from* — integration tests

%% ai-graph-end %%