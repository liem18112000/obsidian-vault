---
ai_hash: faf0f05047e604df
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 10
depth: 2.73
entities: []
relevance: 0.731
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20482985244/luz_cor_api
space: LUZ
status: reference
tags:
- confluence
- programming
- space/luz
title: luz_cor_api
topic: programming
type: source
updated: 2019-06-07
---

# luz_cor_api

> [!info] Imported from Confluence
> Space **LUZ** · updated 2019-06-07 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20482985244/luz_cor_api)
> Relevance 0.731 · topic `programming`

# Oauth flow after consent setting

  


![[20482985244-OAuth Flow.png]]



  

### REST Services

<div>

<table style="width: 99.9444%;">
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
<th>Class</th>
<th>Method</th>
<th>Path</th>
<th>In</th>
<th>Out</th>
<th>Status</th>
<th>Description</th>
</tr>
&#10;<tr>
<td>CorTokenResource<br />
{tenant-id}/companies/{company-id}/oauth</td>
<td><br />
</td>
<td><br />
</td>
<td><br />
</td>
<td><br />
</td>
<td><br />
</td>
<td><br />
</td>
</tr>
<tr>
<td><br />
</td>
<td>GET</td>
<td>/ping</td>
<td><br />
</td>
<td>SixTokenResource + current date</td>
<td>OK</td>
<td>Ping method to check if resource is available</td>
</tr>
<tr>
<td><br />
</td>
<td>GET</td>
<td>/token</td>
<td><p>bankId (QueryParam)</p>
<p>authCode (QueryParam)</p></td>
<td><p>CorToken</p>
<p>    Token token</p>
<p>    List&lt;String&gt; ibans</p>
<p>    long lastRefresh</p>
<p><br />
</p>
<p><br />
</p></td>
<td><p>OK</p>
<p>BAD_REQUEST</p>
<p>SEE_OTHER (redirect)</p></td>
<td><p>Retrieve token from token service either via auth code or refresh token.</p>
<p>Access token is valid for 24h, therefore it makes sense to call this service before calling GetAccounts or GetTransactions</p></td>
</tr>
<tr>
<td><p>CorBaseResource</p>
<p>{tenant-id}/companies/{company-id}/base</p></td>
<td><br />
</td>
<td><br />
</td>
<td><br />
</td>
<td><br />
</td>
<td><br />
</td>
<td><br />
</td>
</tr>
<tr>
<td><br />
</td>
<td>GET</td>
<td>/ping</td>
<td><br />
</td>
<td>SixTokenResource + current date</td>
<td>OK</td>
<td>Ping method to check if resource is available</td>
</tr>
<tr>
<td><br />
</td>
<td>GET</td>
<td>/redirect-data/{bank-id}</td>
<td>bank-id</td>
<td><p>CorRedirectData</p>
<p>    String clientId</p>
<p>    String clientRedirectionEndpointUrl</p>
<p>    String providerAuthEndpointUri</p></td>
<td><p>OK</p>
<p>BAD_REQUEST</p></td>
<td>Retrieve data for OAuth flow (redirection)</td>
</tr>
<tr>
<td><p>CorAisResource</p>
<p>{tenant-id}/companies/{company-id}/ais</p></td>
<td><br />
</td>
<td><br />
</td>
<td><br />
</td>
<td><br />
</td>
<td><br />
</td>
<td><br />
</td>
</tr>
<tr>
<td><br />
</td>
<td>GET</td>
<td>/ping</td>
<td><br />
</td>
<td><br />
</td>
<td><br />
</td>
<td>Ping method to check if resource is available</td>
</tr>
<tr>
<td><br />
</td>
<td>GET</td>
<td>/accounts/{bank-id}</td>
<td>bank-id</td>
<td><p>AccountsHolder</p>
<p>    List&lt;AccountListElem&gt; accountList</p></td>
<td><p>OK</p>
<p>BAD_REQUEST</p></td>
<td>Retrieve accounts for the bank id</td>
</tr>
<tr>
<td><br />
</td>
<td>GET</td>
<td>/transactions/{iban}</td>
<td>iban</td>
<td><p>TransactionListElem</p>
<p>    String iban;</p>
<p>    String designation;</p>
<p>    List&lt;Entry&gt; entries</p>
<p>    Links links;</p></td>
<td><p>OK</p>
<p>BAD_REQUEST</p></td>
<td>Retrieve transactions for a specific iban</td>
</tr>
<tr>
<td><br />
</td>
<td>POST</td>
<td>/update-consent/{bank-id}</td>
<td>bank-id</td>
<td><br />
</td>
<td><p>OK</p>
<p>BAD_REQUEST</p></td>
<td>Update consent data (see below in "Consent")</td>
</tr>
</tbody>
</table>

</div>

  

# Exception Handling


![[20482985244-Exception handling regarding token refresh and expiration.png]]



  

  

## Consent

  


![[20482985244-image2019-4-16_17-45-57.png]]



  

## Token


![[20482985244-image2019-4-16_17-46-46.png]]



  

## lastDate of synchronisation (for each IBAN)


![[20482985244-image2019-6-7_9-29-21.png]]



  

## Configuration luz_cor_api (**tbd**)

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
<th>Param</th>
<th>Property key</th>
<th>Location</th>
<th>Value Test Env</th>
<th>Description</th>
</tr>
&#10;<tr>
<td>PATH_KEYSTORE</td>
<td>ch.klara.bank.corapi.path_keystore</td>
<td>Wildfly Sys Props</td>
<td>keystore.jks</td>
<td>Absolut path to the keystore</td>
</tr>
<tr>
<td>JKS_PWD</td>
<td>ch.klara.bank.corapi.jks_pw</td>
<td>Wildfly Sys Props</td>
<td>klara123!</td>
<td>Keystore password</td>
</tr>
<tr>
<td>KEY_PWD</td>
<td>ch.klara.bank.corapi.key_pw</td>
<td>Wildfly Sys Props</td>
<td>klara123!</td>
<td>Certificate password</td>
</tr>
<tr>
<td>COR_API_DOMAIN</td>
<td>ch.klara.bank.corapi.cor_api_domain</td>
<td>Wildfly Sys Props</td>
<td><a href="https://api-cert-etu.six-group.com" class="external-link" rel="nofollow">https://api-cert-etu.six-group.com</a></td>
<td>Depending on the environment Test=etu, Prod=?)</td>
</tr>
<tr>
<td>CLIENT_NAME</td>
<td>ch.klara.bank.corapi.client_name</td>
<td>Wildfly Sys Props</td>
<td>KLARA BUSINESS AG</td>
<td>Used to load directory for Klara</td>
</tr>
<tr>
<td><br />
</td>
<td><br />
</td>
<td><br />
</td>
<td><br />
</td>
<td><br />
</td>
</tr>
<tr>
<td><br />
</td>
<td><br />
</td>
<td><br />
</td>
<td><br />
</td>
<td><br />
</td>
</tr>
</tbody>
</table>

</div>

  

====================================================================================================

  

Test cases

<div>

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<tbody>
<tr>
<th>Case</th>
<th>Description</th>
<th><br />
</th>
</tr>
&#10;<tr>
<td>Set consent</td>
<td><ul>
<li>set token for the specific bank_id</li>
<li>set token for the accounts</li>
</ul></td>
<td><br />
</td>
</tr>
<tr>
<td>Get Accounts</td>
<td><ul>
<li>return accounts for a bank_id</li>
</ul></td>
<td><br />
</td>
</tr>
<tr>
<td>Get Transactions</td>
<td><ul>
<li>return transactions for a specific IBAN</li>
</ul></td>
<td><br />
</td>
</tr>
<tr>
<td>Check Token</td>
<td><ul>
<li>Token expired due to lastRefresh</li>
<li>Token refresh after 24h</li>
<li>Token refresh with refresh token<br />
<br />
</li>
</ul></td>
<td><br />
</td>
</tr>
</tbody>
</table>

</div>

  

====================================================================================================

  

  

# Certificates (**tbd**)

Certs created with OpenSSL:

<a href="http://api.dev.klara.ch" class="external-link" rel="nofollow">api.dev.klara.ch</a>.key

linux_cert+ca.pem

  

Keystore


![[20482985244-image2019-1-21_17-44-3.png]]




![[20482985244-image2019-1-21_17-45-24.png]]




![[20482985244-image2019-1-21_17-45-42.png]]



  

Truststore : We can use Java Truststore

  

  

  

Ok:

curl -v -H "X-CorAPI-Target-ID:IIDX99999" -H "X-Correlation-ID: 123" -H "X-PSU-User-Agent: 123" -H "X-PSU-IP-Address: 127.0.0.1" -X POST --key <a href="http://api.dev.klara.ch" class="external-link" rel="nofollow">api.dev.klara.ch</a>.key --cert linux_cert+ca.pem -d 'client_id=CIDX0000000002&grant_type=authorization_code&code=otwcNj&redirect_uri=<a href="http://localhost:8080/CorAPI/CorAPITestServlet" class="external-link" rel="nofollow">http://localhost:8080/CorAPI/CorAPITestServlet</a>' <a href="https://api-cert-etu.six-group.com/api/bankingservices/corporate/v1/oauth/token" class="external-link" rel="nofollow">https://api-cert-etu.six-group.com/api/bankingservices/corporate/v1/oauth/token</a>

%% ai-graph-start %%

**Related notes:**
- [[Implement Corporate API Access (LUZ-17959)]]
- [[Token JWT Security]]
- [[Getting tenant list]]
- [[How to run export API for specific tenant and date - Manual export]]
- [[Swagger with api explorer]]

%% ai-graph-end %%