---
ai_hash: efd10578ca3c34d6
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 0
depth: 2.65
entities: []
relevance: 0.786
source: https://axonivy.atlassian.net/wiki/spaces/IO/pages/48352919566/SSL+certificate+overview
space: IO
status: reference
tags:
- confluence
- security
- space/io
title: SSL certificate overview
topic: security
type: source
updated: 2025-07-16
---

# SSL certificate overview

> [!info] Imported from Confluence
> Space **IO** · updated 2025-07-16 · [open original](https://axonivy.atlassian.net/wiki/spaces/IO/pages/48352919566/SSL+certificate+overview)
> Relevance 0.786 · topic `security`

Axon ICT will inform us 1 month in advance prior certificate expirations.

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
<th><p><strong>Certificates</strong></p></th>
<th><p><strong>Clusters: Environments</strong></p></th>
<th><p><strong>Issuer</strong></p></th>
<th><p><strong>Notes</strong></p></th>
</tr>
&#10;<tr>
<td><p>*.klara.tech</p></td>
<td><p><strong>klara-dev-vn:</strong> dev-vn</p>
<p><strong>klara-nonprod:</strong> dev, dev-staging, test, swissdec</p>
<p><strong>klara-perforamnce:</strong> performance</p></td>
<td><p>Axon ICT (Team Werner)</p></td>
<td><p>07 Feb 2025 renewed</p></td>
</tr>
<tr>
<td><p>*.klara-epost.tech</p></td>
<td><p><strong>klara-dev-vn:</strong> dev-vn</p>
<p><strong>klara-nonprod:</strong> dev, dev-staging, test, swissdec</p>
<p><strong>klara-perforamance:</strong> performance</p>
<p><strong>klara-nonprod-secmail:</strong> dev-secmail, test-secmail</p>
<p><strong>klara-nonprod-synapse:</strong> dev-adelbodenlive-synapse,dev-biz-tenant-fallback-synapse, dev-fchorgen-synapse, dev-staging-adelbodenlive-synapse, dev-staging-fchorgen-synapse, test-adelbodenlive-synapse, test-fchorgen-synapse</p></td>
<td><p>Axon ICT (Team Werner)</p></td>
<td><p>07 Feb 2025 renewed</p></td>
</tr>
<tr>
<td><p>*.online-dev.klara.tech</p></td>
<td><p><strong>klara-nonprod:</strong> dev</p></td>
<td><p>Axon ICT (Team Werner)</p></td>
<td><p>16 Jul 2025 renewed<br />
<a href="https://axonivy.atlassian.net/wiki/x/RQCyTws" data-card-appearance="inline" rel="nofollow">https://axonivy.atlassian.net/wiki/x/RQCyTws</a></p></td>
</tr>
<tr>
<td><p>*.online-dev-staging.klara.tech</p></td>
<td><p><strong>klara-nonprod:</strong> dev-staging</p></td>
<td><p>KLARA/ePost</p></td>
<td><p>14 Feb 2025 renewed</p></td>
</tr>
<tr>
<td><p>*.online-test.klara.tech</p></td>
<td><p><strong>klara-nonprod:</strong> test</p></td>
<td><p>Axon ICT (Team Werner)</p></td>
<td></td>
</tr>
<tr>
<td><p>*.online-swissdec.klara.tech</p></td>
<td><p><strong>klara-nonprod:</strong> swissdec</p></td>
<td><p>KLARA/ePost</p></td>
<td><p>*.klara.tech certificate used</p></td>
</tr>
<tr>
<td><p>*.online-dev-vn.klara.tech</p></td>
<td><p><strong>klara-dev-vn:</strong> dev-vn</p></td>
<td><p>Axon ICT (Team Werner)</p></td>
<td><p>24 Feb 2025 renewed</p></td>
</tr>
<tr>
<td><p>*.online-performance.klara.tech</p></td>
<td><p><strong>klara-perforamnce:</strong> performance</p></td>
<td><p>KLARA/ePost</p></td>
<td><p>*.klara.tech certificate used</p></td>
</tr>
<tr>
<td><p>*.klara.ch</p></td>
<td><p><strong>klara-prod:</strong> prod</p></td>
<td><p>Axon ICT (Team Werner)</p></td>
<td></td>
</tr>
<tr>
<td><p>*.epost.ch</p></td>
<td><p><strong>klara-prod:</strong> prod</p>
<p><strong>klara-prod-secmail:</strong> prod-secmail</p>
<p><strong>APOLLON KLARA:</strong> MFT (goAnywhere)</p></td>
<td><p>Axon ICT (Team Werner)</p></td>
<td></td>
</tr>
<tr>
<td><p>*.online.klara.ch</p></td>
<td><p><strong>klara-prod:</strong> prod</p></td>
<td><p>Axon ICT (Team Werner)</p></td>
<td></td>
</tr>
</tbody>
</table>

</div>

%% ai-graph-start %%

**Related notes:**
- [[SSL certificate for KLARA Website (own domain)]]
- [[mcp.klara.ch current evaluation]]
- [[Update ePost certificate - .epost.ch - in PROD]]
- [[Security]]
- [[Get tenant token from public api]]

%% ai-graph-end %%