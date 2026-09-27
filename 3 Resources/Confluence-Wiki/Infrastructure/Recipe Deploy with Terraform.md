---
title: "Recipe: Deploy with Terraform"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/48040149081/Recipe+Deploy+with+Terraform
space: "LUZ"
topic: infra
relevance: 0.87
depth: 3
updated: 2024-09-20
attachments: 9
tags:
  - confluence
  - infra
  - space/luz
---

# Recipe: Deploy with Terraform

> [!info] Imported from Confluence
> Space **LUZ** · updated 2024-09-20 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/48040149081/Recipe+Deploy+with+Terraform)
> Relevance 0.87 · topic `infra`

<div class="toc-macro client-side-toc-macro conf-macro output-block" cssliststyle="none" hasbody="false" headerelements="H1,H2,H3,H4,H5,H6,H7" macro-id="d1aaca1e-a89b-4886-ab06-a1678b1492e0" macro-name="toc">

</div>

# Target

To deploy the terraform configurations defined in luz-kubernetes: <a href="https://bitbucket.org/axonivy-prod/luz_kubernetes/src/master/terraform/" class="external-link" rel="nofollow">https://bitbucket.org/axonivy-prod/luz_kubernetes/src/master/terraform/</a>


![[48040149081-image-20240917-033327.png]]



# Steps

1.  Start a luz-deploy:0.0.2 container on your local machine using the below command:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="b1857532-67bb-4416-9f60-816b4868a210" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
docker run -it -v <directory-of-luz-kubernetes-on-your-local-machine>:/luz_kubernetes gcr.io/klara-repo/luz-deploy:0.0.2 bash

# For example if you cloned luz-kubernetes to C:/work/projects/luz_kubernetes on your local machine, then the command is:
docker run -it -v C:/work/projects/luz_kubernetes:/luz_kubernetes gcr.io/klara-repo/luz-deploy:0.0.2 bash
```

</div>

</div>

After the container has started, move to the luz-kubernetes folder with `cd luz_kubernetes/`


![[48040149081-image-20240917-042201.png]]



2.  Run `gcloud auth application-default login`, then follow the instructions

When prompted with a link, copy and paste that link in a browser on your local machine.


![[48040149081-image-20240917-044029.png]]



Then select an account that have permissions on our GCP project.


![[48040149081-image-20240917-043848.png]]



Continue then allow.


![[48040149081-image-20240917-044045.png]]

![[48040149081-image-20240917-044059.png]]



Then you will received a code, paste that code in the luz-deploy container


![[48040149081-image-20240917-044112.png]]




![[48040149081-image-20240917-044128.png]]



3.  To deploy the terraform configurations, run `./deploy_terraform.sh <environment>`

For example, if you want to deploy terraform configurations for dev environment, run `./deploy_terraform.sh dev` .

If you encounter an error saying `bash: ./deploy_terraform.sh: /bin/bash^M: bad interpreter: No such file or directory`, then run `sed -i -e 's/\r$//' ./deploy_terraform.sh` and run deploy_terraform.sh script again.


![[48040149081-image-20240917-045921.png]]
