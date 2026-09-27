---
ai_hash: a1dae1529a4fad94
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 6
depth: 2.48
entities: []
relevance: 0.724
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/48390045721/Discussion+GCP+Release+Process+with+Google+Cloud+Build
space: LUZ
status: reference
tags:
- confluence
- infra
- space/luz
title: '[Discussion] GCP Release Process with Google Cloud Build'
topic: infra
type: source
updated: 2025-06-06
---

# [Discussion] GCP Release Process with Google Cloud Build

> [!info] Imported from Confluence
> Space **LUZ** · updated 2025-06-06 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/48390045721/Discussion+GCP+Release+Process+with+Google+Cloud+Build)
> Relevance 0.724 · topic `infra`

<div class="toc-macro client-side-toc-macro conf-macro output-block" cssliststyle="none" hasbody="false" headerelements="H1,H2,H3,H4,H5,H6,H7" macro-id="1b01dd73-9fc8-4902-a903-0f4a456910b6" macro-name="toc">

</div>

# A. Release - hotfix

**1. Case 1 hotfix for a service module**

Example: we have to release a hotfix for luz-compensation 0.1.0.0  
--\> luz-compensation 0.1.0.1-SNAPSHOT instead of release version **0.1.0.1** as before  
Reason for this: the new maven artifactory server **doesn’t allow to override the release version**

When the hotfix is OK:  
+ Remove -SNAPSHOT (Manually)  
+ Create TAG

**2. Case 2 hotfix for an ivy module**

\+ Change version to hotfix snapshot

\+ Everything else is the same  
When the hotfix is OK:  
+ Remove -SNAPSHOT  
+ Create TAG

# B. Normal sprint release package


![[48390045721-release-jwt-service.png]]




![[48390045721-release-jwt-service-and-dependencies.png]]




![[48390045721-release-ivy-modules.png]]



1.  **Manually prepare:**  
    Try to merge master to release branch of luz_kuberentes, whether any conflicts -\> if yes, resolve before trigger

2.  **Create trigger release package**

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="cd59a7db-f53c-4704-b11b-76b57e7bff8a" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
-> prepare luz_kubernetes branch
        + checkout master
        + merge all to release 
        + copy back all hash from dev-staging to release -> for all the modules that don't need to be released this sprint
    -> common libs (wait for prepare)
            -> lib1
        -> service -> service-modules (wait for lib)
                   -> module-a
        -> webclient -> ivy-modules (wait for lib)
                     -> ivy1
```

</div>

</div>

3.  **Create trigger release services**  
    The trigger will call each of trigger release service modules.

4.  **Create trigger release each service:** parameter as release tag to build docker only in case of failure

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="30a4167b-ef5a-486b-a189-3474ee8d3814" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
if param is empty
        - create branch release for each module
        - build maven
            + cut snapshot 
            + build mvn:release: tag, push tag, .,,,
            + deploy release version ---
            + increase to new version snapshot
            + build build new verson snapshot
            + ...deploy...

        - build docker if module is containerize module
            + build file war or pull from repo 
            + docker build push
            + update image hash to luz_kubernetes

        - update version to pom of luz_kubernetes? ...
        - update release flag on luz_versioning_release
        - merge branch release to master for each module
```

</div>

</div>

4.  **Create trigger release luz_webclient**  
    This will call each of trigger release ivy modules

5.  **Create trigger release each ivy module:**

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="5f22794a-9113-4971-a483-e633707bf613" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
- create branch release for each module
    - build maven
        + cut snapshot 
        + build mvn:release: tag, push tag, .,,,
        + deploy release version ---
        + increase to new version snapshot
        + build build new verson snapshot
        + ...deploy...
        
    - update version to pom.xml
    - update release hash to hash.txt
    - merge branch release to master for each module
```

</div>

</div>

# C. Deploy - all environments

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="ab2110cc-c99a-4279-a7de-168edeb97bc2" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
- Trigger for all
- Trigger for individual
- notify slack? - ask Kepler
```

</div>

</div>

# D. Integrate scanning Sonar - code quality

- Create steps for scanning the code in each module

  - Create steps for scanning the code in each module

- Check the result from the running Sonar server:

  - Where it should be create to run - klara-infra?

  - How? like dev-portal service?

### How To Add Sonar Qube to my cloud build pipeline in klara-infra?

Add the following configuration to your cloud-build.yaml file. Please make sure, that the blocks “availableSecrets” and “options” are copied as well. The step “sonarqube-scan” shoud be the first step in your cloud-build.yaml.  
**IMPORTANT**: In line 7: Replace “YOUR_PROJECT_KEY” with your project key from sonar qube (for example “luz_next”)

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="8c41c515-0ef9-4beb-aeff-e124cf810a9b" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
steps:
  # Perform a sonar qube scan of the code.
  - id: "sonarqube-scan"
    name: "europe-west6-docker.pkg.dev/klara-infra/google-cloud-build/sonar-scanner"
    args:
      - "-Dsonar.host.url=http://10.128.0.47/"
      - "-Dsonar.projectKey=YOUR_PROJECT_KEY"
      - "-Dsonar.sources=."
    secretEnv: ["SONAR_TOKEN"]

availableSecrets:
  secretManager:
    - versionName: "projects/klara-infra/secrets/sonarqube-access-token/versions/latest"
      env: SONAR_TOKEN

options:
  pool:
    name: projects/$PROJECT_ID/locations/$LOCATION/workerPools/klara-infra-cloud-build-private-pool
```

</div>

</div>

#### Explanation of Elements in cloud-build.yaml:

**<u>steps</u>**: In this array, there need to be one “step” element with the id ”sonarqube-scan” and our own docker image “sonar-scanner”. If you need, you can pass more CLI Arguments to sonar Qube with “-D” prefixed lines in “args” array.  
Short overview of the needed arguments:

<div>

|  |  |
|----|----|
| **Key Name** | **Explanation** |
| sonar.host.url | Sonar Qube Server URL. **For any pipelien in klara-infra, this must always be** http://10.128.0.47/ |
| sonar.projectKey | You have to use an existing project in Sonar Qube. On the Sonar Website <a href="https://sonar.axongroupio.ch/" class="external-link" rel="nofollow">https://sonar.axongroupio.ch/</a> you open your project dashboard and klick on “Project Information” on the top right corner. Then a box on the right will apprea with a field “Project Key” and the text box underneath. Copy the Value from the text box, this is your full project key |
| sonar.sources | The path to the source code, this is mostly the root folder of the repository: “ . “ |

</div>

**<u>availableSecrets</u>:** Here we need to set one important element in the array “**<u>secretManager</u>**”. This will provide the container an env variable called “SONAR_TOKEN” which contains the value of google secret `“sonarqube-access-token"`  
Remarks: You can have multiple values in “secretManager” and also have more values in “availableSecrets”. Just make sure, the block form the code is provided.

**<u>options</u>:** Here it is very important to set the “**<u>pool.name</u>**” value. Feel free to set other variables in “options” (for example “logging”), it will not harm the process. If you change something in “pool”, please update Team Invisble.

#### Why we did it like this:

<span class="inline-comment-marker" ref="882eb8b7-9e60-4c97-8236-89f056b75b3f">Sonar Qube is still runnung on Axon site : </span><a href="https://sonar.axongroupio.ch/" class="external-link" rel="nofollow"><span class="inline-comment-marker" data-ref="882eb8b7-9e60-4c97-8236-89f056b75b3f">https://sonar.axongroupio.ch/</span></a><span class="inline-comment-marker" ref="882eb8b7-9e60-4c97-8236-89f056b75b3f"> </span>  
Google Cloud Build cannot connect to Axon directly, so we provide an reverse proxy inside klara-infra: <a href="http://10.128.0.47/" class="external-link" rel="nofollow">http://10.128.0.47/</a>  
The building steps on Google Cloud Build can only connect to local servers when we use a “private pool” for the build containers.

# E. Tests' Reports

1.  Each build in Jenkins visualizes all tests result then everybody can easily to check what is going wrong and adapt quickly.  
    Do we want the same as Jenkins in cloudbuild?

2.  Announce to team (or member) via any channel each time of test failed.

%% ai-graph-start %%

**Related notes:**
- [[CI CD (Google Cloud Build & Google Cloud Deploy)]]
- [[Document flow setup build Jenkins job Maven]]
- [[Kubernetes knowledge]]
- [[Recipe Deploy with Terraform]]
- [[Deployment Process]]

%% ai-graph-end %%