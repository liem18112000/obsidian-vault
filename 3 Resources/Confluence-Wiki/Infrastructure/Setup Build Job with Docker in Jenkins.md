---
ai_hash: 4f4994f558dffa7c
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 5
depth: 2.55
entities: []
relevance: 0.748
source: https://axonivy.atlassian.net/wiki/spaces/LUZCOMP/pages/20675002737/Setup+Build+Job+with+Docker+in+Jenkins
space: LUZCOMP
status: reference
tags:
- confluence
- infra
- space/luzcomp
title: Setup Build Job with Docker in Jenkins
topic: infra
type: source
updated: 2016-03-31
---

# Setup Build Job with Docker in Jenkins

> [!info] Imported from Confluence
> Space **LUZCOMP** · updated 2016-03-31 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZCOMP/pages/20675002737/Setup+Build+Job+with+Docker+in+Jenkins)
> Relevance 0.748 · topic `infra`

## **Introduction**

- In this material, it will guide you to create a job in Jenkins that is automatically built if have any change in source code. After this job runs successfully, a Docker image should be made and pushed to Google Cloud Registry (Google Service Account is already provided by Sven, it is a JSON file).

 

- Here is the environment that I use for implementing this task:

1.  1.  Jenkins ver. 1.614
    2.  Docker 1.9.1
    3.  Ubuntu 14.04 LTS

## **Part 1. Install Ubuntu**

- Please follow this post <a href="http://www.ubuntu.com/download/desktop/install-ubuntu-desktop" class="external-link" rel="nofollow">http://www.ubuntu.com/download/desktop/install-ubuntu-desktop</a>

## **Part 2. Install Jenkins**

- Please follow this post <a href="https://wiki.jenkins-ci.org/display/JENKINS/Installing+Jenkins+on+Ubuntu" class="external-link" rel="nofollow">https://wiki.jenkins-ci.org/display/JENKINS/Installing+Jenkins+on+Ubuntu</a>

## **Part 3. Install Docker**

- Please follow this post <a href="http://imviveka.github.io/2016/01/install-docker-on-ubuntu/" class="external-link" rel="nofollow">http://imviveka.github.io/2016/01/install-docker-on-ubuntu/</a> and follow this post <a href="https://docs.docker.com/machine/install-machine/" class="external-link" rel="nofollow">https://docs.docker.com/machine/install-machine/</a> to install docker-machine tool. After finished installation, we have to configure Docker like this post <a href="https://docs.docker.com/engine/articles/configuring/" class="external-link" rel="nofollow">https://docs.docker.com/engine/articles/configuring/</a>
- Something like details below:  
    

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="1549b451-0a9e-48ba-8a49-d86371a2c662" macro-name="code" style="border-width: 1px;">

<div class="codeHeader panelHeader pdl" style="border-bottom-width: 1px;">

**Configure Docker**

</div>

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
$ sudo vi /etc/default/docker
Then update this variable like this
DOCKER_OPTS="-H tcp://0.0.0.0:4444 -H unix:///var/run/docker.sock"
$ sudo restart docker
```

</div>

</div>

## **Part 4. Create a job in Jenkins**

- ### Configure Maven, JDK


![[20675002737-0.png]]



- ### Create a job in Jenkins


![[20675002737-error.png]]



- ### Add a source code management


![[20675002737-2.png]]



- ### Setup an execute shell to build and push a Docker image to Google Cloud


![[20675002737-3.png]]



 

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="e75191bf-88f2-4d22-a5ab-ab963f613640" macro-name="code" style="border-width: 1px;">

<div class="codeHeader panelHeader pdl" style="border-bottom-width: 1px;">

**Command scripts**

</div>

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
docker -H tcp://192.168.73.128:4444 build -t gcr.io/luzcomp-integration/luz-person:v1 .
docker -H tcp://192.168.73.128:4444 login -e smurfs-deploy@luzcompintegration.
iam.gserviceaccount.com -u _json_key -p "$(cat GoogleServiceAccount.json)" https://gcr.io
docker -H tcp://192.168.73.128:4444 push gcr.io/luzcomp-integration/luz-person:v1
```

</div>

</div>

%% ai-graph-start %%

**Related notes:**
- [[Document flow setup build Jenkins job Maven]]
- [[Recipe Deploy with Terraform]]
- [[Docker Cloud Study]]
- [[Jenkins (How to build & deploy)]]
- [[Deploy to Kubernetes and get External IP]]

%% ai-graph-end %%