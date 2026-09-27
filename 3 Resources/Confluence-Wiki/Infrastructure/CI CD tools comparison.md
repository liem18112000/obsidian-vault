---
ai_hash: 70dc5f5559c9394c
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 14
depth: 2.17
entities: []
relevance: 0.703
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20496027596/CI+CD+tools+comparison
space: LUZ
status: reference
tags:
- confluence
- infra
- space/luz
title: CI/CD tools comparison
topic: infra
type: source
updated: 2020-05-28
---

# CI/CD tools comparison

> [!info] Imported from Confluence
> Space **LUZ** · updated 2020-05-28 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20496027596/CI+CD+tools+comparison)
> Relevance 0.703 · topic `infra`

Why these tools below are in the list?

As you might know, we are planning on hosting Klara in Google Cloud. This turns out that our job 

<div>

<table style="width: 66.7479%;">
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<tbody>
<tr>
<th>Continuous delivery tool</th>
<th>PROS</th>
<th>CONS</th>
</tr>
&#10;<tr>
<td><div class="content-wrapper">
<p>Jenkins 

![[20496027596-image2019-9-20_10-30-10.png]]


</div></td>
<td>No migration needed when moving to GKE.<br />
Has official container image</td>
<td><br />
</td>
</tr>
<tr>
<td><div class="content-wrapper">
<p>Jenkins X

![[20496027596-image2019-9-20_10-30-21.png]]


</div></td>
<td>No migration needed when moving to GKE</td>
<td>No image container, have to install cli tool and install from it.<br />
It install a lot of thing in cluster. <br />
It default working with githup for manage environment. If working with Bitbucket it need a personal token but Bitbucket doest provide personal token like githup.</td>
</tr>
<tr>
<td><div class="content-wrapper">
<p>Cloud Build

![[20496027596-image2019-9-20_10-29-17.png]]


</div></td>
<td><p>Integrated with various git provider: github, bitbucket...</p>
<p>Code to images support</p>
<p>Fast build</p>
<p>Auto deployment</p>
<p>...</p></td>
<td><p>First 120 build-minutes per day - Free</p>
<p>Additional build-minutes - $0.003 per minute</p></td>
</tr>
<tr>
<td><div class="content-wrapper">
<p>Bamboo</p>
<p><a href="https://www.atlassian.com/software/bamboo" class="external-link" rel="nofollow">

![[20496027596-image2020-5-28_11-10-6.png]]

</a></p>
</div></td>
<td><p>Continuous delivery, from code to deployment</p>
<p>Pipelines support kubectl command - easy to integrate</p>
<p>Run batches of tests in parallel and get feedback quickly</p>
<p>Creates images and pushes into a registry</p>
<p>Per-environment permissions that allow developers and testers to deploy to their environments on-demand while the production stays locked down</p>
<p>Securing environment variables - <a href="https://confluence.atlassian.com/bitbucket/variables-in-pipelines-794502608.html" class="external-link" rel="nofollow">https://confluence.atlassian.com/bitbucket/variables-in-pipelines-794502608.html</a></p>
<p>Well-integrated with Atlassian products (Jira, confluence)</p></td>
<td><p>Of course, it does not built-in with Google Kubernetes</p>
<p>It's not free</p>
<p><br />
</p></td>
</tr>
<tr>
<td><div class="content-wrapper">
<p>Cloud Build Artifacts

![[20496027596-image2019-9-20_10-29-28.png]]


</div></td>
<td><br />
</td>
<td>Alpha release</td>
</tr>
<tr>
<td><div class="content-wrapper">
<p>CI Circle

![[20496027596-image2019-9-20_10-29-41.png]]


</div></td>
<td><p><strong>Automated build, test &amp; deployment for public &amp; private projects</strong></p>
<p>Integrate with GitHub and Bitbucket</p>
<p>Full support for Docker</p>
<p>Orchestrate CI/CD jobs using a declarative YAML syntax</p>
<p>SSH debugging the running jobs</p>
<p>Predefine ORB to reuse</p>
<p>For GKE: <a href="https://circleci.com/orbs/registry/orb/circleci/gcp-gke" class="external-link" rel="nofollow">https://circleci.com/orbs/registry/orb/circleci/gcp-gke</a></p></td>
<td><p>It is not free</p>
<p>$35 / user for self-hosted license</p></td>
</tr>
<tr>
<td><div class="content-wrapper">
<p>Codefresh

![[20496027596-image2019-9-20_10-30-32.png]]


</div></td>
<td><p><strong>Speedy Docker-native CI/CD with an embedded registry and one-click code previews</strong></p>
<p>Best-in-class Kubernetes integration</p>
<p>Works with any K8s cluster<br />
Monitor all of your clusters with the Kubernetes dashboard<br />
Manage Helm releases easier than ever<br />
Flexible Deployment Strategies</p>
<p>==&gt; The same procedure like what we do manually right now</p></td>
<td>NOT free</td>
</tr>
<tr>
<td><div class="content-wrapper">
<p>Codeship

![[20496027596-image2019-9-20_10-37-34.png]]


</div></td>
<td><br />
</td>
<td>NOT free
<div class="content-wrapper">
<p><br />
</p>
</div></td>
</tr>
<tr>
<td><div class="content-wrapper">
<p>Semaphore

![[20496027596-image2019-9-20_10-40-1.png]]


</div></td>
<td><br />
</td>
<td><br />
</td>
</tr>
<tr>
<td><div class="content-wrapper">
<p>Shippable

![[20496027596-image2019-9-20_10-40-13.png]]


</div></td>
<td><br />
</td>
<td><br />
</td>
</tr>
<tr>
<td><div class="content-wrapper">
<p>Spinnaker

![[20496027596-image2019-9-20_10-40-24.png]]


</div></td>
<td><br />
</td>
<td><br />
</td>
</tr>
<tr>
<td><div class="content-wrapper">
<p>TeamCity

![[20496027596-image2019-9-20_10-40-33.png]]


</div></td>
<td><br />
</td>
<td><br />
</td>
</tr>
<tr>
<td><div class="content-wrapper">
<p>Travis CI

![[20496027596-image2019-9-20_10-40-42.png]]


</div></td>
<td><br />
</td>
<td><br />
</td>
</tr>
<tr>
<td><div class="content-wrapper">
<p>Wercker

![[20496027596-image2019-9-20_10-40-51.png]]


</div></td>
<td><br />
</td>
<td><br />
</td>
</tr>
</tbody>
</table>

</div>

%% ai-graph-start %%

**Related notes:**
- [[CI CD (Google Cloud Build & Google Cloud Deploy)]]
- [[CICD for Kogito]]
- [[Jenkins (How to build & deploy)]]
- [[Discussion GCP Release Process with Google Cloud Build]]
- [[Document flow setup build Jenkins job Maven]]

%% ai-graph-end %%