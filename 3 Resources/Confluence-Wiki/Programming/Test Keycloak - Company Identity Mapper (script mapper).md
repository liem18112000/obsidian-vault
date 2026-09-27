---
ai_hash: d096058f85b4e90b
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 18
depth: 3
entities: []
relevance: 0.792
source: https://axonivy.atlassian.net/wiki/spaces/TS/pages/47603253249/Test+Keycloak+-+Company+Identity+Mapper+script+mapper
space: TS
status: reference
tags:
- confluence
- programming
- space/ts
title: Test Keycloak - Company Identity Mapper (script mapper)
topic: programming
type: source
updated: 2024-01-31
---

# Test Keycloak - Company Identity Mapper (script mapper)

> [!info] Imported from Confluence
> Space **TS** · updated 2024-01-31 · [open original](https://axonivy.atlassian.net/wiki/spaces/TS/pages/47603253249/Test+Keycloak+-+Company+Identity+Mapper+script+mapper)
> Relevance 0.792 · topic `programming`

KLARA uses the custom id to identify the company, so we have a script to map those when serialize/deserialize the token for public apis

<div>

<table style="width:100%;">
<colgroup>
<col style="width: 16%" />
<col style="width: 16%" />
<col style="width: 16%" />
<col style="width: 16%" />
<col style="width: 16%" />
<col style="width: 16%" />
</colgroup>
<tbody>
<tr>
<th><p><strong>Case</strong></p></th>
<th><p><strong>Steps</strong></p></th>
<th><p><strong>Expect</strong></p></th>
<th><p><strong>DEV</strong></p></th>
<th><p><strong>DEV-STAGING</strong></p>
<p>(02.62)</p></th>
<th><p><strong>DEV-STAGING</strong></p>
<p>(02.63)</p></th>
</tr>
&#10;<tr>
<td><p>Get token from Public API</p></td>
<td><ul>
<li><p>Go to <a href="https://api-dev.klara.tech/docs#/Authentication/post_core_latest_token" class="external-link" data-card-appearance="inline" rel="nofollow">https://api-dev.klara.tech/docs#/Authentication/post_core_latest_token</a></p></li>
<li><p>In <code>Authentication</code>, get token via API: <strong>/core/latest/token</strong></p></li>
<li><p>Fulfill the informations</p></li>
<li><p>Decode the access_token in jwt.io</p></li>
</ul></td>
<td><p>Token must include the tenant id and company id</p></td>
<td><p>

![[47603253249-check.png]]

</p>

![[47603253249-image-20231226-083352.png]]

</td>
<td><p>

![[47603253249-check.png]]

</p>

![[47603253249-image-20240103-032911.png]]

</td>
<td><p>

![[47603253249-check.png]]

</p></td>
</tr>
</tbody>
</table>

</div>

%% ai-graph-start %%

**Related notes:**
- [[Test Keycloak - Public API]]
- [[How to use Public API to create update KLARA Business Company]]
- [[Get tenant token from public api]]
- [[Understanding Keycloak Authorization Code flow]]
- [[Use KLARA Swagger UI for REST API]]

%% ai-graph-end %%