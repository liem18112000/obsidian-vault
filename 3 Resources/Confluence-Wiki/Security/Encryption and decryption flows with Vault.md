---
title: "Encryption and decryption flows with Vault"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20525655052/Encryption+and+decryption+flows+with+Vault
space: "LUZ"
topic: security
relevance: 0.921
depth: 3
updated: 2021-03-12
attachments: 48
tags:
  - confluence
  - security
  - space/luz
---

# Encryption and decryption flows with Vault

> [!info] Imported from Confluence
> Space **LUZ** · updated 2021-03-12 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20525655052/Encryption+and+decryption+flows+with+Vault)
> Relevance 0.921 · topic `security`

<div class="toc-macro client-side-toc-macro conf-macro output-block" hasbody="false" headerelements="H1,H2,H3,H4,H5,H6,H7" macro-id="b78ddda2-13a7-4f63-9b12-d0382d689258" macro-name="toc">

</div>

# <span class="inline-comment-marker" ref="26202e80-550d-44d9-a0ae-c45c7baf96ae">Sequence diagram</span>


![[20525655052-Encrypt and Decrypt files.png]]



# Upload flow

  

<div>

<table style="width: 97.6254%;">
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<tbody>
<tr>
<th>Step</th>
<th>Description</th>
<th>Explaination/Notes</th>
</tr>
&#10;<tr>
<td>1</td>
<td>Upload file from luz_docs</td>
<td><br />
</td>
</tr>
<tr>
<td>2-3</td>
<td>Authen/Author in MinIO, receive success signal</td>
<td><br />
</td>
</tr>
<tr>
<td>4-5</td>
<td>Authen/Author in Vault from token/policy</td>
<td>return the client token with attached policy </td>
</tr>
<tr>
<td>6-7-8</td>
<td>luz_docs call datakey to get DEK and eDEK</td>
<td><div class="content-wrapper">
<p>Vault use engine at: transit/datakey/plaintext/&lt;key_ring&gt; to create DEK (plantext) and eDEK(cipher)<br />

![[20525655052-image2021-3-12_17-7-19.png]]


</div>
<p>plaintext: DEK,<br />
ciphertext: eDEK</p></td>
</tr>
<tr>
<td>9</td>
<td>luz_docs use DEK to encrypt Data  -&gt; eData</td>
<td><br />
</td>
</tr>
<tr>
<td>10</td>
<td>Create encrypted filename, Build metadata which includes eData and real file path</td>
<td><div class="content-wrapper">
<p>suggest use AES algorithm, the eData will be created with name is encrypted by an UUID </p>
<p>FileName: a random UUID</p>
<p>metadata:</p>
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="71c67d52-a051-4788-b2f6-8da16687c939" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>
{
   x-amz-meta-edex: edex string
   x-amz-meta-filepath: bucketName/realFileName
}</code></pre>
</div>
</div>
</div></td>
</tr>
<tr>
<td>11</td>
<td>luz_docs call MinIO to save eData and the metadata</td>
<td><span class="legacy-color-text-blue3">Checking if Metadata Support is given from Min.IO</span></td>
</tr>
<tr>
<td>12</td>
<td>MinIO return signal to luz_docs</td>
<td><br />
</td>
</tr>
<tr>
<td>13</td>
<td>luz_docs return upload status, response to client</td>
<td><br />
</td>
</tr>
</tbody>
</table>

</div>

# Download flow

<div>

<table style="width: 97.8943%;">
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<tbody>
<tr>
<th>Step</th>
<th>Description</th>
<th>Explaination/Notes</th>
</tr>
&#10;<tr>
<td>1</td>
<td>Send download file from luz_docs</td>
<td><br />
</td>
</tr>
<tr>
<td>2-3</td>
<td>Authen/Author in MinIO, receive success signal</td>
<td>Similar as upload flow</td>
</tr>
<tr>
<td>4 -5</td>
<td>luz_docs call MinIO Get eData, metadata</td>
<td>give the real file path, get metadata with include eDEK and eData with name is encrypted</td>
</tr>
<tr>
<td>6</td>
<td>luz_docs exclude metadata to get eDEK</td>
<td><br />
</td>
</tr>
<tr>
<td>7-8</td>
<td>luz_docs Authen/Author with Vault</td>
<td><br />
</td>
</tr>
<tr>
<td>9</td>
<td>luz_docs send eDEK to Vault to get DEK</td>
<td><br />
</td>
</tr>
<tr>
<td>10</td>
<td>Decrypt eDEK to DEK by the KEK which is saved in Vault</td>
<td><div class="content-wrapper">
<p><span>This endpoint decrypts the provided ciphertext using the named key.</span><br />

![[20525655052-image2021-3-8_11-43-38.png]]


</div>
<p><br />

![[20525655052-image2021-3-8_11-43-57.png]]



![[20525655052-image2021-3-8_11-44-8.png]]

<br />
<br />
</p>

![[20525655052-image2021-3-8_11-44-26.png]]

</td>
</tr>
<tr>
<td>11</td>
<td>Vault return DEK to luz_docs</td>
<td><br />
</td>
</tr>
<tr>
<td>12</td>
<td>luz_docs use DEK to decrypt eData to data</td>
<td>suggest use AES algorithm</td>
</tr>
<tr>
<td>13</td>
<td>luz_docs return data to client</td>
<td><br />
</td>
</tr>
</tbody>
</table>

</div>

# Flow to configure key and policy

=== HashiCorp Configuration Language (HCL) ===  
\$ export VAULT_ADDR=<a href="http://127.0.0.1:8200" class="external-link" rel="nofollow">http://127.0.0.1:8200</a> // make sure this plz  
\$ vault server -config=config.hcl -dev // remove -dev in production  
==== Setup transit engine ===

open second terminal  
\$ vault secrets enable transit  
\$ vault secrets list // verify  
\$ vault policy write transit-policy ./transit-policy.hcl  
<span class="confluence-embedded-file-wrapper conf-macro output-inline" hasbody="false" macro-id="bfa49c74-83f0-4498-b6ba-17ff8db2524b" macro-name="view-file"><a href="../_attachments/20525655052-transit-policy (2).hcl" class="confluence-embedded-file" data-nice-type="null" data-file-src="/wiki/download/attachments/20525655052/transit-policy%20(2).hcl?version=1&amp;modificationDate=1615354963000&amp;cacheVersion=1&amp;api=v2" data-mime-type="application/octet-stream" data-has-thumbnail="true">

![[20525655052-transit-policy (2).hcl]]

</a></span>  
\$ vault write -f transit/keys/kepler-key //Now, create an encryption key ring named kepler-key by executing the following command.  
\$ vault token create -policy=transit-policy ==\> save \<client_token\>  
=== Example for encrypt/decrypt  
\$  vault write transit/encrypt/vault-transit plaintext=\$(base64 \<\<\< "4111 1111 1111 1111")  
VAULT_TOKEN= \<client_token\> && vault write transit/encrypt/kepler-key \\  
plaintext=\$(base64 \<\<\< "4111 1111 1111 1111")

# <span class="legacy-color-text-blue3">Manage lifecycle of tenant keys for CSE</span>

<span class="legacy-color-text-blue3">  
     Firstly. we export API for show key's value  
         

![[20525655052-image2021-3-10_17-57-21.png]]

  
</span>

1.  Rotate a tenant's key  
    

![[20525655052-image2021-3-10_17-48-20.png]]

  
      
    For convert cipher text from previous version to the latest one, use unwrap:  
    

![[20525655052-image2021-3-10_17-49-46.png]]

  
    

![[20525655052-image2021-3-10_17-50-15.png]]



2.  <span class="legacy-color-text-blue3"><span class="inline-comment-marker" ref="f7cb02ef-7eaa-4390-aa27-901ceba99316">Disable previous version</span>  
    We update the key's config by:  
    

![[20525655052-image2021-3-10_19-14-18.png]]

</span><span class="legacy-color-text-blue3">  
      
    Forcus on   
    </span>

    <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="dc3ddd71-a8e4-4fa9-b85f-dc3ee50e0946" macro-name="code" style="border-width: 1px;">

    <div class="codeContent panelContent pdl">

    ``` syntaxhighlighter-pre
    "min_decryption_version": 1,
    "min_encryption_version": 0
    ```

    </div>

    </div>

    <span class="legacy-color-text-blue3">  
      
    

![[20525655052-image2021-3-10_19-15-34.png]]

  
      
    We use these params for enable/disable versions of encryption key:  
    </span>

    - <a href="https://www.vaultproject.io/api/secret/transit#min_decryption_version" class="external-link" rel="nofollow" style="text-decoration: none;"><code>min_decryption_version</code></a> `(int: 0)` – Specifies the minimum version of ciphertext allowed to be decrypted. Adjusting this as part of a key rotation policy can prevent old copies of ciphertext from being decrypted, should they fall into the wrong hands. For signatures, this value controls the minimum version of signature that can be verified against. For HMACs, this controls the minimum version of a key allowed to be used as the key for verification.

    - <a href="https://www.vaultproject.io/api/secret/transit#min_encryption_version" class="external-link" rel="nofollow" style="text-decoration: none;"><code>min_encryption_version</code></a> `(int: 0)` – Specifies the minimum version of the key that can be used to encrypt plaintext, sign payloads, or generate HMACs. Must be `0` (which will use the latest version) or a value greater or equal to `min_decryption_version`.

    <span class="legacy-color-text-blue3">  
      
    </span>

3.  Disable all keys of a tenant

4.  Revoke a tenant's key so that all it's data become inaccessible  
    We use the delete api  
    

![[20525655052-image2021-3-10_19-8-35.png]]




![[20525655052-revoke.png]]
