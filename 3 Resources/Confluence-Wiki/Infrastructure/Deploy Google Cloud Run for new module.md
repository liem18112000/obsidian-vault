---
title: "Deploy Google Cloud Run for new module"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/Helios/pages/48903651398/Deploy+Google+Cloud+Run+for+new+module
space: "Helios"
topic: infra
relevance: 0.844
depth: 3
updated: 2025-11-25
attachments: 8
tags:
  - confluence
  - infra
  - space/helios
---

# Deploy Google Cloud Run for new module

> [!info] Imported from Confluence
> Space **Helios** · updated 2025-11-25 · [open original](https://axonivy.atlassian.net/wiki/spaces/Helios/pages/48903651398/Deploy+Google+Cloud+Run+for+new+module)
> Relevance 0.844 · topic `infra`

1.  **Create new module using devportal**


![[48903651398-image-20251124-041652.png]]

![[48903651398-image-20251124-041756.png]]

![[48903651398-image-20251124-043422.png]]



After few minutes, new module created and you can download it.


![[48903651398-image-20251124-043616.png]]



- Extract the file and import it into the IDE. The project structure looks like this:  

  

![[48903651398-image-20251124-043835.png]]



- In the deployment-scripts:  

  

![[48903651398-image-20251124-044143.png]]



  - <a href="http://main.tf" class="external-link" rel="nofollow">main.tf</a>: change the image hash in the image_tag

  - For image hash, need to build the module and push to docker_registry url:  
    `europe-west6-docker.pkg.dev/klara-repo/artifact-registry-container-images`

2.  Config kubernetes to deploy GCR:

- Access to luz-kubernetes/terraform → create new folder as your module name.

- Create the Terraform configuration file (Can use AI generate it). You can follow the structure shown below.

  

![[48903651398-image-20251124-064910.png]]



- Now, we can deploy GCR with luz-kubernetes

  - Run command `gcloud auth application-default login`, enter the code to authorize

  - Download Terraform, add environment variables for it

  - Then run this command

    <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="faa009bc-6cc1-41d1-83cc-b1f7b9325f35" macro-name="code" style="border-width: 1px;">

    <div class="codeContent panelContent pdl">

    ``` syntaxhighlighter-pre
    terraform init
    ```

    </div>

    </div>

- Create new workspace`terraform workspace new dev`

- Choose the workspace `terraform workspace select dev`

- Create a plan to deploy `terraform plan -target=module.{your-module-name}`

- Apply it `terraform apply -target=module.luz-epc-ws-gateway`

Now, you can see your module deployed on GCR as below


![[48903651398-image-20251124-070049.png]]
