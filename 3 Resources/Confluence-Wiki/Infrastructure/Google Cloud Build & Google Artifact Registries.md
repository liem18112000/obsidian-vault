---
ai_hash: df3eff9bce6abc1f
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 0
depth: 2.9
entities: []
relevance: 0.819
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/48341811344/Google+Cloud+Build+Google+Artifact+Registries
space: LUZ
status: reference
tags:
- confluence
- infra
- space/luz
title: Google Cloud Build & Google Artifact Registries
topic: infra
type: source
updated: 2025-02-20
---

# Google Cloud Build & Google Artifact Registries

> [!info] Imported from Confluence
> Space **LUZ** · updated 2025-02-20 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/48341811344/Google+Cloud+Build+Google+Artifact+Registries)
> Relevance 0.819 · topic `infra`

All code project are being migrated to the new Google CI/CD strategy.

- Jenkins → Google Cloud Build

- Artifactory → Google Artifact Registry (for Maven Packages)

- Container Registry → Google Artifact Registry (for Containers)

## Preparation Phase

<div hasbody="true" macro-id="7a7f2c3e-d16f-4ad3-86d8-bea95e0777bf" macro-name="info">

<span class="aui-icon aui-icon-small aui-iconfont-info confluence-information-macro-icon"> </span>

<div>

See how to prepare a java project [here](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/48342499329/Migrate+a+Java+Project+to+Google+Cloud+Build).

</div>

</div>

For each project

1.  Create a branch `invisible/devops` and update the project to use the new CI/CD infrastructure

2.  Ask Team Invisible to activate a Google Cloud Build pipeline for your new `invisible/devops` branch

3.  Confirm that your project is using artifacts from the new Google Artifact Registry

4.  Confirm that your project’s artifacts are being deploy in the new Google Artifact Registry

## Switch Phase

For each project

1.  Deactivate the `luz_kubernetes` update and apply step on Jenkins

2.  Activate the `luz_kubernetes` update and apply step in Google Cloud Build

3.  Merge the `invisible/devops` branch to `master` or `main`

4.  Ask Team Invisible to activate a Google Cloud Build pipeline for your “master” branch and deactivate the old `invisible/devops` branch pipeline

%% ai-graph-start %%

**Related notes:**
- [[Document flow setup build Jenkins job Maven]]
- [[CI CD (Google Cloud Build & Google Cloud Deploy)]]
- [[Discussion GCP Release Process with Google Cloud Build]]
- [[KlaraLuz Maven builds resolve dependencies from Google Artifact Registry and require gcloud auth]]
- [[Deployment Process]]

%% ai-graph-end %%