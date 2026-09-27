---
ai_hash: 6ac6c23ad4701636
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 7
depth: 3
entities: []
relevance: 0.904
source: https://axonivy.atlassian.net/wiki/spaces/IO/pages/48296099906/CI+CD+Google+Cloud+Build+Google+Cloud+Deploy
space: IO
status: reference
tags:
- confluence
- infra
- space/io
title: CI/CD (Google Cloud Build & Google Cloud Deploy)
topic: infra
type: source
updated: 2025-01-29
---

# CI/CD (Google Cloud Build & Google Cloud Deploy)

> [!info] Imported from Confluence
> Space **IO** · updated 2025-01-29 · [open original](https://axonivy.atlassian.net/wiki/spaces/IO/pages/48296099906/CI+CD+Google+Cloud+Build+Google+Cloud+Deploy)
> Relevance 0.904 · topic `infra`

<div class="contentLayout2">

<div class="columnLayout fixed-width">

<div class="cell normal" data-type="normal">

<div class="innerCell">

### Was wollen wir damit erreichen, was ist es?

Es ist den kleinsten Anfang, um den Reifengrad der Entwicklung weiter zu bringen. CI/CD heisst die Automatisierung der Qualitätscheck, Release und Deployment somit checks und deployment laufend, also Continuously, gemacht werden können.

1.  **Nicht gefangen sein wegen deprecation, Google Container Registry (deprecated since 2023 !)**

2.  Schneller, mehrere builds und releases parallel herstellen können.

3.  Schnellere Releases.

4.  Schnellere Deployments.

### Was wurde gemacht

- Start dieses gegen enden November 2024

- Auflistung der services und libraries welche umgezogen werden müssen.

- Auflistung der verschiedene System wo Software und Lieferbaren (Artifacts) abgelegt werden, von wem, wann, wie etc.

- Befestigung des PoC von Future in einen realen Plan für die gesamte Platform mit alle ihre besondere kleine Projekte.

- Umsetzung Leuchtturmprojekte und dessen Abhängigkeiten mit “kochrezept”

- Setup von SonarQube Zugriff (VPN)

  - Aufgabe wurde Anfang Dezember definiert, <span class="confluence-jim-macro jira-issue conf-macro output-block" client-id="SINGLE_d3f195c5-8684-3b17-b4f6-e9ee3a0b0fe2_48296099906_712020:87b0f7f1-aaab-4406-a25d-fa0fc075c4d4" hasbody="false" jira-key="LUZ-128376" macro-id="6dc89cdb-3ff4-4d38-893e-56060ffefc15" macro-name="jira"> <a href="https://axonivy.atlassian.net/browse/LUZ-128376" class="jira-issue-key"><span class="aui-icon aui-icon-wait issue-placeholder"> </span>LUZ-128376</a> - <span class="summary">Getting issue details...</span> <span class="aui-lozenge aui-lozenge-subtle aui-lozenge-default issue-placeholder">STATUS</span> </span>

### Schwerpunkten (alle überwindbar)

- Gefährlich, wenn faule Artifakten es bis Prod schaffen (SBoM issue)

  - Wir müssen sicherlich nicht alles auf ein mal ändern! Es hat Einfluss auf die Entwicklern und den Betrieb.

- Langwierig um Zugriffe zu bekommen, Zeitverlust

- Es gabt am Anfang, also enden November, keinen Plan von was sollte wohin

- Wichtig ist es, dass die Entwicklern schnell wechseln können, ohne Zeitverlust, an den richtigen Zeit

- Faule Terraform-Projekte müssten aufgeräumt werden

- Einarbeitung in die jeweiligen Code-Projekte hat Zeit beansprucht

  - Kein Standard Git-Flow oder semver Versionsverwaltung

  - Sehr alte Java Versionen werden noch benutzt

- Fehlende “maintainer” für jeweilige Projekte

### Was kommt in die nächste Wochen

#### Stand In November

</div>

</div>

</div>

<div class="columnLayout two-equal">

<div class="cell normal" data-type="normal">

<div class="innerCell">


![[48296099906-Bildschirmfoto 2025-01-28 um 16.51.07.png]]



</div>

</div>

<div class="cell normal" data-type="normal">

<div class="innerCell">


![[48296099906-Bildschirmfoto 2025-01-28 um 17.10.51.png]]



</div>

</div>

</div>

<div class="columnLayout fixed-width">

<div class="cell normal" data-type="normal">

<div class="innerCell">

### In Umsetzung Heute

</div>

</div>

</div>

<div class="columnLayout two-equal">

<div class="cell normal" data-type="normal">

<div class="innerCell">

- 14 aus ~150 Artifakten werden mit Google Cloud Build gebuildet und mit Google Artifact Registry aufbewahrt.

- Bis 19.02.2025 müssen es alle sein.

  - Machbar, mit weniger als ein stunde pro Artifakt

  - Schnelle Projekte sind in 15 Minuten erledigt

  - Den rest ist für die “besondere Projekte” welche bei der Umsetzung entdeckt werden

### Zeitplan

- Alle Artifakten umgesetzt bis ende Februar

</div>

</div>

<div class="cell normal" data-type="normal">

<div class="innerCell">

- Leuchtturmprojekte

  - luz_dockerfiles, ok

  - luz_next projekte

    - build & publish ok,

    - **sonarqube setup, @Jiyan**

  - luz_cache (quarkus project)

    - build & publish ok

  - luz_compensation (work in progress)

  - java projects / artifactory / google artifact registry

    - mostly ok

    - blocker on jwt_service needing luzsec_service roles

</div>

</div>

</div>

<div class="columnLayout two-equal">

<div class="cell normal" data-type="normal">

<div class="innerCell">


![[48296099906-Bildschirmfoto 2025-01-28 um 16.51.44.png]]



</div>

</div>

<div class="cell normal" data-type="normal">

<div class="innerCell">


![[48296099906-Bildschirmfoto 2025-01-28 um 16.52.44.png]]



</div>

</div>

</div>

<div class="columnLayout fixed-width">

<div class="cell normal" data-type="normal">

<div class="innerCell">

#### Umzusetzen Ende Februar


![[48296099906-Bildschirmfoto 2025-01-28 um 16.58.41.png]]



## Detailsübersicht


![[48296099906-Bildschirmfoto 2025-01-28 um 16.13.15.png]]



</div>

</div>

</div>

</div>

%% ai-graph-start %%

**Related notes:**
- [[Google Cloud Build & Google Artifact Registries]]
- [[Discussion GCP Release Process with Google Cloud Build]]
- [[GKE - Cloud Run Migration Trackers]]
- [[CICD for Kogito]]
- [[Infrastructure]]

%% ai-graph-end %%