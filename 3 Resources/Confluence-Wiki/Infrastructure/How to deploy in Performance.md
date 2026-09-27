---
title: "How to deploy in Performance"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/WOW/pages/47148467032/How+to+deploy+in+Performance
space: "WOW"
topic: infra
relevance: 0.716
depth: 2.67
updated: 2022-08-09
attachments: 3
tags:
  - confluence
  - infra
  - space/wow
---

# How to deploy in Performance

> [!info] Imported from Confluence
> Space **WOW** · updated 2022-08-09 · [open original](https://axonivy.atlassian.net/wiki/spaces/WOW/pages/47148467032/How+to+deploy+in+Performance)
> Relevance 0.716 · topic `infra`

1.  Go to klara-performance and increase number of nodes for ivy-pool from 0 to 1

    

![[47148467032-ivy_node.PNG]]



2.  Go to:  
    - C:\Program Files (x86)\Google\Cloud SDK\>docker run --rm -it -v D:/workspace/luz/:/root/development/ <a href="http://gcr.io/klara-repo/luz-deploy:0.0.1" class="external-link" data-card-appearance="inline" rel="nofollow">http://gcr.io/klara-repo/luz-deploy:0.0.1</a>

\- cd root/development/luz_kubernetes/

3\. Export deploy to file  
- ./deploy_to_stdout.sh performance \> performance.yaml

4\. Open file performance.yaml and

a\. remove or comment “return 302 \$redirectUrl;“


![[47148467032-redirectUrl.PNG]]



b\. comment code as image below in webclient-nginx-ingress


![[47148467032-maintenance volumn.PNG]]



5\. Deploy all modules in performance  
kubectl -n performance apply -f performance.yaml

6\. delete the modules that Wow does not use (to save cost we have to pay for Google)  
- cd env-performance-tools  
- ./delete-workloads-for-wow.sh

**Notes:**

1.  To deploy in performance individually, yo can use the job: <a href="https://build.axonivy.io/view/LUZ/job/KLARA/view/gcp-build-deployment/job/gcp-deploy-performance-individually/" class="external-link" rel="nofollow">https://build.axonivy.io/view/LUZ/job/KLARA/view/gcp-build-deployment/job/gcp-deploy-performance-individually/</a>  

**Open Points**

1.  there is an issue at klara-maintenance-volume

2.  issue with redirectUrl. Next is checking
