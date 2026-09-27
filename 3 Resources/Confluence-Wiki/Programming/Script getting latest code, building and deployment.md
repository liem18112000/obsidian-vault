---
title: "Script getting latest code, building and deployment."
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/TK/pages/30660183662/Script+getting+latest+code+building+and+deployment.
space: "TK"
topic: programming
relevance: 0.81
depth: 2.76
updated: 2020-09-17
attachments: 7
tags:
  - confluence
  - programming
  - space/tk
---

# Script getting latest code, building and deployment.

> [!info] Imported from Confluence
> Space **TK** · updated 2020-09-17 · [open original](https://axonivy.atlassian.net/wiki/spaces/TK/pages/30660183662/Script+getting+latest+code+building+and+deployment.)
> Relevance 0.81 · topic `programming`

# Ivy Projects

- Download this script [[30660183662-ivy_script_latest_code_and_run_maven.sh|ivy_script_latest_code_and_run_maven.sh]] and placed to ivy project folder.
- Run in cmd ./[[30660183662-ivy_script_latest_code_and_run_maven.sh|ivy_script_latest_code_and_run_maven.sh]]
- Make sure there is no error during getting latest code and building.  
  

![[30660183662-image2020-6-25_10-54-32.png]]



# Service Projects

- Make sure your local wildfly is running.
- Download these script: [[30660183662-service_script_latest_code_and_run_maven.sh|service_script_latest_code_and_run_maven.sh]] and [[30660183662-service_deployment.sh|service_deployment.sh]]. Copy these files to service project folder.  
  <span class="legacy-color-text-red2">Note: for [[30660183662-service_deployment.sh|service_deployment.sh]] please change to your local path</span>  
  

![[30660183662-image2020-9-17_14-5-21.png]]


- Only run in cmd ./[[30660183662-service_script_latest_code_and_run_maven.sh|service_script_latest_code_and_run_maven.sh]]
- Please make sure that there is no error during get latest code, build and deployment.  
  

![[30660183662-image2020-6-25_13-45-41.png]]


- 

![[30660183662-image2020-6-25_10-52-0.png]]


- If there are some errors during deployment script, please stop wildfly and start it again. Run this script [[30660183662-service_deployment.sh|service_deployment.sh]] to deploy by running in cmd ./[[30660183662-service_deployment.sh|service_deployment.sh.]]
