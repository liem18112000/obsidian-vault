---
ai_hash: e3d4681623c6eada
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 28
depth: 3
entities: []
relevance: 0.794
source: https://axonivy.atlassian.net/wiki/spaces/Arrow/pages/48468918404/Deploy+routing+containerization+as+a+docker+from+local+into+server
space: Arrow
status: reference
tags:
- confluence
- infra
- space/arrow
title: Deploy routing containerization as a docker from local into server
topic: infra
type: source
updated: 2025-05-29
---

# Deploy routing containerization as a docker from local into server

> [!info] Imported from Confluence
> Space **Arrow** · updated 2025-05-29 · [open original](https://axonivy.atlassian.net/wiki/spaces/Arrow/pages/48468918404/Deploy+routing+containerization+as+a+docker+from+local+into+server)
> Relevance 0.794 · topic `infra`

I. Deploy routing-backend

1.  Build the image locally

- Clone and open the repository for routing-backend-docker at: <a href="https://scm.axonfintech.io/cob/cob-routing-backend-docker" class="external-link" rel="nofollow">https://scm.axonfintech.io/cob/cob-routing-backend-docker</a>

- Customize your built branch at build.gradle:  
  Define your branch at  

  <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="7560685e-9f62-4890-ba38-f401b6100221" macro-name="code" style="border-width: 1px;">

  <div class="codeContent panelContent pdl">

  ``` syntaxhighlighter-pre
  def branch = "master"
  git = org.ajoberstar.grgit.Grgit.clone(dir: cloneDir, uri: cloneUrl, credentials: credentials, refToCheckout: branch)
  ```

  </div>

  </div>


![[48468918404-image-20250425-091028.png]]



2.  Save the image as a tar file

Run `cb `to build the source code from your branch as an image  


![[48468918404-image-20250425-091615.png]]



After being built successfully, we get a new image `cob-routing-backend-docker:0.20.20-SNAPSHOT`. Then, we will save it as a tar file by :

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="b5ef0a7c-ab48-46c7-9eb2-eb66644fbbd6" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
docker save -o routing-backend.tar cob-routing-backend-docker:0.20.20-SNAPSHOT
```

</div>

</div>

In the source code directory, we will have a `routing-backend.tar` file.


![[48468918404-image-20250425-092123.png]]



3.  Copy the tar file to the server

- Copy this tar file into the routing-backend directory on the integration server at `/opt/integration_services/routing-containerization/routing-backend`  

  

![[48468918404-image-20250425-093043.png]]



4.  Load the tar file as an image  
    We have to load the tar file as an image into the server by:

    <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="5729f3b6-1f55-4221-a728-25b20b059ed4" macro-name="code" style="border-width: 1px;">

    <div class="codeContent panelContent pdl">

    ``` syntaxhighlighter-pre
    docker load -i routing-backend.tar
    ```

    </div>

    </div>

Then we will have this image on the server


![[48468918404-image-20250425-093622.png]]



We can double-check again by

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="a22a6b7c-5d88-4b4c-9bb6-f9c90f437aba" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
docker images | grep cob-routing-backend-docker
```

</div>

</div>


![[48468918404-image-20250425-093823.png]]



5.  Update the .env file with the new image version  
    Go to the `.env` file and update with the correct version of the new image at `IMAGE_VERSION `

    

![[48468918404-image-20250425-094007.png]]



6.  Start the routing backend Docker  
    Run the docker by:

    <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="a69a464f-0062-4872-ba6f-c28070dc889b" macro-name="code" style="border-width: 1px;">

    <div class="codeContent panelContent pdl">

    ``` syntaxhighlighter-pre
    sh start_docker.sh
    ```

    </div>

    </div>


![[48468918404-image-20250425-094423.png]]



Check the result:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="bf3db8c2-e66c-491b-baf7-082c10e0d141" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
curl --location 'http://10.0.4.11:3030/api/ping'
```

</div>

</div>


![[48468918404-image-20250425-094626.png]]



II\. Deploy routing-frontend

1.  Build the image locally  
    Clone and open the repository for routing-frontend-docker at: <a href="https://scm.axonfintech.io/cob/cob-routing-frontend-docker" class="external-link" rel="nofollow">https://scm.axonfintech.io/cob/cob-routing-frontend-docker</a>  
      
    Customize your built branch at build.gradle:  
    Define your branch at

    <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="b2f3b957-8d0f-4314-86a2-6861dd84845f" macro-name="code" style="border-width: 1px;">

    <div class="codeContent panelContent pdl">

    ``` syntaxhighlighter-pre
    def branch = "master"
    ```

    </div>

    </div>


![[48468918404-image-20250425-091028.png]]



2.  Save the image as a tar file  
    Run `cb `to build the source code from your branch as an image  

    

![[48468918404-image-20250425-101132.png]]



After being built successfully, we get a new image `cob-routing-frontend-docker:0.20.20-SNAPSHOT`. Then, we will save it as a tar file by :

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="d7c4febe-d84a-46a7-8782-cc4191bc4b37" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
docker save -o routing-frontend.tar cob-routing-frontend-docker:0.20.20-SNAPSHOT
```

</div>

</div>

In the source code directory, we will have a `routing-frontend.tar` file.  


![[48468918404-image-20250425-101345.png]]



3.  Copy the tar file to the server  
    Copy this tar file into the routing-frontend directory on the integration server at `/opt/integration_services/routing-containerization/routing-frontend`  

    

![[48468918404-image-20250425-102521.png]]



4.  Load the tar file as an image  
    We have to load the tar file as an image into the server by:

    <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="f403eb6f-c83e-40cd-87aa-fed180647aeb" macro-name="code" style="border-width: 1px;">

    <div class="codeContent panelContent pdl">

    ``` syntaxhighlighter-pre
    docker load -i routing-frontend.tar
    ```

    </div>

    </div>

Then we will have this image on the server


![[48468918404-image-20250425-102640.png]]



We can double-check again by

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="e2ffbb70-56e7-4161-9297-f00bf6d5b47b" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
docker images | grep cob-routing-frontend-docker
```

</div>

</div>


![[48468918404-image-20250425-103319.png]]



5.  Update the .env file with the new image version  
    Go to the `.env` file and update it with the correct version of the new image at `IMAGE_VERSION `  

    

![[48468918404-image-20250425-103121.png]]



6.  Start the Docker  
    Run the docker by:

    <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="fe16dc0f-a082-448a-80f9-4ff3e3012dd2" macro-name="code" style="border-width: 1px;">

    <div class="codeContent panelContent pdl">

    ``` syntaxhighlighter-pre
    sh start_docker.sh
    ```

    </div>

    </div>

    

![[48468918404-image-20250425-103224.png]]



    Verify the result in the `routing-frontend-logs.log` file at `/opt/integration_services/routing-containerization/routing-frontend/logs`:  

    

![[48468918404-image-20250425-103455.png]]



III\. Deploy by script file for Routing Containerization on the Integration Server

1.  Deploy the Routing backend  
    1.1 Build the image locally

    - Clone and open the repository for routing-backend-docker at: <a href="https://scm.axonfintech.io/cob/cob-routing-backend-docker" class="external-link" rel="nofollow">https://scm.axonfintech.io/cob/cob-routing-backend-docker</a>

    - Customize your built branch at build.gradle:  
      Define your branch at

      <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="91494a23-e892-459a-9bab-4403726a5704" macro-name="code" style="border-width: 1px;">

      <div class="codeContent panelContent pdl">

      ``` syntaxhighlighter-pre
      def branch = "master"
      ```

      </div>

      </div>

    

![[48468918404-image-20250425-091028.png]]



    Run `cb `to build the source code from your branch as an image


![[48468918404-image-20250521-041507.png]]



1.2 Download the deployment script for the backend:  

<span class="confluence-embedded-file-wrapper conf-macro output-inline" hasbody="false" macro-id="e3c63c2e-3f71-4018-bd4b-59925576789b" macro-name="view-file"><a href="../_attachments/48468918404-deploy_backend.sh" class="confluence-embedded-file" data-nice-type="Text File" data-file-src="/wiki/download/attachments/48468918404/deploy_backend.sh?version=1&amp;modificationDate=1747801074711&amp;cacheVersion=1&amp;api=v2" data-mime-type="text/plain" data-has-thumbnail="true">

![[48468918404-deploy_backend.sh]]

</a></span>

1.3 Deploy with the deployment script

In the folder of the **deploy_backend.sh**, run the command with the bash terminal:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="22d2c38d-1a97-49b3-b22b-efdd63347d4a" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
sh deploy_backend.sh [IMAGE_VERSION]
```

</div>

</div>

- IMAGE_VERSION is the version of the built image from step 1.1, for example:

  <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="0ca570dc-61f0-40c4-af31-80b11db00cde" macro-name="code" style="border-width: 1px;">

  <div class="codeContent panelContent pdl">

  ``` syntaxhighlighter-pre
  sh deploy_backend.sh 0.21.11-SNAPSHOT
  ```

  </div>

  </div>

Result:


![[48468918404-image-20250521-042154.png]]



**NOTE: If there is any configuration (except the IMAGE_VERSION) for the backend environment, please update the .env file on the INT server first, then run this script to correct all configurations.**

Check the result:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="8998bd9b-4172-48d8-915e-179e46754dab" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
curl --location 'http://10.0.4.11:3030/api/ping'
```

</div>

</div>


![[48468918404-image-20250425-094626.png]]



2.  Deploy the Routing frontend

2.1. Build the image locally  
Clone and open the repository for routing-frontend-docker at: <a href="https://scm.axonfintech.io/cob/cob-routing-frontend-docker" class="external-link" rel="nofollow">https://scm.axonfintech.io/cob/cob-routing-frontend-docker</a>

Customize your built branch at build.gradle:  
Define your branch at

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="51003422-0bb1-4dcf-891e-f878e02209ac" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
def branch = "master"
```

</div>

</div>


![[48468918404-image-20250425-091028.png]]



Run `cb `to build the source code from your branch as an image


![[48468918404-image-20250521-044224.png]]



2.2 Download the deployment script for the frontend:

<span class="confluence-embedded-file-wrapper conf-macro output-inline" hasbody="false" macro-id="9527b9e9-cd37-4c8d-9608-da270e00a370" macro-name="view-file"><a href="../_attachments/48468918404-deploy_frontend.sh" class="confluence-embedded-file" data-nice-type="null" data-file-src="/wiki/download/attachments/48468918404/deploy_frontend.sh?version=1&amp;modificationDate=1747802741541&amp;cacheVersion=1&amp;api=v2" data-mime-type="text/x-sh" data-has-thumbnail="true">

![[48468918404-deploy_frontend.sh]]

</a></span>

2.3 Deploy with the deployment script

In the folder of the **deploy_frontend.sh**, run the command with the bash terminal:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="8a0ac273-8a24-41f3-972c-7b82e79d9a3a" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
sh deploy_frontend.sh [IMAGE_VERSION]
```

</div>

</div>

- IMAGE_VERSION is the version of the built image from step 1.1, for example:

  <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="38d49c2e-e25f-44d4-a25d-35d1fe7f74c0" macro-name="code" style="border-width: 1px;">

  <div class="codeContent panelContent pdl">

  ``` syntaxhighlighter-pre
  sh deploy_frontend.sh 0.21.11-SNAPSHOT
  ```

  </div>

  </div>

Result:


![[48468918404-image-20250521-050258.png]]



**NOTE: If there is any configuration (except the IMAGE_VERSION) for the frontend environment, please update the .env file on the INT server first, then run this script to correct all configurations.**

Verify the result in the `routing-frontend-logs.log` file at `/opt/integration_services/routing-containerization/routing-frontend/logs`:


![[48468918404-image-20250425-103455.png]]

%% ai-graph-start %%

**Related notes:**
- [[2.31 Build & deploy agent review service to k8s (POC)]]
- [[Deploy AFDEMO CPM]]
- [[Deploy AFDEMO OM]]
- [[Port forward and Docker compose]]
- [[Recipe Deploy with Terraform]]

%% ai-graph-end %%