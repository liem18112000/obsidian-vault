---
ai_hash: e50f3db1fd934cf2
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 14
depth: 2.57
entities: []
relevance: 0.721
source: https://axonivy.atlassian.net/wiki/spaces/TP2020/pages/30906850008/Document+flow+setup+build+Jenkins+job+Maven
space: TP2020
status: reference
tags:
- confluence
- programming
- space/tp2020
title: Document flow setup build Jenkins job Maven
topic: programming
type: source
updated: 2021-03-01
---

# Document flow setup build Jenkins job Maven

> [!info] Imported from Confluence
> Space **TP2020** · updated 2021-03-01 · [open original](https://axonivy.atlassian.net/wiki/spaces/TP2020/pages/30906850008/Document+flow+setup+build+Jenkins+job+Maven)
> Relevance 0.721 · topic `programming`

### I. **Determine the type of project you want to build**

**

![[30906850008-image11.png]]

**

### **II. Setup build job**

- About the maven job you can refer to this job: <a href="https://build.axonivy.io/view/LUZ/job/KLARA/job/gcp-luz-online-web/configure" class="external-link" rel="nofollow">https://build.axonivy.io/view/LUZ/job/KLARA/job/gcp-luz-online-web/configure</a>
- Some key steps

          - Specifies to the repo that contains the source code


![[30906850008-image2021-3-1_14-53-21.png]]



  - For the maven build job you don't need to create a Jenkins file in the repo of the project. However, You must config "Post Steps" in Jenkins job to pull, create, push,... image to the server 


![[30906850008-image2021-3-1_9-41-24.png]]




![[30906850008-image2021-3-1_9-41-53.png]]




![[30906850008-image2021-3-1_16-2-19.png]]



### III. Config deploy image and packaging 

####       **1. Config deploy the image to the server**

   - At present, we have two servers with a namespace is dev and dev-vn in GCP. If you want to deploy an image of your project to that server, you must configure some other related repo on Bitbucket

- **luz_kubernetes repo**

   - In the **luz_kubernetes** repo, you can config in **kubernetes** folder, **kubernetes-overlays** folder, and **pom.xml** file


![[30906850008-image5.png]]




![[30906850008-image12.png]]



  

  - The second, About **kubernetes** folder that is the place to declare kind kubernetes, name project, container name, specifications for the container,... in GCP

     **note**: If your project is not released yet, you don't need config in **kubernetes** folder

     + Create a folder that has a name like your project in **kubernetes** folder, that folder content **k8s.yaml** file to config

 

![[30906850008-image7.png]]

![[30906850008-image2021-3-1_11-35-29.png]]



      + Add path **k8s.yaml** file into **kustomization.yaml** file


![[30906850008-image9.png]]



  

 - The finally, About **kubernetes-overlays** folder is the place to contain overwrite file config for each environment dev, dev-vn, dev-staging, production,...

    For each environment, like config with **kubernetes** folder you can add **k8s.yaml** file and declare path in **kustomization.yaml** file


![[30906850008-image2021-3-1_11-40-4.png]]



   

- **luz_deploy_dev repo**

           If your project is not deploying with the** luz_webclient** repo, you must declare your project in the **deployProjects.txt** file in the **luz_deploy_dev** repo

**

![[30906850008-image2021-3-1_11-53-42.png]]

**

**2. Config packaging **

    We have Jenkins job **"<a href="https://build.axonivy.io/view/LUZ/job/KLARA/view/gcp-release-package/job/gcp-one-click-to-package/" class="external-link" rel="nofollow">gcp-one-click-to-package</a>"** to build packaging, if you want to build a package for your project you must declare the name of your project in the **project_list_to_release.txt** file

**

![[30906850008-image2021-3-1_11-58-21.png]]

![[30906850008-image2021-3-1_11-58-35.png]]

**

%% ai-graph-start %%

**Related notes:**
- [[Jenkins (How to build & deploy)]]
- [[Deployment Process]]
- [[Kubernetes knowledge]]
- [[Google Cloud Build & Google Artifact Registries]]
- [[Recipe Deploy with Terraform]]

%% ai-graph-end %%