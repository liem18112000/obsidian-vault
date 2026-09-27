---
ai_hash: ce8360dcbd1a59ee
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 0
depth: 2.89
entities: []
relevance: 0.716
source: https://axonivy.atlassian.net/wiki/spaces/Helios/pages/38174377560/Getting+tenant+list
space: Helios
status: reference
tags:
- confluence
- programming
- space/helios
title: Getting tenant list
topic: programming
type: source
updated: 2018-07-24
---

# Getting tenant list

> [!info] Imported from Confluence
> Space **Helios** · updated 2018-07-24 · [open original](https://axonivy.atlassian.net/wiki/spaces/Helios/pages/38174377560/Getting+tenant+list)
> Relevance 0.716 · topic `programming`

Get a user's list of tenants.

### **I. Endpoint **

{protocol}://{ip}:{port}/{applicationName}/api/tenants

Example: <span class="legacy-color-text-blue4">http://192.168.72.90:8080/luz_mobile/api/tenants</span>

### **II. Method**

POST

### **III. Request**

Header

<div>

| Attribute name | Attribute value | Description |
|----|----|----|
| Authorization | Bearer eyJhb0lD4IATGWUlfH...EcWF0HOyvXsAKUXjQNo0-BpueGkysOl9ncmd5cfsDfJa6FJOzOzyK3DINIpGXA2ug7jW-MGxQ | <span class="legacy-color-text-default">authorization token here is the retained refresh_token in the authentication step </span> |
| App-Version | 0.0.1 | the current version of myKLARA app |

</div>

###  **IV.Response**

Successful case 

<div>

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr>
<td><p><code class="text plain">{</code><br />
<code class="text spaces">    </code><code class="text plain">"status": "success",</code><br />
<code class="text spaces">    </code><code class="text plain">"message": null,</code><br />
<code class="text spaces">    </code><code class="text plain">"tenants": [</code><br />
<code class="text spaces">        </code><code class="text plain">{</code><br />
<code class="text spaces">            </code><code class="text plain">"id": 1665,</code><br />
<code class="text spaces">            </code><code class="text plain">"roles": [</code><br />
<code class="text spaces">                </code><code class="text plain">"tenant-creator"</code><br />
<code class="text spaces">            </code><code class="text plain">],</code><br />
<code class="text spaces">            </code><code class="text plain">"tenantId": "8815e317-f12f-4d5b-93e1-e0bc27f233ab",</code><br />
<code class="text spaces">            </code><code class="text plain">"username": "hcmc-helios@</code><a href="https://axonivy.atlassian.net/wiki/display/Helios/axonactive.com" rel="nofollow"><code class="text plain">axonactive.com</code></a><code class="text plain">",</code><br />
<code class="text spaces">            </code><code class="text plain">"type": "COMPANY",</code><br />
<code class="text spaces">            </code><code class="text plain">"companyInfoId": 1319,</code><br />
<code class="text spaces">            </code><code class="text plain">"companyId": 1,</code><br />
<code class="text spaces">            </code><code class="text plain">"companyName": "Helios Aviation AG",</code><br />
<code class="text spaces">            </code><code class="text plain">"address": {</code><br />
<code class="text spaces">                </code><code class="text plain">"validFrom": null,</code><br />
<code class="text spaces">                </code><code class="text plain">"validTo": null,</code><br />
<code class="text spaces">                </code><code class="text plain">"id": "1",</code><br />
<code class="text spaces">                </code><code class="text plain">"addressType": "PRIVATE",</code><br />
<code class="text spaces">                </code><code class="text plain">"addressLines": "Industriestrasse 17",</code><br />
<code class="text spaces">                </code><code class="text plain">"additionalAddress": "",</code><br />
<code class="text spaces">                </code><code class="text plain">"canton": "BE",</code><br />
<code class="text spaces">                </code><code class="text plain">"city": {</code><br />
<code class="text spaces">                    </code><code class="text plain">"cityName18": "Trachselwald",</code><br />
<code class="text spaces">                    </code><code class="text plain">"cityName27": "Trachselwald"</code><br />
<code class="text spaces">                </code><code class="text plain">}</code><br />
<code class="text spaces">            </code><code class="text plain">},</code><br />
<code class="text spaces">            </code><code class="text plain">"person": null</code><br />
<code class="text spaces">        </code><code class="text plain">},</code><br />
<code class="text spaces">        </code><code class="text plain">...</code><br />
<code class="text spaces">    </code><code class="text plain">],</code><br />
<code class="text spaces">    </code><code class="text plain">"refreshToken": "</code><a href="https://axonivy.atlassian.net/wiki/display/Helios/eyJhbGciOiJSUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICJPVkMyV29PVjdfMGJkR1lqSFJYb1FZVE0yekhNOHlJQVh2YWY2dEhnQXpZIn0.eyJqdGkiOiI2Njc0ZjhkMC1hNWNhLTRmNTctOGZmOS1lODMyNzY4NDczYTYiLCJleHAiOjAsIm5iZiI6MCwiaWF0IjoxNTMxMTI4NTk2LCJpc3MiOiJodHRwczovL2xvZ2luLmtsYXJhLmNoL2F1dGgvcmVhbG1zL2tsYXJhIiwiYXVkIjoia2xhcmEtbW9iaWxlIiwic3ViIjoiYzEwNDU0MDktMzEzNS00MGRiLWJhZDYtM2M5ZGJiODgzNjJkIiwidHlwIjoiT2ZmbGluZSIsImF6cCI6ImtsYXJhLW1vYmlsZSIsImF1dGhfdGltZSI6MCwic2Vzc2lvbl9zdGF0ZSI6IjljMzRjOWU2LTkwZDAtNGQ2MS1iZDMzLTMxYmY1ZWMyMDIzMCIsImNsaWVudF9zZXNzaW9uIjoiNjk0NjdiODUtYzkyMi00OTZlLTk1NmUtMjNmNGZlYWJjMDA3IiwicmVhbG1fYWNjZXNzIjp7InJvbGVzIjpbIm9mZmxpbmVfYWNjZXNzIiwidW1hX2F1dGhvcml6YXRpb24iXX0sInJlc291cmNlX2FjY2VzcyI6eyJhY2NvdW50Ijp7InJvbGVzIjpbIm1hbmFnZS1hY2NvdW50Iiwidmlldy1wcm9maWxlIl19fX0.hUFBFEOUJxyQi7_WCjlYrTBZcXPR1GItrJtIVXzbqgqJJhtfAVFwmEd6vVksrcbROphDnjpQVwYvZyHS7UDs75VYmsuBC5mzxFyPUHU2YByoizGzD2F-ofDj2TW6tMqImGsRF69ZBWnqPPq91rggT8SGCyySUJr1ccxjMsR1gNfNWv7O9mO1jvRspdgw-jwqcgF_vqFJYp2avB_vwp17-dnnVNLfnJdtu8lb_wCRD1fKnk8qMki-GS_4VhHTwDJm5aS5v-Ikf-gW0oPSgALFsucQIPUIrqVKbbwNTdLQtdkX8UXiOTPqQTBDxmO6YfdDm1zpqUBoxolXcvxRXQ_Qcw" rel="nofollow"><code class="text plain">eyJhbGciOiJSUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICJPVkMyV29PVjdfMGJkR1lqSFJYb1FZVE0yekhNOHlJQVh2YWY2dEhnQXpZIn0.eyJqdGkiOiI2Njc0ZjhkMC1hNWNhLTRmNTctOGZmOS1lODMyNzY4NDczYTYiLCJleHAiOjAsIm5iZiI6MCwiaWF0IjoxNTMxMTI4NTk2LCJpc3MiOiJodHRwczovL2xvZ2luLmtsYXJhLmNoL2F1dGgvcmVhbG1zL2tsYXJhIiwiYXVkIjoia2xhcmEtbW9iaWxlIiwic3ViIjoiYzEwNDU0MDktMzEzNS00MGRiLWJhZDYtM2M5ZGJiODgzNjJkIiwidHlwIjoiT2ZmbGluZSIsImF6cCI6ImtsYXJhLW1vYmlsZSIsImF1dGhfdGltZSI6MCwic2Vzc2lvbl9zdGF0ZSI6IjljMzRjOWU2LTkwZDAtNGQ2MS1iZDMzLTMxYmY1ZWMyMDIzMCIsImNsaWVudF9zZXNzaW9uIjoiNjk0NjdiODUtYzkyMi00OTZlLTk1NmUtMjNmNGZlYWJjMDA3IiwicmVhbG1fYWNjZXNzIjp7InJvbGVzIjpbIm9mZmxpbmVfYWNjZXNzIiwidW1hX2F1dGhvcml6YXRpb24iXX0sInJlc291cmNlX2FjY2VzcyI6eyJhY2NvdW50Ijp7InJvbGVzIjpbIm1hbmFnZS1hY2NvdW50Iiwidmlldy1wcm9maWxlIl19fX0.hUFBFEOUJxyQi7_WCjlYrTBZcXPR1GItrJtIVXzbqgqJJhtfAVFwmEd6vVksrcbROphDnjpQVwYvZyHS7UDs75VYmsuBC5mzxFyPUHU2YByoizGzD2F-ofDj2TW6tMqImGsRF69ZBWnqPPq91rggT8SGCyySUJr1ccxjMsR1gNfNWv7O9mO1jvRspdgw-jwqcgF_vqFJYp2avB_vwp17-dnnVNLfnJdtu8lb_wCRD1fKnk8qMki-GS_4VhHTwDJm5aS5v-Ikf-gW0oPSgALFsucQIPUIrqVKbbwNTdLQtdkX8UXiOTPqQTBDxmO6YfdDm1zpqUBoxolXcvxRXQ_Qcw</code></a><code class="text plain">"</code><br />
<code class="text plain">}</code></p></td>
</tr>
</tbody>
</table>

</div>

#### Error cases

<div>

| Code | What went wrong |
|----|----|
| GONE (410) | The generated refresh_token is not valid or expired. |
| INTERNAL SERVER ERROR (500) | An unexpected exception is happen on server |

</div>

  
Example

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="70d7f544-5e55-4a96-9f42-00f1f02eff8e" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
{
    "status": "fail",
    "message": "The refreshtoken could not be validated at jwt side : Gone"
}
```

</div>

</div>

%% ai-graph-start %%

**Related notes:**
- [[Login]]
- [[LUZ-109076 Public API - Create update new tenant (implementation)]]
- [[How to use Public API to create update KLARA Business Company]]
- [[Uploading documents]]
- [[Token JWT Security]]

%% ai-graph-end %%