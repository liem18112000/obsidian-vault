---
ai_hash: ce0e25785493e221
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 3
depth: 2.67
entities: []
relevance: 0.716
source: https://axonivy.atlassian.net/wiki/spaces/TS/pages/31160275843/Deployment+Process
space: TS
status: reference
tags:
- confluence
- infra
- space/ts
title: Deployment Process
topic: infra
type: source
updated: 2021-02-05
---

# Deployment Process

> [!info] Imported from Confluence
> Space **TS** · updated 2021-02-05 · [open original](https://axonivy.atlassian.net/wiki/spaces/TS/pages/31160275843/Deployment+Process)
> Relevance 0.716 · topic `infra`

Hi everyone in this article I will clarify the current process of our module "Booking". How can we build and deploy this module (Front-end and Backend) to GCP.

As you may know we have a Jenkins View for luz-booking module: <a href="https://build.axonivy.io/view/LUZ/job/KLARA/view/luz_booking/" class="external-link" rel="nofollow">https://build.axonivy.io/view/LUZ/job/KLARA/view/luz_booking/</a>

Under this view you can find many build jobs that help us build and deploy our module to GCP Enviroment.


![[31160275843-image2021-2-5_11-27-36.png]]



# **BUILD**

But you just focus on 3 build jobs as I highlited below:


![[31160275843-image2021-2-5_11-28-56.png]]



## **Backend:**

The build job ***luz_booking*** has responsiblity to build and update image hash to GCP repository for backend module. Whenever you need to update the new code to the repository please use this job.

## **Frontend:**

We will build 2 jobs:

\+ First one is <span class="legacy-color-text-red2">**luz_booking_web**</span>: this job will build the source code for luz_booking_web module, beside that it also update the commit hash to luz_webclient module. As you can see in the image below:


![[31160275843-image2021-2-5_11-32-56.png]]



\+ Second one is <span class="legacy-color-text-red2">**gcp-webclient**</span>: this job has responsiblity to update the latest image itself to the GCP repository and keep these commit hash from other frontend modules will be up to date. And when the timer start to deploy all services and modules it will deploy newest code that contain within the commit hash above of frontend module to new pod.

# **DEPLOY**

**Indivudal Deployment:**

<a href="https://build.axonivy.io/view/LUZ/job/KLARA/view/luz_booking/job/gcp-dev-deploy-individually/" class="external-link" rel="nofollow">https://build.axonivy.io/view/LUZ/job/KLARA/view/luz_booking/job/gcp-dev-deploy-individually/</a>

This job will only deploy a list of modules we list out from the parameters of this job.

**<u>For the Frontend:</u>** It will not re-deploy the webclient that contain all frontend module. It just only copy the \*.iar file into the Ivy folder on current pod of webclient.

*And it will not show the mantainance page to notifiy to user that the deployment is happening *

**Deploy for all modules:**

<a href="https://build.axonivy.io/view/LUZ/job/KLARA/view/luz_booking/job/gcp-dev-deployment/" class="external-link" rel="nofollow">https://build.axonivy.io/view/LUZ/job/KLARA/view/luz_booking/job/gcp-dev-deployment/</a>

This job will redeploy all services and frontend when needed. It means it will check whether the image from repository is different from the latest image. If no it still keep the pod running. But Yes it will terminate the old pod and start the new pod.

%% ai-graph-start %%

**Related notes:**
- [[Jenkins (How to build & deploy)]]
- [[Document flow setup build Jenkins job Maven]]
- [[Kubernetes knowledge]]
- [[Discussion GCP Release Process with Google Cloud Build]]
- [[Deploy Google Cloud Run for new module]]

%% ai-graph-end %%