---
ai_hash: 553ef7bffecc2f61
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 1
depth: 2.45
entities: []
relevance: 0.714
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20435291959/Docker+Cloud+Study
space: LUZ
status: reference
tags:
- confluence
- infra
- space/luz
title: Docker Cloud Study
topic: infra
type: source
updated: 2016-09-07
---

# Docker Cloud Study

> [!info] Imported from Confluence
> Space **LUZ** · updated 2016-09-07 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20435291959/Docker+Cloud+Study)
> Relevance 0.714 · topic `infra`

Docker cloud could be a solution for managing our own docker based infrastructure, if we choose a provider that does not offer a native docker management system: green.ch, cloudscale.ch ...

From <a href="https://docs.docker.com/docker-cloud/getting-started/intro_cloud/" class="external-link" rel="nofollow">https://docs.docker.com/docker-cloud/getting-started/intro_cloud/</a> :

*Docker Cloud is a hosted service that provides a Registry with build and testing facilities for Dockerized application images, tools to help you set up and manage your host infrastructure, and deployment features to help you automate deploying your images to your infrastructure.*

## Steps needed for setting up docker cloud.

1.  ## <span style="font-size: 14.0px;line-height: 1.42857;">Create a docker-cloud account. Example <span class="legacy-color-text-default">: </span><a href="https://cloud.docker.com/" class="external-link" rel="nofollow">https://cloud.docker.com/</a><span class="legacy-color-text-default"> -\> login with dockeraxonivy / docker@axon2016!</span></span>

2.  <div>

    <span style="font-size: 14.0px;line-height: 1.42857;"><span class="legacy-color-text-default">As we come with our own hosting servers we have to link our host with docker-cloud (they provide native support for <span class="legacy-color-text-default">Amazon Web Services, DigitalOcean, Microsoft Azure, <a href="http://Packet.net" class="external-link" rel="nofollow">Packet.net</a>, and IBM SoftLayer).</span></span></span>

    </div>

    <div>

    <span style="font-size: 14.0px;line-height: 1.42857;"><span class="legacy-color-text-default"><span class="legacy-color-text-default"><a href="https://docs.docker.com/docker-cloud/infrastructure/byoh/" class="external-link" rel="nofollow">https://docs.docker.com/docker-cloud/infrastructure/byoh/</a> shows how to install the Docker Cloud Agent on our linux host machine. In a few words:</span></span></span>

    </div>

    1.  <div>

        <span style="font-size: 14.0px;line-height: 1.42857;"><span class="legacy-color-text-default"><span class="legacy-color-text-default"><span class="legacy-color-text-default">... make sure that ports </span><span class="legacy-color-text-default">6783/tcp</span><span class="legacy-color-text-default"> and </span><span class="legacy-color-text-default">6783/udp</span><span class="legacy-color-text-default"> are open on the target host. Optionally, open port </span><span class="legacy-color-text-default">2375/tcp</span><span class="legacy-color-text-default"> too.</span>  
        </span></span></span>

        </div>

    2.  <div>

        <span style="font-size: 14.0px;line-height: 1.42857;"><span class="legacy-color-text-default"><span class="legacy-color-text-default"><span class="legacy-color-text-default">install the docker cloud agent as explained on the documentation page</span></span></span></span>

        </div>

3.  The Docker Cloud Agent has been tested on:

    - Ubuntu 14.04, 15.04, 15.10
    - Debian 8
    - Centos 7
    - Red Hat Enterprise Linux 7
    - Fedora 21, 22, 23

4.  Launch the first nodes cluster: <a href="https://docs.docker.com/docker-cloud/getting-started/your_first_node/" class="external-link" rel="nofollow">https://docs.docker.com/docker-cloud/getting-started/your_first_node/</a> (<span class="legacy-color-text-default">*Node clusters are groups of nodes of the same type and from the same cloud provider, and they allow you to scale the infrastructure by provisioning more nodes with a drag of a slider.*)</span>

5.  <span class="legacy-color-text-default">Deploy a first service: <a href="https://docs.docker.com/docker-cloud/getting-started/your_first_service/" class="external-link" rel="nofollow">https://docs.docker.com/docker-cloud/getting-started/your_first_service/</a> (*<span class="legacy-color-text-default">A service is a group of containers of the same </span><span class="legacy-color-text-default">image:tag</span>*<span class="legacy-color-text-default">*. Services make it simple to scale your application. With Docker Cloud, you simply drag a slider to change the number of containers in a service.*) </span></span>  
    <span class="legacy-color-text-default"><span class="legacy-color-text-default">\*\*\*\*\*\*\* now the service should be running and be accessible to the world...</span></span>  
    <span class="legacy-color-text-default"><span class="legacy-color-text-default">  
    </span></span>

6.  <span class="legacy-color-text-default"><span class="legacy-color-text-default">Learn how to scale the service: <a href="https://docs.docker.com/docker-cloud/apps/service-scaling/" class="external-link" rel="nofollow">https://docs.docker.com/docker-cloud/apps/service-scaling/</a></span></span>

7.  <span class="legacy-color-text-default">Learn more about deploying applications: <a href="https://docs.docker.com/docker-cloud/getting-started/deploy-app/" class="external-link" rel="nofollow">https://docs.docker.com/docker-cloud/getting-started/deploy-app/</a></span>

 

## Costs

<a href="https://www.docker.com/pricing#/pricing_cloud" class="external-link" rel="nofollow">https://www.docker.com/pricing#/pricing_cloud</a>

If I understand it right, it costs \$15 / month for one node (e.g. a node would be the wildfly node)


![[20435291959-image2016-9-7 11-55-6.png]]

%% ai-graph-start %%

**Related notes:**
- [[Setup Build Job with Docker in Jenkins]]
- [[Deploy to Kubernetes and get External IP]]
- [[Port forward and Docker compose]]
- [[GCP Overview]]
- [[One Portainer manages many Docker hosts via portaineragent, not a second Portainer]]

%% ai-graph-end %%