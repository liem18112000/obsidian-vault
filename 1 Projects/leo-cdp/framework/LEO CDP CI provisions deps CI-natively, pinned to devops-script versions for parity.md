---
ai_hash: 50a49789e1953c8b
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-06-06
entities:
- LEO CDP CI
- dependencies
- JDK
- CI-native provisioning
- devops-script
- '`validate` job'
- '`.github/workflows/ci-cd.yml`'
- GitHub `services:` containers
- '`actions/setup-java` action'
- VM-oriented provisioning scripts
- '`devops-script/docker-arangodb/start.sh`'
- '`install-java.sh`'
- ephemeral runner
- long-lived hosts
- production parity
- version-pinning
- '`services:` images'
- '`devops-script/docker-arangodb` compose'
- '`arangodb:3.11.14`'
- '`redis:7.4`'
- deployment
- CI provisioning
- GitHub Actions runners
- '`JAVA_HOME`'
- '`PATH`'
- '`docker-compose` v1→v2 shim'
- manual readiness wait loop
- '`JAVA_HOME` pin hack'
- single source of truth
- CI
- load-bearing facts
- versions
- ports
- config
source: leo-cdp-framework ci-cd.yml work 2026-06-06
status: seedling
tags:
- leo-cdp
- ci
- github-actions
- devops
- decision
title: LEO CDP CI provisions deps CI-natively, pinned to devops-script versions for
  parity
type: lesson
---

# LEO CDP CI provisions deps CI-natively, pinned to devops-script versions for parity

The CI `validate` job in `.github/workflows/ci-cd.yml` provisions its dependencies and JDK **CI-natively** — GitHub `services:` containers + the `actions/setup-java` action — rather than invoking the VM-oriented `devops-script` provisioning scripts. (An earlier attempt to call `devops-script/docker-arangodb/start.sh` + `install-java.sh` was reverted as the wrong tradeoff for an ephemeral runner.)

**Why CI-native wins here:** `services:` containers are auto health-gated and torn down; `setup-java` is cached and deterministic. Calling the VM scripts instead forced a `docker-compose` v1→v2 shim, a manual readiness wait loop, and a `JAVA_HOME` pin hack — fragility for little gain. The devops scripts are designed for long-lived hosts (`/build/cdp-instance`, `nohup`, persistent volumes, `restart: unless-stopped`), not ephemeral CI.

**Keeping the legitimate kernel of "use devops-script" — production parity — without the fragility:** pin the `services:` images to the SAME versions the `devops-script/docker-arangodb` compose declares (`arangodb:3.11.14`, `redis:7.4`). Parity is the real goal; version-pinning achieves it. `devops-script` remains the source of truth for *deployment*, which is its actual purpose.

**General principle:** "single source of truth" for *deployment* scripts should not be force-fit onto *CI* provisioning — instead share the load-bearing facts (versions, ports, config) and let each environment use its idiomatic mechanism.

## Related
[[GitHub Actions runners pick JDK from inherited JAVA_HOME, not PATH]]
[[Shim legacy docker-compose v1 to docker compose v2 on GitHub runners]]

## Related

- [[GitHub Actions runners pick JDK from inherited JAVA_HOME, not PATH]]
- [[Shim legacy docker-compose v1 to docker compose v2 on GitHub runners]]

%% ai-graph-start %%

**Related notes:**
- [[Running the LEO CDP GHCR image needs mounted configs (image ships JARs only)]]
- [[GitHub Actions runners pick JDK from inherited JAVA_HOME, not PATH]]
- [[Shim legacy docker-compose v1 to docker compose v2 on GitHub runners]]
- [[LEO CDP SYSTEM_ENV_VARS still requires database-configs.json to exist first]]
- [[leo-customer360 CD builds images on the VM instead of pulling from GHCR (CICD gap)]]

**Relations:**
- LEO CDP CI — *provisions* — dependencies
- LEO CDP CI — *provisions* — JDK
- LEO CDP CI — *uses* — CI-native provisioning
- `validate` job — *is defined in* — `.github/workflows/ci-cd.yml`
- `validate` job — *uses* — CI-native provisioning
- CI-native provisioning — *leverages* — GitHub `services:` containers
- CI-native provisioning — *leverages* — `actions/setup-java` action
- CI-native provisioning — *is preferred over* — VM-oriented provisioning scripts
- VM-oriented provisioning scripts — *include* — `devops-script/docker-arangodb/start.sh`
- VM-oriented provisioning scripts — *include* — `install-java.sh`
- VM-oriented provisioning scripts — *are designed for* — long-lived hosts
- VM-oriented provisioning scripts — *are unsuitable for* — ephemeral runner
- devops-script — *is* — single source of truth
- single source of truth — *is for* — deployment
- devops-script — *declares versions for* — `devops-script/docker-arangodb` compose
- `devops-script/docker-arangodb` compose — *declares version* — `arangodb:3.11.14`
- `devops-script/docker-arangodb` compose — *declares version* — `redis:7.4`
- version-pinning — *achieves* — production parity
- `services:` images — *are pinned to versions from* — `devops-script/docker-arangodb` compose
- production parity — *is goal of* — version-pinning
- VM-oriented provisioning scripts — *required* — `docker-compose` v1→v2 shim
- VM-oriented provisioning scripts — *required* — manual readiness wait loop
- VM-oriented provisioning scripts — *required* — `JAVA_HOME` pin hack
- single source of truth — *should not be force-fit onto* — CI provisioning
- CI provisioning — *should share* — load-bearing facts
- load-bearing facts — *include* — versions
- load-bearing facts — *include* — ports
- load-bearing facts — *include* — config
- GitHub Actions runners — *pick JDK from* — `JAVA_HOME`
- GitHub Actions runners — *do not pick JDK from* — `PATH`
- CI — *uses* — ephemeral runner
- deployment — *uses* — long-lived hosts
- LEO CDP CI — *aims for* — production parity
- CI-native provisioning — *is* — cached
- CI-native provisioning — *is* — deterministic
- GitHub `services:` containers — *are* — auto health-gated
- GitHub `services:` containers — *are* — torn down
- devops-script — *is actual purpose* — deployment

%% ai-graph-end %%