---
ai_hash: 6f2f51f0318ee891
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 1
depth: 2.57
entities: []
relevance: 0.746
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47937716452/Non-Java+modules+work+with+cgroupv2
space: LUZ
status: reference
tags:
- confluence
- programming
- space/luz
title: Non-Java modules work with cgroupv2
topic: programming
type: source
updated: 2024-07-26
---

# Non-Java modules work with cgroupv2

> [!info] Imported from Confluence
> Space **LUZ** · updated 2024-07-26 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47937716452/Non-Java+modules+work+with+cgroupv2)
> Relevance 0.746 · topic `programming`

The information of Non-Java modules are following step of [Upgrade to have cgroupv2 supported](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47924281934/Upgrade+to+have+cgroupv2+supported)

<div>

<table style="width:100%;">
<colgroup>
<col style="width: 14%" />
<col style="width: 14%" />
<col style="width: 14%" />
<col style="width: 14%" />
<col style="width: 14%" />
<col style="width: 14%" />
<col style="width: 14%" />
</colgroup>
<tbody>
<tr>
<th></th>
<th><p><strong>Module</strong></p></th>
<th><p><strong>Team</strong></p></th>
<th><p><strong>Technique</strong></p></th>
<th><p><strong>Running with</strong></p></th>
<th><p><strong>Work with cgroupv2</strong></p></th>
<th><p><strong>Notes</strong></p></th>
</tr>
&#10;<tr>
<td>1</td>
<td><p>luz_reporting_dotnet</p></td>
<td></td>
<td><p>.NET</p></td>
<td><p>sdk:3.1</p></td>
<td><p>

![[47937716452-check.png]]

</p></td>
<td><p>.NET Core is designed to be platform-agnostic and should run in any environment that meets its basic requirements, including a Kubernetes cluster.</p></td>
</tr>
<tr>
<td>2</td>
<td><p>eom</p></td>
<td><p>Pixels</p></td>
<td><p>Angular</p></td>
<td><p>Angular-5</p></td>
<td><p>

![[47937716452-check.png]]

</p></td>
<td><p>Running Angular applications on Wildfly 26 in a cgroup v2 environment should not inherently cause any errors. Angular applications are typically built into static files (HTML, CSS, JavaScript) which are served by the server, in this case, Wildfly. The server doesn't interpret or execute the Angular code, it merely delivers the files to the client's browser to be executed there.</p></td>
</tr>
<tr>
<td>3</td>
<td><p>luz_epc</p></td>
<td><p>Titan</p></td>
<td><p>React Js</p>
<p>Node Js</p></td>
<td><p>react-18.2.0</p>
<p>node-20.4.9</p></td>
<td><p>

![[47937716452-check.png]]

</p>
<p>

![[47937716452-check.png]]

</p></td>
<td><ul>
<li><p>There are multiple projects in one mono-repo called luz_epc. The catalog 'admin-ui' contains frontend ReactJs project. The catalog 'libs' contains some shared code, like DTO objects, utils, shared interfaces etc. These are static files (HTML, CSS, JavaScript) which are served by the server. These files should not cause any errors cgroup v2 environment.</p></li>
<li><p>The catalog “api“ contains the back-end API’s in NodeJs using NestJs framework. This are the all .ts files which not cause any errors cgroup v2 environment.</p></li>
</ul></td>
</tr>
<tr>
<td>4</td>
<td><p>luz_epc_services</p></td>
<td><p>Titan</p></td>
<td><p>Java-Script</p></td>
<td><p>JS-29.5.0</p></td>
<td><p>

![[47937716452-check.png]]

</p></td>
<td><p>These services is used to process the pdf and attachment of smartsend messages.</p>
<p><strong>Services:</strong></p>
<ul>
<li><p>address-normalization</p></li>
<li><p>calculate-cost</p></li>
<li><p>composition</p></li>
<li><p>convert-metadata</p></li>
<li><p>copy-sftp</p></li>
<li><p>dead-letter</p></li>
<li><p>deliveries</p></li>
<li><p>digital-delivery</p></li>
<li><p>has-attachment</p></li>
<li><p>has-message</p></li>
<li><p>merge-metadata</p></li>
<li><p>notify-message-status-changed</p></li>
<li><p>preview</p></li>
<li><p>print</p></li>
<li><p>queue-processor</p></li>
<li><p>send-ebill</p></li>
<li><p>send-postmark</p></li>
<li><p>spoke-delivery</p></li>
<li><p>status-update</p></li>
<li><p>supplement-pages</p></li>
<li><p>validate-ebill-recipient</p></li>
<li><p>validate-metadata</p></li>
<li><p>wait</p></li>
<li><p>wait-attachment</p></li>
<li><p>xml2json</p></li>
</ul></td>
</tr>
</tbody>
</table>

</div>

%% ai-graph-start %%

**Related notes:**
- [[Luz Kubernetes Terraform]]
- [[Performance pain points]]
- [[LUZ DevOps Next Gen (proposal and discussion)]]
- [[Impact of code changes on common components]]
- [[Joint review 0.03.23.00 (02.06.2026 - 15.06.2026)]]

%% ai-graph-end %%