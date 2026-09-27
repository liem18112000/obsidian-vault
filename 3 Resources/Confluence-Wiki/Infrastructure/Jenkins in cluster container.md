---
title: "Jenkins in cluster container"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/AII/pages/3517581925/Jenkins+in+cluster+container
space: "AII"
topic: infra
relevance: 0.706
depth: 2.38
updated: 2017-03-06
attachments: 6
tags:
  - confluence
  - infra
  - space/aii
---

# Jenkins in cluster container

> [!info] Imported from Confluence
> Space **AII** · updated 2017-03-06 · [open original](https://axonivy.atlassian.net/wiki/spaces/AII/pages/3517581925/Jenkins+in+cluster+container)
> Relevance 0.706 · topic `infra`

In this concept, the material we need is:

- 1 Instance to install NFS services as a share storage
- 1 ECS cluster: We can add 1,2 EC2. That container instances(node) will install nfs client and mount volume from NFS servers. The number of nodes can be change base on auto scaling policy.
- 1 ELB: Due to Jenkins service will run in multi node, so we will use this service to route traffic, it will generate a unique dns name, so we just use this dns name to access services, no matter this service is running in which hosts
- 1 Auto scaling: use to scale in or scale our instance and containers
- 1 S3 bucket: This bucket will be mount to cluster node as a volume. Artifactory container will using this volume to store artifact. So we should identifiy which path will store artifact to map volume.
- Cloudwatch is used for Auto scaling: will trigger when instance memory utilization more than 80% and greater than 25%.

  

  


![[3517581925-ECS Autoscaling.jpg]]



  

  

  

  

<u>**Difficulty**</u>

- Jenkins not work well in Cluster.


![[3517581925-image2017-3-2_14-4-23.png]]



- Appearance in Jenkins not correct if using 2 node even they use the same home directory. For example: if access Jenkins in node 1 and create new project, then login to jenkins in node 2, can not see new project until restart jenkins service in node 2.
- Copy file to S3 slow, so it could affect build process. Overall, it slower than local 49 times.  
    
  

![[3517581925-image2017-3-2_14-57-57.png]]



  

-   
  

![[3517581925-unknown-attachment.png]]

![[3517581925-unknown-attachment.png]]
