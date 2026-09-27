---
ai_hash: 1eae35c830e5be63
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 6
depth: 2.65
entities: []
relevance: 0.738
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47739797726/One+API+Module+Responsibilities
space: LUZ
status: reference
tags:
- confluence
- programming
- space/luz
title: One API Module Responsibilities
topic: programming
type: source
updated: 2026-01-15
---

# One API Module Responsibilities

> [!info] Imported from Confluence
> Space **LUZ** · updated 2026-01-15 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47739797726/One+API+Module+Responsibilities)
> Relevance 0.738 · topic `programming`

# Modules details

<div>

<table>
<colgroup>
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
</colgroup>
<tbody>
<tr>
<th><p><strong>Module name</strong></p></th>
<th><p><strong>Responsibility</strong></p></th>
<th><p><strong>Comment</strong></p></th>
<th><p><strong>Team</strong></p></th>
<th><p><strong>Implemented</strong></p></th>
</tr>
&#10;<tr>
<td><p>luz-public-api-adapter</p></td>
<td><ul>
<li><p>translates the external (easier) requests for the internal (more complex) APIs</p></li>
<li><p>handle authentication from the outside world to internal KLARA</p></li>
</ul></td>
<td></td>
<td><p>Hacka</p></td>
<td><p>

![[47739797726-2705.png]]

</p></td>
</tr>
<tr>
<td><p>luz-eletter</p></td>
<td><ul>
<li><p>find recipients based on the provided credentials </p></li>
<li><p>implement the identity matching process</p></li>
<li><p>receive and validate letter data</p></li>
<li><p>dispatch documents to appropriate channels</p></li>
<li><p>track delivery data and send to <strong>luz-store</strong> to be billed</p></li>
</ul></td>
<td></td>
<td><p>Hacka</p></td>
<td><p>

![[47739797726-2705.png]]

</p></td>
</tr>
<tr>
<td><p>luz-tenant-dir</p></td>
<td><ul>
<li><p>implements the tenants directory</p></li>
<li><p>implements search and match functionalities regarding tenants</p></li>
</ul></td>
<td></td>
<td><p>Hacka</p></td>
<td><p>

![[47739797726-2705.png]]

</p></td>
</tr>
<tr>
<td><p>luz-address-normalizer</p></td>
<td><ul>
<li><p>communicates with Swiss Post address service to normalize addresses</p></li>
<li><p>normalize addresses before identity matching and storing of addresses</p></li>
</ul></td>
<td></td>
<td><p>Hacka</p></td>
<td><p>

![[47739797726-2705.png]]

</p></td>
</tr>
<tr>
<td><p>luz-docs-view-controller</p></td>
<td><ul>
<li><p>implements view for documents to be presented to the end user</p></li>
<li><p>implements branded folders</p></li>
<li><p>implements special views per document (e.g. for invoices)</p></li>
<li><p>API to store documents, e-letters and archive. The final storage is delegated to <code>luz-docs</code> </p></li>
</ul></td>
<td></td>
<td><p>various teams</p></td>
<td><p>

![[47739797726-2705.png]]

</p></td>
</tr>
<tr>
<td><p>luz-jsonstore</p></td>
<td><ul>
<li><p>high performance tenant specific json document store</p></li>
<li><p>tenant-specific encryption</p></li>
</ul></td>
<td></td>
<td><p>Invisible (John)</p></td>
<td><p>

![[47739797726-2705.png]]

</p></td>
</tr>
<tr>
<td><p>luz-audit</p></td>
<td><ul>
<li><p>stores audit logs</p></li>
<li><p>retrieve audit logs</p></li>
<li><p>export signed audit logs to external storage</p></li>
</ul></td>
<td></td>
<td><p>Kepler</p></td>
<td><p>

![[47739797726-2705.png]]

</p></td>
</tr>
<tr>
<td><p>luz-antivirus</p></td>
<td><ul>
<li><p>Scans files for viruses</p></li>
</ul></td>
<td></td>
<td><p>Avatar</p></td>
<td><p>

![[47739797726-2705.png]]

</p></td>
</tr>
<tr>
<td><p>luz-docs</p></td>
<td><ul>
<li><p>store documents</p></li>
<li><p>find documents</p></li>
</ul></td>
<td></td>
<td><p>Kepler</p></td>
<td><p>

![[47739797726-2705.png]]

</p></td>
</tr>
<tr>
<td><p>luz-vault/luz-vault-unseal</p></td>
<td><ul>
<li><p>Encrypt and decrypt data (metadata, password,…)</p></li>
<li><p>Store data (key or password)</p></li>
</ul></td>
<td></td>
<td><p>Kepler</p></td>
<td><p>

![[47739797726-2705.png]]

</p></td>
</tr>
<tr>
<td><p>luz-sms</p></td>
<td><ul>
<li><p>communicates with 3rd party SMS sending service providers to send real SMS</p></li>
<li><p>track amount of SMS sent and send the consumption data to <strong>luz-store</strong> to be billed</p></li>
</ul></td>
<td></td>
<td><p>Hacka</p></td>
<td><p>

![[47739797726-2705.png]]

</p></td>
</tr>
<tr>
<td><p>luz-doc-output-mgmt</p></td>
<td><ul>
<li><p>communicates with printer modules to send PDF documents to physical addresses</p></li>
<li><p>send consumption data to <strong>luz-store</strong> to be billed</p></li>
</ul></td>
<td></td>
<td><p>Hacka, John</p></td>
<td><p>

![[47739797726-2705.png]]

</p></td>
</tr>
<tr>
<td><p>luz-baumer</p></td>
<td><ul>
<li><p>Integrate with printer BAUMER to send physical documents</p></li>
</ul></td>
<td></td>
<td><p>Hacka</p></td>
<td><p>

![[47739797726-2705.png]]

</p></td>
</tr>
<tr>
<td><p>luz-epost-hub</p></td>
<td><ul>
<li><p>Integrate with printer ePost Hub to send physical documents</p></li>
</ul></td>
<td></td>
<td><p>Hacka</p></td>
<td><p>

![[47739797726-2705.png]]

</p></td>
</tr>
<tr>
<td><p>luz-sps-outline</p></td>
<td><ul>
<li><p>Integrate with printer SPS to send physical documents</p></li>
</ul></td>
<td></td>
<td><p>Hacka</p></td>
<td><p>

![[47739797726-2705.png]]

</p></td>
</tr>
<tr>
<td><p>luz-media-mail</p></td>
<td><ul>
<li><p>Integrate with printer MediaMail to send physical documents</p></li>
</ul></td>
<td></td>
<td><p>Hacka</p></td>
<td><p>

![[47739797726-2705.png]]

</p></td>
</tr>
<tr>
<td><p>luz-ebill-networkpartner</p></td>
<td><ul>
<li><p>find out if user with given credentials uses eBill</p></li>
<li><p>communicates with eBill to send invoices</p></li>
<li><p>send consumption data to luz-store to be billed</p></li>
</ul></td>
<td></td>
<td><p>Hacka</p></td>
<td><p>

![[47739797726-2705.png]]

</p></td>
</tr>
<tr>
<td><p>luz-store</p></td>
<td><ul>
<li><p>create invoices from consumption data sent by other modules</p></li>
</ul></td>
<td></td>
<td><p>various teams</p></td>
<td><p>

![[47739797726-2705.png]]

</p></td>
</tr>
<tr>
<td><p>luz-email</p></td>
<td><ul>
<li><p>send email to email addresses</p></li>
</ul></td>
<td></td>
<td><p>Hacka</p></td>
<td><p>

![[47739797726-2705.png]]

</p></td>
</tr>
<tr>
<td><p>Kong</p></td>
<td><ul>
<li><p>Kong route public API requests to correspondence modules</p></li>
</ul></td>
<td></td>
<td></td>
<td></td>
</tr>
</tbody>
</table>

</div>

%% ai-graph-start %%

**Related notes:**
- [[Collect all calls FileManager APIs by Klara Modules]]
- [[Investigate Analyze the API's which call to FileManager]]
- [[OneAPI Architecture overview]]
- [[Architecture]]
- [[Copy 4. Architecture for delivering eLetter after email verified]]

%% ai-graph-end %%