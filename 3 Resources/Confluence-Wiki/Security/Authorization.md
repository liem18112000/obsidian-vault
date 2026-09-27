---
title: "Authorization"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20530435691/Authorization
space: "LUZ"
topic: security
relevance: 0.75
depth: 2.7
updated: 2021-05-18
attachments: 10
tags:
  - confluence
  - security
  - space/luz
---

# Authorization

> [!info] Imported from Confluence
> Space **LUZ** · updated 2021-05-18 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20530435691/Authorization)
> Relevance 0.75 · topic `security`

### 1. Enable JWT Authentication method 

![[20530435691-image2021-5-17_10-5-42.png]]



<div>

|                         |
|:------------------------|
| `vault auth enable jwt` |

</div>

### 2. Get JWT Mount Accessor 

![[20530435691-image2021-5-17_10-7-56.png]]



<div>

|                   |
|:------------------|
| `vault auth list` |

</div>

### 3. Create Policy for authorization (key/tenant)

Following Vault document: <a href="https://www.vaultproject.io/docs/concepts/policies" class="external-link" rel="nofollow">https://www.vaultproject.io/docs/concepts/policies</a>  
Create a policy with variables to authorize with a key per tenant

### 

![[20530435691-image2021-5-17_10-11-1.png]]

 

![[20530435691-image2021-5-17_10-10-8.png]]



<div>

<table style="text-align: left;">
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody style="text-align: left;">
<tr style="text-align: left;">
<td style="text-align: left;"><p>path "transit/keys/{{identity.entity.aliases.auth_jwt_bb8d482a.metadata.tenantId}}" {<br />
    capabilities = [ "create", "read", "update", "delete", "list" ]<br />
}</p>
<p>path "transit/datakey/plaintext/{{identity.entity.aliases.auth_jwt_bb8d482a.metadata.tenantId}}" {<br />
    capabilities = [ "create", "update" ]<br />
}</p>
<p>path "transit/encrypt/{{identity.entity.aliases.auth_jwt_bb8d482a.metadata.tenantId}}" {<br />
    capabilities = [ "update" ]<br />
}<br />
path "transit/decrypt/{{identity.entity.aliases.auth_jwt_bb8d482a.metadata.tenantId}}" {<br />
    capabilities = [ "update" ]<br />
}</p></td>
</tr>
</tbody>
</table>

</div>

### 4. Create a role (test-role) to validate for token

  
**request create role**

<div>

<table style="text-align: left;">
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody style="text-align: left;">
<tr style="text-align: left;">
<td style="text-align: left;"><p><code class="sourceCode yaml" style="text-align: left;"><span class="at">curl --location --request POST</span></code><span><code class="sourceCode yaml" style="text-align: left;"><span class="at"> </span></code></span><code class="sourceCode yaml" style="text-align: left;"><span class="st">&#39;</span></code><span rel="nofollow" style="text-decoration: none;text-align: left;"><code class="sourceCode yaml" style="text-align: left;"><span class="at">http://127.0.0.1:8200/v1/auth/jwt/role/test-role&#39;</span></code></span><span> </span><code class="sourceCode yaml" style="text-align: left;"><span class="at">\</span></code><br />
<code class="sourceCode yaml" style="text-align: left;"><span class="at">--header</span></code><span><code class="sourceCode yaml" style="text-align: left;"><span class="at"> </span></code></span><code class="sourceCode yaml" style="text-align: left;"><span class="st">&#39;X-Vault-Token: s.vZzxBXzKh6oFMBwc5Ifx5Y4x&#39;</span></code><span> </span><code class="sourceCode yaml" style="text-align: left;"><span class="at">\</span></code><br />
<code class="sourceCode yaml" style="text-align: left;"><span class="at">--header</span></code><span><code class="sourceCode yaml" style="text-align: left;"><span class="at"> </span></code></span><code class="sourceCode yaml" style="text-align: left;"><span class="st">&#39;Content-Type: application/json&#39;</span></code><span> </span><code class="sourceCode yaml" style="text-align: left;"><span class="at">\</span></code><br />
<code class="sourceCode yaml" style="text-align: left;"><span class="at">--data-raw &#39;</span></code><code class="sourceCode yaml" style="text-align: left;"><span class="kw">{</span></code><br />
<code class="sourceCode yaml" style="text-align: left;"><span class="at">  </span></code><code class="sourceCode yaml" style="text-align: left;"><span class="st">&quot;policies&quot;</span></code><code class="sourceCode yaml" style="text-align: left;"><span class="kw">:</span></code><span> </span><code class="sourceCode yaml" style="text-align: left;"><span class="kw">[</span></code><code class="sourceCode yaml" style="text-align: left;"><span class="st">&quot;transit-policy&quot;</span></code><code class="sourceCode yaml" style="text-align: left;"><span class="at">]</span></code><code class="sourceCode yaml" style="text-align: left;"><span class="at">,</span></code><br />
<code class="sourceCode yaml" style="text-align: left;"><span class="at">  </span></code><code class="sourceCode yaml" style="text-align: left;"><span class="st">&quot;user_claim&quot;</span></code><code class="sourceCode yaml" style="text-align: left;"><span class="kw">:</span></code><span> </span><code class="sourceCode yaml" style="text-align: left;"><span class="st">&quot;sub&quot;</span></code><code class="sourceCode yaml" style="text-align: left;"><span class="at">,</span></code><br />
<code class="sourceCode yaml" style="text-align: left;"><span class="at">  </span></code><code class="sourceCode yaml" style="text-align: left;"><span class="st">&quot;bound_claims&quot;</span></code><code class="sourceCode yaml" style="text-align: left;"><span class="kw">:</span></code><span> </span><code class="sourceCode yaml" style="text-align: left;"><span class="kw">{</span></code><br />
<code class="sourceCode yaml" style="text-align: left;"><span class="at">    </span></code><code class="sourceCode yaml" style="text-align: left;"><span class="st">&quot;iss&quot;</span></code><code class="sourceCode yaml" style="text-align: left;"><span class="kw">:</span></code><span> </span><code class="sourceCode yaml" style="text-align: left;"><span class="st">&quot;com.axonivy&quot;</span></code><br />
<code class="sourceCode yaml" style="text-align: left;"><span class="at">  </span></code><code class="sourceCode yaml" style="text-align: left;"><span class="at">},</span></code><br />
<code class="sourceCode yaml" style="text-align: left;"><span class="at">  </span><span class="fu">&quot;claim_mapping&quot;</span><span class="kw">:</span><span class="at"> </span><span class="kw">{</span></code><br />
<code class="sourceCode yaml" style="text-align: left;"><span class="at">    </span><span class="fu">&quot;indiviual-tenant/tenantId&quot;</span><span class="kw">:</span><span class="at"> </span><span class="st">&quot;tenantId&quot;</span></code><br />
<span style="font-family: SFMono-Medium , &quot;SF Mono&quot; , &quot;Segoe UI Mono&quot; , &quot;Roboto Mono&quot; , &quot;Ubuntu Mono&quot; , Menlo , Courier , monospace;letter-spacing: 0.0px;">  },<br />
</span><code class="sourceCode yaml" style="text-align: left;"><span class="at">  </span></code><code class="sourceCode yaml" style="text-align: left;"><span class="st">&quot;role_type&quot;</span></code><code class="sourceCode yaml" style="text-align: left;"><span class="kw">:</span></code><span> </span><code class="sourceCode yaml" style="text-align: left;"><span class="st">&quot;jwt&quot;</span></code><br />
<code class="sourceCode yaml" style="text-align: left;"><span class="at">}</span></code><code class="sourceCode yaml" style="text-align: left;"><span class="st">&#39;</span></code></p></td>
</tr>
</tbody>
</table>

</div>


![[20530435691-image2021-5-17_10-16-8.png]]



### 5. Login

<div>

|  |
|:---|
| <a href="http://127.0.0.1:8200/v1/auth/jwt/login" class="external-link" rel="nofollow"><code class="sourceCode yaml" style="text-align: left;"><span class="at">http://127.0.0.1:8200/v1/auth/jwt/login</span></code></a> |

</div>

### 

![[20530435691-image2021-5-17_10-28-42.png]]

 

![[20530435691-image2021-5-17_10-30-9.png]]



**Perform encrypt on own tenant key: *098cf703-97d6-4b68-975c-29586f90cdc8  ***

<div>

|  |
|:---|
| <a href="http://127.0.0.1:8200/v1/transit/encrypt/098cf703-97d6-4b68-975c-29586f90cdc8" class="external-link" rel="nofollow"><code class="sourceCode yaml" style="text-align: left;"><span class="at">http://127.0.0.1:8200/v1/transit/encrypt/098cf703-97d6-4b68-975c-29586f90cdc8</span></code></a> |

</div>

  
  

![[20530435691-image2021-5-17_10-32-11.png]]

  
**Perform encrypt on another tenant key: *eea8b5a1-c93c-458e-8917-27c75167d626  ***

***

![[20530435691-image2021-5-17_10-37-45.png]]

***
