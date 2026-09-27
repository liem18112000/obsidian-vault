---
title: "Jenkins - deploy eportal to SE-Server by jenkins"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/X4/pages/47119204477/Jenkins+-+deploy+eportal+to+SE-Server+by+jenkins
space: "X4"
topic: infra
relevance: 0.741
depth: 2.66
updated: 2022-05-31
attachments: 9
tags:
  - confluence
  - infra
  - space/x4
---

# Jenkins - deploy eportal to SE-Server by jenkins

> [!info] Imported from Confluence
> Space **X4** · updated 2022-05-31 · [open original](https://axonivy.atlassian.net/wiki/spaces/X4/pages/47119204477/Jenkins+-+deploy+eportal+to+SE-Server+by+jenkins)
> Relevance 0.741 · topic `infra`

(aka Base-Line-Server, SE=SystemEngineers)

  

go to Jenkins-Server:  <a href="http://3.71.136.55/jenkins/" class="external-link" rel="nofollow">soad jenkins</a>

log in (this way your name will be added to the automatically sent Emails)  
  

![[47119204477-image2020-3-25_8-27-54.png]]



  

go to tab "man-singleton"

choose "***build-deploy-eportal-dev-lnx***"


![[47119204477-image2022-5-31_8-48-15.png]]



on top left choose "Bauen mit Parametern" (build with parameters)

  


![[47119204477-image2020-3-25_8-32-16.png]]



  
Select target which you want to deploy the eportal


![[47119204477-grafik-20220531-065217.png]]



  
Notes:

You can find the Branch in our Nexus repository at <a href="http://3.71.136.55/nexus/" class="external-link" rel="nofollow">soad nexus</a>.

- if version has a snapshot postfix you find it under snapshots repository

- if version has no snapshot postfix you will find artifacts on the continuous repository
