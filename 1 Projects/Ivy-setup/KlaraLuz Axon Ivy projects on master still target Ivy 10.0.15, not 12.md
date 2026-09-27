---
ai_hash: 4bbf56054305c1cf
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-07-27
entities:
- KlaraLuz Axon Ivy projects
- master branch
- Ivy 10.0.15
- Ivy 12
- Axon Ivy repos
- axonivy-prod
- Bitbucket
- Ivy Designer 12.0.16
- klara_prototype/pom.xml
- luz_components/pom.xml
- klara_theme/pom.xml
- ch.ivyteam.ivy:project-build-plugin
- ch.klara.ivy:luz_ivy_common:2.0.01.0
- Maven
- CLI Maven builds
- Ivy engine 10.0.15
- Ivy cache path
- Maven settings.xml
- ch Maven profile
- Ivy-10 projects
- migration prompt
- Ivy Designer 10.0.16
- Dependency graph
- luz_ivy_common
- klara_faces
- luz_templates
- luz_common
- xent_* modules
- incamail
- ch.ivyteam.ivy.addons
- corporate registry
- Google Artifact Registry
- Google Artifact Registry URL
- dependency resolution repositories
- repo.axongroupio.ch/artifactory
- EclipseIvy Designer workspace
- .prefs files
source: session 2026-07-27 (Ivy setup)
status: seedling
tags:
- axon-ivy
- klara
- luz
- maven
- version-mismatch
- gotcha
title: Klara/Luz Axon Ivy projects on master still target Ivy 10.0.15, not 12
type: observation
---

# Klara/Luz Axon Ivy projects on master still target Ivy 10.0.15, not 12

As of 2026-07-27, the `master` branches of the Klara/Luz Axon Ivy repos (`axonivy-prod` on Bitbucket) still build against **Ivy 10.0.15**, despite a freshly downloaded **Ivy Designer 12.0.16**. Do not assume "master == latest Ivy".

Evidence:
- `klara_prototype/pom.xml`: `<ivyVersion>10.0.15</ivyVersion>`, `<ivy.engine.version>10.0.15</ivy.engine.version>`, packaging `iar`, uses `ch.ivyteam.ivy:project-build-plugin:${ivyVersion}`.
- `luz_components/pom.xml`: packaging `iar`, parent `ch.klara.ivy:luz_ivy_common:2.0.01.0` (Ivy version defined in that uncloned parent).
- `klara_theme/pom.xml`: packaging **`jar`** (plain PrimeFaces/JSF theme lib) — not Ivy-versioned, builds with plain Maven.

Implications:
- **CLI Maven builds** are fine on Ivy 10.0.15: the pom pins `project-build-plugin` 10.0.15 and the engine 10.0.15 is already cached at `~/.m2/repository/.cache/ivy/10.0.15`; the existing `~/.m2/settings.xml` `ch` profile already matches (no rewiring needed).
- **Ivy Designer 12.0.16 mismatches** these Ivy-10 projects — opening/running them in it triggers a migration prompt; to develop in-IDE you want Designer **10.0.16** (the guide's version).
- **Dependency graph exceeds the 3 cloned repos**: builds also need `luz_ivy_common` (parent), `klara_faces`, `luz_templates`, `luz_common`, `xent_*` (6+ modules), `incamail`, `ch.ivyteam.ivy.addons` — resolved from the corporate registry. Projects deploy to Google Artifact Registry (`europe-west6-maven.pkg.dev/klara-repo/...`); dependency resolution repos are set to `repo.axongroupio.ch/artifactory` in settings.xml.

## Related
[[Pre-configure an EclipseIvy Designer workspace by seeding .prefs files]]

## Related

- [[Pre-configure an EclipseIvy Designer workspace by seeding .prefs files]]

%% ai-graph-start %%

**Related notes:**
- [[Building KlaraLuz Ivy projects off-VPN by routing Maven through Google Artifact Registry]]
- [[Add Ivy jars Maven plugin]]
- [[KlaraLuz Maven builds resolve dependencies from Google Artifact Registry and require gcloud auth]]
- [[Axon Ivy project anatomy logic split across processes, data classes, HTML dialogs, and Java]]
- [[Axon Ivy installEngine fails when ivy.engine.directory is stale or unwritable]]

**Relations:**
- KlaraLuz Axon Ivy projects — *use* — master branch
- KlaraLuz Axon Ivy projects — *target Ivy version* — Ivy 10.0.15
- KlaraLuz Axon Ivy projects — *do not target Ivy version* — Ivy 12
- master branch — *is in* — Axon Ivy repos
- Axon Ivy repos — *are* — axonivy-prod
- axonivy-prod — *hosted on* — Bitbucket
- klara_prototype/pom.xml — *configures Ivy version* — Ivy 10.0.15
- klara_prototype/pom.xml — *uses plugin* — ch.ivyteam.ivy:project-build-plugin
- klara_prototype/pom.xml — *has packaging type* — iar
- luz_components/pom.xml — *has packaging type* — iar
- luz_components/pom.xml — *has parent* — ch.klara.ivy:luz_ivy_common:2.0.01.0
- klara_theme/pom.xml — *has packaging type* — jar
- klara_theme/pom.xml — *builds with* — Maven
- CLI Maven builds — *are compatible with* — Ivy 10.0.15
- CLI Maven builds — *use plugin* — ch.ivyteam.ivy:project-build-plugin
- CLI Maven builds — *use engine* — Ivy engine 10.0.15
- Ivy engine 10.0.15 — *is cached at* — Ivy cache path
- Maven settings.xml — *contains* — ch Maven profile
- Ivy Designer 12.0.16 — *mismatches* — Ivy-10 projects
- Ivy Designer 12.0.16 — *triggers* — migration prompt
- Ivy Designer 10.0.16 — *is recommended for* — Ivy-10 projects
- Ivy Designer 10.0.16 — *is* — guide's version
- Dependency graph — *includes module* — luz_ivy_common
- Dependency graph — *includes module* — klara_faces
- Dependency graph — *includes module* — luz_templates
- Dependency graph — *includes module* — luz_common
- Dependency graph — *includes module* — xent_* modules
- Dependency graph — *includes module* — incamail
- Dependency graph — *includes module* — ch.ivyteam.ivy.addons
- luz_ivy_common — *resolved from* — corporate registry
- klara_faces — *resolved from* — corporate registry
- luz_templates — *resolved from* — corporate registry
- luz_common — *resolved from* — corporate registry
- xent_* modules — *resolved from* — corporate registry
- incamail — *resolved from* — corporate registry
- ch.ivyteam.ivy.addons — *resolved from* — corporate registry
- Projects — *deploy to* — Google Artifact Registry
- Google Artifact Registry — *has URL* — Google Artifact Registry URL
- dependency resolution repositories — *configured in* — Maven settings.xml
- dependency resolution repositories — *include* — repo.axongroupio.ch/artifactory
- EclipseIvy Designer workspace — *configured by* — .prefs files

%% ai-graph-end %%