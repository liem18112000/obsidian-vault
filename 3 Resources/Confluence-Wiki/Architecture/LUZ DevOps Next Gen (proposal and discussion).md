---
ai_hash: cfca5413f5cdceb0
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 0
depth: 2.79
entities: []
relevance: 0.769
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20436750268/LUZ+DevOps+Next+Gen+proposal+and+discussion
space: LUZ
status: reference
tags:
- confluence
- architecture
- space/luz
title: LUZ DevOps Next Gen (proposal and discussion)
topic: architecture
type: source
updated: 2016-12-12
---

# LUZ DevOps Next Gen (proposal and discussion)

> [!info] Imported from Confluence
> Space **LUZ** · updated 2016-12-12 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20436750268/LUZ+DevOps+Next+Gen+proposal+and+discussion)
> Relevance 0.769 · topic `architecture`

<span class="status-macro aui-lozenge aui-lozenge-visual-refresh aui-lozenge-error conf-macro output-inline" hasbody="false" macro-id="2d80772b-f608-4d6b-8932-001876410553" macro-name="status">TO BE DISCUSSED</span>

# Current situation (as of December 7th, 2016)

Currently, the whole LUZ application is delivered as a single distribution package `luz_dist-0.00.xx.00.zip`. The contents of that zip file is as follows (example):

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="159ab6ca-eff7-4b70-bdf0-d0faf4ad1502" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
luz_dist-0.00.86.00.zip
- application server
  - luz_ear-0.00.86.00.ear
    - META-INF
      - application.xml
      - ...
    - jwt_service-0.0.5.war
    - klee-event-broker-0.00.17.00.war
    - luz_admin_service-0.00.19.00.war
    - luz_compensation-0.0.28.0.war
    - luz_person-0.0.20.0.war
    - luzfin_finance-0.0.13.0.war
    - luztenant_service-0.0.6.war
    - xent-rest-0.00.89.00.war
- databases_migration
  - FileManager
    - V1__baseline.sql
    - V2__migrate_path_document.sql
  - authorization
    - ...
  - klee
    - ...
  - luz_admin
    - ...
  - xent_additionals
    - ...
  - xline
    - ...
  - changelog.txt
- ivy projects.zip
  - ch.ivyteam.ivy.addons-6030100.0.0-20160915.121606-1.iar
  - klee_process-0.00.56.00.iar
  - luz_admin-0.00.59.00.iar
  - luz_components-0.00.85.00.iar
  - luz_finance-0.00.31.00.iar
  - luz_web-0.00.85.00.iar
  - luz_xhrm_processes-0.00.85.00.iar
  - xpertline_web_base-1.04.39.00.iar
```

</div>

</div>

This distribution package is used for the deployments, especially to the pre-production (test) and production environments. Beside this distribution package, there are a variety of other resources that need to be deployed/installed on a regular basis, e.g. `standard_configuration.groovy` which is a Groovy script used to (re-)initialise the salary item types, salary configurations and global variables for the payroll module. Sometimes there are even SQL scripts which need to be executed directly on the database, although this is decreasing since we automatically migrate databases within the modules. An important point is, that the different distribution files may have different release cycles, e.g. the `standard_configuration.groovy` is not bound to any product release cycle, since it is a kind of configuration and not *installation*. However, currently we deploy always all files in one single pass.

The current procedure to make a deployment to one of the environment mentioned above is like so:

1.  Enable maintenance mode, so that the users are informed what's currently happening (manual)
2.  Create a snapshot of the current virtual server (manual)
3.  Deploy the distribution package with the deployment scripts, see <a href="https://bitbucket.org/axonivy-prod/luz_devops/src/HEAD/klara-environment/deployment-scripts/?at=master" class="external-link" rel="nofollow">https://bitbucket.org/axonivy-prod/luz_devops/src/HEAD/klara-environment/deployment-scripts</a> for details. (manual, scripted)
4.  Stop the Axon.ivy and WildFly services (manual)
5.  Execute SQL scripts, if any (manual)
6.  Start the Axon.ivy and WildFly services (manual)
7.  Deploy any additional configuration, e.g. `standard_configuration.groovy` (manual, scripted)
8.  Quick test to verify that basic stuff is still working - kinda smoke test (manual)
    1.  Test result is not ok: Restore previously created snapshot (manual)
9.  Disable maintenance mode. Application is online again (manual)

Usually, such kind of a deployment takes about 15 - 20 minutes in time.

## How is the distribution package created?

Currently there is a quite huge number of projects included and referenced in one distribution package and for testing the projects in that package. To make this visible, a distribution package was unpacked recursively and all the contained, non-3rd-party projects and libraries were listed here:

<div>

|  |  |  |
|----|----|----|
| Artifact Name | Artifact Type | Remarks |
| luz_ear-0.00.86.00.ear | ear | Used as a container for all the JEE projects. |
| klee_process-0.00.56.00.iar | iar | Ivy project artefact |
| luz_admin-0.00.59.00.iar | iar | Ivy project artefact |
| luz_components-0.00.85.00.iar | iar | Ivy project artefact |
| luz_finance-0.00.31.00.iar | iar | Ivy project artefact |
| luz_web-0.00.85.00.iar | iar | Ivy project artefact |
| luz_xhrm_processes-0.00.85.00.iar | iar | Ivy project artefact |
| xpertline_web_base-1.04.39.00.iar | iar | Ivy project artefact |
| luz_admin_client-0.00.14.00.jar | jar |   |
| luz_admin_client.jar | jar | Dependencies in IAR projects were packaged without version number |
| luz_common-0.0.12.0.jar | jar | Different versions of the same artefact are referenced from different higher-level artefacts |
| luz_common-0.0.16.0.jar | jar |   |
| luz-jax-rs-patch-0.00.02.00.jar | jar |   |
| luzcomp-data-0.00.89.00.jar | jar |   |
| luzsec_service-0.0.1.jar | jar |   |
| luzsec_service-0.0.6.jar | jar |   |
| multi_tenancy_api-0.0.1.jar | jar |   |
| multi_tenancy_api-0.0.4.jar | jar |   |
| xent_business_bus_model-0.00.02.00.jar | jar |   |
| xent_business_bus_model-0.00.04.00.jar | jar |   |
| xent_business_bus_model.jar | jar |   |
| xent_business_bus-0.00.08.00.jar | jar |   |
| xent_client_info-0.00.13.00.jar | jar |   |
| xent_common-0.00.06.00.jar | jar |   |
| xent_common.jar | jar |   |
| xent_data_analyzer-0.00.23.00.jar | jar |   |
| xent_ivy_data-0.00.89.00.jar | jar |   |
| xent_localch_data-0.00.89.00.jar | jar |   |
| xent_oauth.jar | jar |   |
| xent_process_manager.jar | jar |   |
| xent_service_model-0.00.62.00.jar | jar |   |
| xent_service_model-0.00.89.00.jar | jar |   |
| xent_service_model.jar | jar |   |
| xent_task_manager.jar | jar |   |
| xent_user_security.jar | jar |   |
| xent-additional-data-0.00.89.00.jar | jar |   |
| xent-service-0.00.89.00.jar | jar |   |
| xent-uid-service-0.00.06.00.jar | jar |   |
| xpertline_meta_api-1.00.00.00.jar | jar |   |
| xpertline_meta_api.jar | jar |   |
| jwt_service-0.0.5.war | war | Independent war as service layer implementation ("micro service") |
| klee-event-broker-0.00.17.00.war | war | Independent war as service layer implementation ("micro service") |
| luz_admin_service-0.00.19.00.war | war | Independent war as service layer implementation ("micro service") |
| luz_compensation-0.0.28.0.war | war | Independent war as service layer implementation ("micro service") |
| luz_person-0.0.20.0.war | war | Independent war as service layer implementation ("micro service") |
| luzfin_finance-0.0.13.0.war | war | Independent war as service layer implementation ("micro service") |
| luztenant_service-0.0.6.war | war | Independent war as service layer implementation ("micro service") |
| xent-rest-0.00.89.00.war | war | Independent war as service layer implementation ("micro service") |

</div>

Even if we eliminate the doublets, there are still around 40 different artefacts which needs to be managed for every single release, since they are all together delivered in one distribution package. Since in the current process, the trunk version always represents a SNAPSHOT version and may also reference SNAPSHOT versions, that could lead to a situation, that almost 40 individual projects need to be released. For every single projects, that includes the following steps: Replace all SNAPSHOT dependencies with dependencies to released versions. Change the project version itself from a SNAPSHOT version to a release version. Tag the version and change the project version back to a SNAPSHOT version.

Even though this is not absolutely true since some of the projects are released together, it's still a time consuming process.

# Issues with the current approach

- The release process is quite time consuming and involves many manual steps (assumption)
- Even for the smallest update in a small library, the whole package has to be released
- Most of the deployment steps are manually executed. That's error prone and prevents a unattended deployment during night-time
- Because of the above points, we usually wait longer until we deploy a new package, which leads to a higher risk to break the product

# Sketch the future

How can we attack and solve the issues listed in the previous chapter?

- <u>Automate everything:</u> The whole deployment process to the different environments should be automated. Jenkins Pipelines could be a candidate to implement the whole pipeline (see <a href="https://jenkins.io/solutions/pipeline/" class="external-link" rel="nofollow">https://jenkins.io/solutions/pipeline/</a>)
- <u>Unscramble the deployment:</u> Split the one distribution package deployment into several small deployments, one for each top-level project: 
  - jwt_service.war
  - klee-event-broker.war
  - luz_admin_service.war
  - luz_compensation.war
  - luz_person.war
  - luzfin_finance.war
  - luztenant_service.war
  - xent-rest.war
  - klee_process.iar
  - luz_admin.iar
  - luz_components.iar
  - luz_finance.iar
  - luz_web.iar
  - luz_xhrm_processes.iar
  - xpertline_web_base.iar  
- <u>Container project to track versions:</u> To ease the deployment, we could still have a "container" project which references all the top-level projects to be deployed. That one could be used to trigger a new deployment to TEST and PROD.
- <u>`luz_ear.ear` is not needed anymore</u>
- <u>Every build is a release:</u> Every build triggered on the build server results in a released product/module, which is uniquely identifiable (e.g. by a build number included in the version string). That implies, that we never have dependencies to SNAPSHOT versions again (at least not in den committed `pom.xml`), see <a href="https://axelfontaine.com/blog/final-nail.html" class="external-link" rel="nofollow">https://axelfontaine.com/blog/final-nail.html</a> for a discussion about such an approach.

%% ai-graph-start %%

**Related notes:**
- [[Architecture Overview LUZ]]
- [[CI CD (Google Cloud Build & Google Cloud Deploy)]]
- [[Document flow setup build Jenkins job Maven]]
- [[API models libraries for reducing duplicated code and increasing the maintainability of our JEE]]
- [[Deployment Process]]

%% ai-graph-end %%