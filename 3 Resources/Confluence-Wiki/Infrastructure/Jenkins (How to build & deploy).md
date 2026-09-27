---
ai_hash: 287f3e8a3cb4245f
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 9
depth: 2.53
entities: []
relevance: 0.734
source: https://axonivy.atlassian.net/wiki/spaces/TS/pages/46935703602/Jenkins+How+to+build+deploy
space: TS
status: reference
tags:
- confluence
- infra
- space/ts
title: Jenkins (How to build & deploy)
topic: infra
type: source
updated: 2022-04-15
---

# Jenkins (How to build & deploy)

> [!info] Imported from Confluence
> Space **TS** · updated 2022-04-15 · [open original](https://axonivy.atlassian.net/wiki/spaces/TS/pages/46935703602/Jenkins+How+to+build+deploy)
> Relevance 0.734 · topic `infra`

# **List of modules on Jenkins:**

<div id="expander-2045667763" class="expand-container conf-macro output-block" hasbody="true" macro-id="2e0e14dc-6bd9-4ecd-b2f4-347bb7e57d7a" macro-name="expand">

<div id="expander-control-2045667763" class="expand-control">

<span class="expand-control-icon icon"> </span><span class="expand-control-text">Table of deploy-jobs to servers</span>

</div>

<div id="expander-content-2045667763" class="expand-content expand-hidden">

## \*Note:

<div>

<table>
<colgroup>
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
</colgroup>
<tbody>
<tr>
<th><p><strong>Server</strong></p></th>
<th><p><strong>Name</strong></p></th>
<th><p><strong>Link</strong></p></th>
<th><p><strong>Guide image</strong></p></th>
</tr>
&#10;<tr>
<td><p>Local</p></td>
<td><h2 id="Jenkins(Howtobuild&amp;deploy)-deploy_services_to_local"><a href="https://build.axonivy.io/job/KLARA/job/deploy_services_to_local/" class="external-link" rel="nofollow">deploy_services_to_local</a></h2></td>
<td><p><a href="https://build.axonivy.io/job/KLARA/job/deploy_services_to_local/" class="external-link" rel="nofollow">https://build.axonivy.io/job/KLARA/job/deploy_services_to_local/</a></p></td>
<td></td>
</tr>
<tr>
<td><p>DEV-VN</p></td>
<td><h2 id="Jenkins(Howtobuild&amp;deploy)-gcp-dev-vn-deploy-individually"><a href="https://build.axonivy.io/view/LUZ/job/KLARA/view/luz_booking/job/gcp-dev-vn-deploy-individually/" class="external-link" rel="nofollow">gcp-dev-vn-deploy-individually</a></h2></td>
<td><p><a href="https://build.axonivy.io/view/LUZ/job/KLARA/view/luz_booking/job/gcp-dev-vn-deploy-individually/" class="external-link" rel="nofollow">https://build.axonivy.io/view/LUZ/job/KLARA/view/luz_booking/job/gcp-dev-vn-deploy-individually/</a></p></td>
<td>

![[46935703602-image-20211004-101126.png]]

</td>
</tr>
<tr>
<td><p>DEV</p></td>
<td><h2 id="Jenkins(Howtobuild&amp;deploy)-gcp-dev-deploy-individually"><a href="https://build.axonivy.io/view/LUZ/job/KLARA/view/luz_booking/job/gcp-dev-deploy-individually/" class="external-link" rel="nofollow">gcp-dev-deploy-individually</a></h2></td>
<td><p><a href="https://build.axonivy.io/view/LUZ/job/KLARA/view/luz_booking/job/gcp-dev-deploy-individually/" class="external-link" rel="nofollow">https://build.axonivy.io/view/LUZ/job/KLARA/view/luz_booking/job/gcp-dev-deploy-individually/</a></p></td>
<td></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="expander-294590634" class="expand-container conf-macro output-block" hasbody="true" macro-id="77919ba1-a14a-4073-837d-3f2566036374" macro-name="expand">

<div id="expander-control-294590634" class="expand-control">

<span class="expand-control-icon icon"> </span><span class="expand-control-text">Table of modules on Jenkins</span>

</div>

<div id="expander-content-294590634" class="expand-content expand-hidden">

## \*Note:

- Front-end: Ivy project

- Back-end: Service project

- Modules that have suffix “\_web” (Ex: luz_booking_web, …) are often in Front-end. (exclude: luz_online_web)

- Modules that have suffix without “\_web” (Ex: luz_booking, …) are often in Back-end.

<div>

<table>
<colgroup>
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
</colgroup>
<tbody>
<tr>
<th><p><strong>Module name</strong></p></th>
<th><p><strong>Side (FE or BE)</strong></p></th>
<th><p><strong>Image</strong></p></th>
<th><p><strong>Description / Note</strong></p></th>
</tr>
&#10;<tr>
<td><p><a href="https://build.axonivy.io/view/LUZ/job/KLARA/job/luz_components/" class="external-link" rel="nofollow">luz_components</a></p></td>
<td><p>FE</p></td>
<td></td>
<td><ul>
<li><p>URL: <a href="https://build.axonivy.io/view/LUZ/job/KLARA/job/luz_components/" class="external-link" rel="nofollow">https://build.axonivy.io/view/LUZ/job/KLARA/job/luz_components/</a></p></li>
</ul></td>
</tr>
<tr>
<td><p><a href="https://build.axonivy.io/view/LUZ/job/KLARA/job/luz_online/" class="external-link" rel="nofollow">luz_online</a></p></td>
<td><p>BE</p></td>
<td>

![[46935703602-image-20211013-041358.png]]

</td>
<td></td>
</tr>
<tr>
<td><p><a href="https://build.axonivy.io/view/LUZ/job/KLARA/job/luz_online_web/" class="external-link" rel="nofollow"><span>luz_online_web</span></a></p></td>
<td><p><strong><span>BE</span></strong>, although it has suffix “_web”</p></td>
<td></td>
<td><ul>
<li><p>Build type: <strong><span>Automatically </span></strong>(when merge PR). Update: 23/02/2022</p></li>
<li><p>URL: <a href="https://build.axonivy.io/view/LUZ/job/KLARA/job/luz_online_web/" class="external-link" rel="nofollow">https://build.axonivy.io/view/LUZ/job/KLARA/job/luz_online_web/</a></p></li>
</ul></td>
</tr>
<tr>
<td><p><a href="https://build.axonivy.io/view/LUZ/job/KLARA/view/luz_booking/job/luz_booking/" class="external-link" rel="nofollow">luz_booking</a></p></td>
<td><p>BE</p></td>
<td>

![[46935703602-image-20211013-042126.png]]

![[46935703602-image-20211013-042431.png]]

</td>
<td><ul>
<li><p>Build type: <strong><span>Manually</span></strong>. Update: 23/02/2022</p></li>
<li><p>URL: <a href="https://build.axonivy.io/view/LUZ/job/KLARA/view/luz_booking/job/luz_booking/" class="external-link" rel="nofollow">https://build.axonivy.io/view/LUZ/job/KLARA/view/luz_booking/job/luz_booking/</a></p></li>
</ul>

![[46935703602-image-20220223-043610.png]]


<p>Step-by-step:</p>
<ul>
<li><p>Step 1: Fill luz_booking branch to build (often is <strong>master</strong>)</p></li>
<li><p>Step 2: Check to is_added_hash_to_yaml</p></li>
<li><p>Step 3: Click to Build</p></li>
</ul></td>
</tr>
<tr>
<td><p><a href="https://build.axonivy.io/view/LUZ/job/KLARA/view/luz_booking/job/luz_booking_web/" class="external-link" rel="nofollow">luz_booking_web</a></p></td>
<td><p>FE</p></td>
<td></td>
<td><ul>
<li><p>Build type: <strong><span>Automatically </span></strong>(when merge PR). Update: 23/02/2022</p></li>
<li><p>URL: <a href="https://build.axonivy.io/view/LUZ/job/KLARA/view/luz_booking/job/luz_booking_web/" class="external-link" rel="nofollow">https://build.axonivy.io/view/LUZ/job/KLARA/view/luz_booking/job/luz_booking_web/</a></p></li>
</ul></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

# **Jenkins’s tips & tricks**

<div id="expander-1034724425" class="expand-container conf-macro output-block" hasbody="true" macro-id="ee9b5e60-b86d-4ea7-b969-c48d40304dad" macro-name="expand">

<div id="expander-control-1034724425" class="expand-control">

<span class="expand-control-icon icon"> </span><span class="expand-control-text">Q01: How to deploy multi-branches without waiting approved PR?</span>

</div>

<div id="expander-content-1034724425" class="expand-content expand-hidden">


![[46935703602-image-20211007-083257.png]]



</div>

</div>

<div id="expander-1142992735" class="expand-container conf-macro output-block" hasbody="true" macro-id="81c0c48d-dcd2-4a22-9eab-23de2bf30b2f" macro-name="expand">

<div id="expander-control-1142992735" class="expand-control">

<span class="expand-control-icon icon"> </span><span class="expand-control-text">Q02: How to build Front-end projects?</span>

</div>

<div id="expander-content-1142992735" class="expand-content expand-hidden">


![[46935703602-image-20211004-094906.png]]



</div>

</div>

<div id="expander-284288552" class="expand-container conf-macro output-block" hasbody="true" macro-id="c9912c46-3355-4757-aeb2-9c441bcda7ff" macro-name="expand">

<div id="expander-control-284288552" class="expand-control">

<span class="expand-control-icon icon"> </span><span class="expand-control-text">Q03: How to build Back-end projects?</span>

</div>

<div id="expander-content-284288552" class="expand-content expand-hidden">


![[46935703602-image-20211004-095338.png]]



</div>

</div>

# **Tutorials**

<div id="expander-667711154" class="expand-container conf-macro output-block" hasbody="true" macro-id="7cbf104e-112b-41b5-a92f-93ab0a9f261b" macro-name="expand">

<div id="expander-control-667711154" class="expand-control">

<span class="expand-control-icon icon"> </span><span class="expand-control-text">Tutorials:</span>

</div>

<div id="expander-content-667711154" class="expand-content expand-hidden">

Location folder: **\ct-fsr\Teams\Miracle\Videos\Jenkins**

- Video \#01: **Jenkins_guideline_byThien.mp4** (**00:23:21**)


![[46935703602-image-20210922-072619.png]]



</div>

</div>

# **References:**

<div id="expander-1510001836" class="expand-container conf-macro output-block" hasbody="true" macro-id="71fdf0fb-edd3-43e9-8f9b-461645542c02" macro-name="expand">

<div id="expander-control-1510001836" class="expand-control">

<span class="expand-control-icon icon"> </span><span class="expand-control-text">Documents & Useful links:</span>

</div>

<div id="expander-content-1510001836" class="expand-content expand-hidden">

- Official Jenkins documentation: <a href="https://www.jenkins.io/doc/" class="external-link" data-card-appearance="inline" rel="nofollow">https://www.jenkins.io/doc/</a>

</div>

</div>

# **\*Notes:**

<div id="expander-1780652643" class="expand-container conf-macro output-block" hasbody="true" macro-id="5846c8b6-3195-490c-a524-60af8eac393a" macro-name="expand">

<div id="expander-control-1780652643" class="expand-control">

<span class="expand-control-icon icon"> </span><span class="expand-control-text">Notes:</span>

</div>

<div id="expander-content-1780652643" class="expand-content expand-hidden">

- If you don’t see “**Build with parameters**” on left-side bar, maybe you not logged in.

- After click “Build”, no need to replace files.war in localhost/management.

</div>

</div>

------------------------------------------------------------------------

# **Definitions:**

%% ai-graph-start %%

**Related notes:**
- [[Deployment Process]]
- [[Document flow setup build Jenkins job Maven]]
- [[Kubernetes knowledge]]
- [[One API end to end testing]]
- [[Luz Kubernetes Terraform]]

%% ai-graph-end %%