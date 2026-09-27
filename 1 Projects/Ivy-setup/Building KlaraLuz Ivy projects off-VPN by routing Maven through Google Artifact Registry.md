---
ai_hash: 3f927b144b9195fc
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-07-27
entities:
- Klara/Luz Ivy projects
- Maven
- Google Artifact Registry
- VPN
- '`repo.axongroupio.ch`'
- '`axonivy-prod` Bitbucket'
- '`~/.m2/settings.xml`'
- '`europe-west6-maven.pkg.dev/klara-repo/...`'
- '`com.google.cloud.artifactregistry:artifactregistry-maven-wagon`'
- '`gcloud ADC`'
- '`klara_theme`'
- '`luz_components`'
- '`ch.klara.ivy:luz_ivy_common:2.0.01.0`'
- '`klara_prototype`'
- '`klara_theme:1.00.22.00-SNAPSHOT`'
- '`klara_theme:1.00.48.00-SNAPSHOT`'
- Maven Central
- Ivy `ch` profiles
- Ivy 10.0.15
- Ivy 12
- Artifactory
source: session 2026-07-27 (Ivy setup)
status: seedling
tags:
- axon-ivy
- maven
- gcloud
- artifact-registry
- vpn
- klara
- luz
- howto
title: Building Klara/Luz Ivy projects off-VPN by routing Maven through Google Artifact
  Registry
type: howto
---

# Building Klara/Luz Ivy projects off-VPN by routing Maven through Google Artifact Registry

The Klara/Luz Axon Ivy projects (`axonivy-prod` Bitbucket) resolve internal Maven artifacts from **two** places, and off the corporate VPN only one works:

- `repo.axongroupio.ch` (JFrog Artifactory) — private IP `10.124.0.59`, **VPN-only**; off-VPN every request times out (~6s each) and that timeout is what fails the build. It is injected by the `quarkus`/`ch` profiles in `~/.m2/settings.xml`.
- **Google Artifact Registry** `europe-west6-maven.pkg.dev/klara-repo/...` — declared as `<repositories>` in the project poms, using the `com.google.cloud.artifactregistry:artifactregistry-maven-wagon` extension (`.mvn/extensions.xml`) + **gcloud ADC**. Reachable over the public internet.

**Recipe to build off-VPN:** run Maven with a settings.xml that KEEPS Maven Central + the Ivy `ch` properties (`ivyVersion=10.0.15`, engine dir `~/.m2/repository/.cache/ivy/10.0.15`) but DROPS the axongroupio repositories/pluginRepositories, and ensure `gcloud auth application-default print-access-token` works. Add `-U` so snapshot metadata refreshes from GAR instead of the dead host. Verified: `klara_theme` (plain `jar`) builds SUCCESS; `luz_components` `validate` SUCCESS (release parent `ch.klara.ivy:luz_ivy_common:2.0.01.0` resolves from GAR).

**Residual blocker (not infra):** GAR snapshot **retention is short**, so old pinned internal SNAPSHOTs are purged. `klara_prototype` master pins `klara_theme:1.00.22.00-SNAPSHOT` (timestamp `20250725.044911-3`) which no longer exists in GAR (current master theme is `1.00.48.00-SNAPSHOT`) → `Could not find artifact`. Fixing that needs either the corporate VPN (Artifactory retains more snapshots), bumping the pin to a current version (source change, API-drift risk), or building the matching klara_theme commit locally. See [[KlaraLuz Axon Ivy projects on master still target Ivy 10.0.15, not 12]].

## Related
[[KlaraLuz Axon Ivy projects on master still target Ivy 10.0.15, not 12]]
[[Clone a Bitbucket repo with an app password without leaking it (inline credential helper)]]

## Related

- [[KlaraLuz Axon Ivy projects on master still target Ivy 10.0.15, not 12]]
- [[Clone a Bitbucket repo with an app password without leaking it (inline credential helper)]]

%% ai-graph-start %%

**Related notes:**
- [[KlaraLuz Maven builds resolve dependencies from Google Artifact Registry and require gcloud auth]]
- [[KlaraLuz Axon Ivy projects on master still target Ivy 10.0.15, not 12]]
- [[Klara Cloud Build pushes images to klara-repo Artifact Registry with the SA on the trigger]]
- [[gather_codebase needs axonivy-prodrepo workspace slug]]
- [[Vinnstack Cloud Build trigger lives in klara-infra, not klara-nonprod]]

**Relations:**
- Klara/Luz Ivy projects — *resolve artifacts from* — `repo.axongroupio.ch`
- Klara/Luz Ivy projects — *resolve artifacts from* — Google Artifact Registry
- `repo.axongroupio.ch` — *is a type of* — Artifactory
- `repo.axongroupio.ch` — *requires* — VPN
- Google Artifact Registry — *is accessible via* — public internet
- Maven — *routes through* — Google Artifact Registry
- Klara/Luz Ivy projects — *are stored in* — `axonivy-prod` Bitbucket
- Maven — *uses* — `~/.m2/settings.xml`
- Google Artifact Registry — *has URL* — `europe-west6-maven.pkg.dev/klara-repo/...`
- Google Artifact Registry — *uses extension* — `com.google.cloud.artifactregistry:artifactregistry-maven-wagon`
- Google Artifact Registry — *uses authentication* — `gcloud ADC`
- `klara_theme` — *is a* — jar
- `luz_components` — *uses* — `ch.klara.ivy:luz_ivy_common:2.0.01.0`
- `klara_prototype` — *pins* — `klara_theme:1.00.22.00-SNAPSHOT`
- Google Artifact Registry — *has property* — short snapshot retention
- Artifactory — *has property* — retains more snapshots
- Klara/Luz Ivy projects — *target Ivy version* — Ivy 10.0.15
- Klara/Luz Ivy projects — *do not target Ivy version* — Ivy 12
- Maven — *uses* — Maven Central
- `~/.m2/settings.xml` — *injects* — Ivy `ch` profiles
- Ivy `ch` profiles — *specify Ivy version* — Ivy 10.0.15
- Ivy 10.0.15 — *has engine directory* — `~/.m2/repository/.cache/ivy/10.0.15`
- `klara_theme:1.00.22.00-SNAPSHOT` — *is an older version of* — `klara_theme`
- `klara_theme:1.00.48.00-SNAPSHOT` — *is a current version of* — `klara_theme`
- Maven — *builds* — `klara_theme`
- Maven — *builds* — `luz_components`
- Maven — *builds* — `klara_prototype`
- Klara/Luz Ivy projects — *are* — Axon Ivy projects

%% ai-graph-end %%