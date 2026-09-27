---
title: "SSL Certificate Providers"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20519715360/SSL+Certificate+Providers
space: "LUZ"
topic: security
relevance: 0.832
depth: 3
updated: 2020-11-23
attachments: 0
tags:
  - confluence
  - security
  - space/luz
---

# SSL Certificate Providers

> [!info] Imported from Confluence
> Space **LUZ** · updated 2020-11-23 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20519715360/SSL+Certificate+Providers)
> Relevance 0.832 · topic `security`

All of the SSL certificate providers that support the URL validation method(<a href="https://wiki.hexonet.net/wiki/SSL#tab=Other_commands__28API_29" class="external-link" rel="nofollow">https://wiki.hexonet.net/wiki/SSL#tab=Other_commands__28API_29</a>)

<div>

<table style="width: 100.0%;">
<colgroup>
<col style="width: 10%" />
<col style="width: 10%" />
<col style="width: 10%" />
<col style="width: 10%" />
<col style="width: 10%" />
<col style="width: 10%" />
<col style="width: 10%" />
<col style="width: 10%" />
<col style="width: 10%" />
<col style="width: 10%" />
</colgroup>
<tbody>
<tr>
<th style="text-align: center; width: 5.85586%;"><span class="legacy-color-text-default">Provider</span></th>
<th style="text-align: center; width: 14.3393%;"><span class="legacy-color-text-default">Name</span></th>
<th style="text-align: center; width: 19.2192%;"><span class="legacy-color-text-default">Class</span></th>
<th style="text-align: center; width: 9.38438%;"><span class="legacy-color-text-default">Type</span></th>
<th style="text-align: center; width: 8.78378%;"><span class="legacy-color-text-default">Additional Domains</span></th>
<th style="text-align: center; width: 14.5646%;"><span class="legacy-color-text-default">Domain Control Validation (DCV)</span></th>
<th style="text-align: center; width: 5.63063%;">Security level</th>
<th style="text-align: center; width: 5.63063%;">SAN support</th>
<th style="text-align: center; width: 6.23123%;">Wildcard support</th>
<th style="text-align: center; width: 10.2853%;">Price(per year)</th>
</tr>
&#10;<tr>
<td rowspan="3" style="text-align: center; width: 5.85586%;"><span class="legacy-color-text-default">Sectigo</span><br />
<br />
<br />
<br />
</td>
<td style="text-align: center; width: 14.3393%;"><span class="legacy-color-text-default"><a href="https://sectigostore.com/ssl-certificates/essentialssl" class="external-link" rel="nofollow">Essential SSL</a></span></td>
<td style="text-align: center; width: 19.2192%;"><span class="legacy-color-text-default">COMODO_ESSENTIALSSL</span></td>
<td style="text-align: center; width: 9.38438%;"><span class="legacy-color-text-default">Domain Validation (DV)</span></td>
<td style="text-align: center; width: 8.78378%;">-</td>
<td style="text-align: center; width: 14.5646%;"><span class="legacy-color-text-default">EMAIL,DNSZONE,URL</span></td>
<td style="text-align: center; width: 5.63063%;">Basic</td>
<td style="text-align: center; width: 5.63063%;">No</td>
<td style="text-align: center; width: 6.23123%;">No</td>
<td style="text-align: center; width: 10.2853%;">16.9 USD</td>
</tr>
<tr>
<td style="text-align: center; width: 14.3393%;"><span class="legacy-color-text-default"><a href="https://sectigostore.com/ssl-certificates/positivessl" class="external-link" rel="nofollow">Positive SSL</a></span></td>
<td style="text-align: center; width: 19.2192%;"><span class="legacy-color-text-default">COMODO_POSITIVESSL</span></td>
<td style="text-align: center; width: 9.38438%;"><span class="legacy-color-text-default">Domain Validation (DV)</span></td>
<td style="text-align: center; width: 8.78378%;">-</td>
<td style="text-align: center; width: 14.5646%;"><span class="legacy-color-text-default">EMAIL,DNSZONE,URL</span></td>
<td style="text-align: center; width: 5.63063%;">Basic</td>
<td style="text-align: center; width: 5.63063%;">No</td>
<td style="text-align: center; width: 6.23123%;">No</td>
<td style="text-align: center; width: 10.2853%;">11.9 USD</td>
</tr>
<tr>
<td style="text-align: center; width: 14.3393%;"><span class="legacy-color-text-default">DV SSL</span></td>
<td style="text-align: center; width: 19.2192%;"><span class="legacy-color-text-default">COMODO_SSL</span></td>
<td style="text-align: center; width: 9.38438%;"><span class="legacy-color-text-default">Domain Validation (DV)</span></td>
<td style="text-align: center; width: 8.78378%;">-</td>
<td style="text-align: center; width: 14.5646%;"><span class="legacy-color-text-default">EMAIL,DNSZONE,URL</span></td>
<td style="text-align: center; width: 5.63063%;">Basic</td>
<td style="text-align: center; width: 5.63063%;">No</td>
<td style="text-align: center; width: 6.23123%;">No</td>
<td style="text-align: center; width: 10.2853%;">79.9 USD</td>
</tr>
<tr>
<td rowspan="4" style="text-align: left; width: 5.85586%;">GeoTrust</td>
<td style="text-align: left; width: 14.3393%;">Quick SSL</td>
<td style="text-align: center; width: 19.2192%;">GEOTRUST_QUICKSSL</td>
<td style="text-align: center; width: 9.38438%;">Domain Validation (DV)</td>
<td style="text-align: center; width: 8.78378%;">-</td>
<td style="text-align: center; width: 14.5646%;">EMAIL,DNSZONE,URL</td>
<td style="text-align: center; width: 5.63063%;">Basic</td>
<td style="text-align: center; width: 5.63063%;">No</td>
<td style="text-align: center; width: 6.23123%;">No</td>
<td style="text-align: center; width: 10.2853%;">49 USD</td>
</tr>
<tr>
<td style="text-align: left; width: 14.3393%;">Quick SSL Premium</td>
<td style="text-align: center; width: 19.2192%;">GEOTRUST_QUICKSSLPREMIUM</td>
<td style="text-align: center; width: 9.38438%;">Domain Validation (DV)</td>
<td style="text-align: center; width: 8.78378%;">-</td>
<td style="text-align: center; width: 14.5646%;">EMAIL,DNSZONE,URL</td>
<td style="text-align: center; width: 5.63063%;">Basic</td>
<td style="text-align: center; width: 5.63063%;">No</td>
<td style="text-align: center; width: 6.23123%;">No</td>
<td style="text-align: center; width: 10.2853%;">69.9 USD</td>
</tr>
<tr>
<td style="text-align: left; width: 14.3393%;">Quick SSL Premium SAN Package</td>
<td style="text-align: center; width: 19.2192%;">GEOTRUST_QUICKSSLPREMIUM_SAN</td>
<td style="text-align: center; width: 9.38438%;">Domain Validation (DV)</td>
<td style="text-align: center; width: 8.78378%;">4 subdomains</td>
<td style="text-align: center; width: 14.5646%;">EMAIL,DNSZONE,URL</td>
<td style="text-align: center; width: 5.63063%;">Basic</td>
<td style="text-align: center; width: 5.63063%;">Yes</td>
<td style="text-align: center; width: 6.23123%;">No</td>
<td style="text-align: center; width: 10.2853%;">95 USD</td>
</tr>
<tr>
<td style="text-align: left; width: 14.3393%;"><a href="https://www.rapidssl.com/buy-ssl/" class="external-link" rel="nofollow">Rapid SSL</a></td>
<td style="text-align: center; width: 19.2192%;">GEOTRUST_RAPIDSSL</td>
<td style="text-align: center; width: 9.38438%;">Domain Validation (DV)</td>
<td style="text-align: center; width: 8.78378%;">-</td>
<td style="text-align: center; width: 14.5646%;">EMAIL,DNSZONE,URL</td>
<td style="text-align: center; width: 5.63063%;">Basic</td>
<td style="text-align: center; width: 5.63063%;">No</td>
<td style="text-align: center; width: 6.23123%;">No</td>
<td style="text-align: center; width: 10.2853%;">14.9 USD</td>
</tr>
<tr>
<td style="text-align: left; width: 5.85586%;">thawte</td>
<td style="text-align: left; width: 14.3393%;">SSL 123</td>
<td style="text-align: center; width: 19.2192%;">THAWTE_SSL123</td>
<td style="text-align: center; width: 9.38438%;">Domain Validation (DV)</td>
<td style="text-align: center; width: 8.78378%;">-</td>
<td style="text-align: center; width: 14.5646%;">EMAIL,DNSZONE,URL</td>
<td style="text-align: center; width: 5.63063%;">Basic</td>
<td style="text-align: center; width: 5.63063%;">No</td>
<td style="text-align: center; width: 6.23123%;">No</td>
<td style="text-align: center; width: 10.2853%;">44.9 USD</td>
</tr>
</tbody>
</table>

</div>
