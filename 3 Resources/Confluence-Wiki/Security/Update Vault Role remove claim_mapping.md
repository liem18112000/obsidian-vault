---
ai_hash: 4c01964fdfac8331
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 1
depth: 2.65
entities: []
relevance: 0.711
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20530454244/Update+Vault+Role+remove+claim_mapping
space: LUZ
status: reference
tags:
- confluence
- security
- space/luz
title: Update Vault Role remove claim_mapping
topic: security
type: source
updated: 2021-07-02
---

# Update Vault Role remove claim_mapping

> [!info] Imported from Confluence
> Space **LUZ** · updated 2021-07-02 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20530454244/Update+Vault+Role+remove+claim_mapping)
> Relevance 0.711 · topic `security`

Previously, when we have issue with concurrent request to Vault because we define the Vault Role like bellow:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="f3e53926-905f-4271-9d7c-803d2241639d" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
{
    "policies": ["transit-policy"],
    "user_claim": "sub",
    "bound_claims": {"iss": "com.axonivy"},
    "role_type": "jwt",
    "token_period": "0",
    "claim_mappings": {"/individual-tenant/tenantId": "individual_tenantId","/company-tenant/tenantId": "company_tenantId"}
}
```

</div>

</div>

  

Then we know the issue came from "user_claim": "sub" and we change it to tenantId and no longer using "claim_mappings"

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="8aeed530-0259-4ffa-b7e0-13d2d7ebd3f2" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
{
    "policies": ["transit-policy"],
    "user_claim": "tenantId",
    "bound_claims": {"iss": "com.axonivy"},
    "role_type": "jwt",
    "token_period": "0"
}
```

</div>

</div>

But following this confluence page: [Amend Vault Role and Policy](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20530449167/Amend+Vault+Role+and+Policy), we using update role api, and this api just update by key:value so the "claim_mappings" is still in the role definition.  
It will lead to the log when user login, if Individual tenant user login then Vault could not found the company-tenant-id and opposite.


![[20530454244-image2021-7-2_19-12-11.png]]

  
So we need to remove this "claim_mappings" from the Role because we not using this anymore.  
  

**Following this to update the Vault klara-tenant-role  
Important**: We need Vault **root-token** to update.  
Port-forward luz_vault and run this to update klara-tenant-role (please correct the root token)

<div>

<table style="text-align: left;">
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody style="text-align: left;">
<tr style="text-align: left;">
<td style="text-align: left;"><p><code class="sourceCode java" style="text-align: left;">curl <span class="op">--</span>location <span class="op">--</span>request POST</code><code class="sourceCode java" style="text-align: left;"><span class="er">&#39;</span></code><span rel="nofollow" style="text-decoration: none;text-align: left;"><code class="sourceCode java" style="text-align: left;">http<span class="op">:</span><span class="co">//127.0.0.1:8200/v1/auth/jwt/role/klara-tenant-role&#39;</span></code></span><code class="sourceCode java" style="text-align: left;">\</code><br />
<code class="sourceCode java" style="text-align: left;"><span class="op">--</span>header</code><code class="sourceCode java" style="text-align: left;"><span class="er">&#39;</span>X<span class="op">-</span>Vault<span class="op">-</span>Token<span class="op">:</span> <span class="op">{</span>rootToken<span class="op">}</span><span class="er">&#39;</span></code><code class="sourceCode java" style="text-align: left;">\</code><br />
<code class="sourceCode java" style="text-align: left;"><span class="op">--</span>header</code><code class="sourceCode java" style="text-align: left;"><span class="er">&#39;</span>Content<span class="op">-</span><span class="bu">Type</span><span class="op">:</span> application<span class="op">/</span>json<span class="er">&#39;</span></code><code class="sourceCode java" style="text-align: left;">\</code><br />
<code class="sourceCode java" style="text-align: left;"><span class="op">--</span>data<span class="op">-</span>raw</code><code class="sourceCode java" style="text-align: left;"><span class="er">&#39;</span><span class="op">{</span><span class="st">&quot;policies&quot;</span><span class="op">:</span> <span class="op">[</span><span class="st">&quot;transit-policy&quot;</span><span class="op">],</span><span class="st">&quot;user_claim&quot;</span><span class="op">:</span> <span class="st">&quot;tenantId&quot;</span><span class="op">,</span><span class="st">&quot;bound_claims&quot;</span><span class="op">:</span> <span class="op">{</span><span class="st">&quot;iss&quot;</span><span class="op">:</span> <span class="st">&quot;com.axonivy&quot;</span><span class="op">},</span><span class="st">&quot;role_type&quot;</span><span class="op">:</span> <span class="st">&quot;jwt&quot;</span><span class="op">,</span><span class="st">&quot;token_period&quot;</span><span class="op">:</span> <span class="st">&quot;0&quot;</span><span class="op">,</span> <span class="st">&quot;claim_mappings&quot;</span><span class="op">:</span> <span class="kw">null</span><span class="op">}</span><span class="er">&#39;</span></code></p></td>
</tr>
</tbody>
</table>

</div>

%% ai-graph-start %%

**Related notes:**
- [[Authorization]]
- [[Tenant token issue]]
- [[Get tenant token from public api]]
- [[Vault overview]]
- [[Update Vault Unseal self-signed certificate]]

%% ai-graph-end %%