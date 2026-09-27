---
ai_hash: 8f568ddf336185b7
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 0
depth: 3
entities: []
relevance: 0.849
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47771255541/Update+Vault+Unseal+self-signed+certificate.
space: LUZ
status: reference
tags:
- confluence
- security
- space/luz
title: Update Vault Unseal self-signed certificate.
topic: security
type: source
updated: 2024-04-17
---

# Update Vault Unseal self-signed certificate.

> [!info] Imported from Confluence
> Space **LUZ** · updated 2024-04-17 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47771255541/Update+Vault+Unseal+self-signed+certificate.)
> Relevance 0.849 · topic `security`

- Why update Vault Unseal cerfitficate?  
    
  As we aim to upgrade our HashiCorp Vault to the latest version (from 1.7.1 to 1.16.1), it's crucial to note that the new version necessitates certificates to include a Subject Alternative Name (SAN) for authentication. However, our current certificate lacks this requirement.  

- To update new certificate for Vault Unseal, please following these steps below:

1.  **Please follow this document to generate new certificate with SAN**: [Generate Keys and Certificates Files](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20530441322/Generate+Keys+and+Certificates+Files)

2.  **Execute these scripts to generate secrets from the PEM files created in the preceding step**

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="9a4ad937-c733-4190-923e-b389d36d998a" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
./luz-vault-create-unseal-key-secret.sh <env> <place store vault_unseal_key.pem>
./luz-vault-create-unseal-cert-secret.sh <env> <place store vault_unseal_cert.pem>
./luz-vault-create-encryption-key-secret.sh <env> <place store vault_encryption_key.pem>
./luz-vault-create-encryption-cert-secret.sh <env> <place store vault_encryption_cert.pem>
```

</div>

</div>

3.  **Deploy to update those secrets**  
    **NOTE**: After updating those secrets, luz-vault will lose the ability to call luz-vault-unseal. Therefore, the next step is crucial to restore luz-vault's functionality.  

4.  **Execute the following commands to update Vault Unseal certificate**  
    **Note**: 3 unseal keys is stored in luz-vault-keyvalue-unseal-secret  
    Execute this command will return these keys:

    <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="4c5e7404-17d2-40f5-b4f6-da689306f512" macro-name="code" style="border-width: 1px;">

    <div class="codeContent panelContent pdl">

    ``` syntaxhighlighter-pre
    kubectl get secret luz-vault-keyvalue-unseal-secret -o=go-template='{{index .data "unseal.key"}}' -n $NAMESPACE | base64 -d
    ```

    </div>

    </div>

    After got the keys, then following this steps:

<div>

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr>
<td><div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="41cbcc76-a615-4c22-8e96-baae002f0281" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: bash; gutter: false; theme: Confluence" data-theme="Confluence"><code># Log into the vault container ($NAMESPACE is a placeholder)
kubectl exec -ti --namespace $NAMESPACE statefulset/luz-vault-unseal -- /bin/sh
&#10;# Execute on the command line of the container
export VAULT_ADDR=https://0.0.0.0:8200
&#10;# Skip Verification
export VAULT_SKIP_VERIFY=&#39;true&#39;
&#10;# Check the status of the vault. It should be unsealed
vault status -tls-skip-verify
&#10;# Initiate root token generation (remember the one time password, otp) 
vault operator generate-root -tls-skip-verify -init
&#10;# Provide the 3 unseal keys by repeating the following command 3 times
vault operator generate-root -tls-skip-verify
vault operator generate-root -tls-skip-verify
vault operator generate-root -tls-skip-verify
&#10;# The last command should print out an encoded root token use that ($ENC_ROOT_TOKEN is a placeholder) and the otp ($OTP is a placeholder) to decrypt the root token
vault operator generate-root -tls-skip-verify -decode=$ENC_ROOT_TOKEN -otp=$OTP
&#10;#export Root token at above step($ROOT_TOKENis a placeholder)
export VAULT_TOKEN=$ROOT_TOKEN
&#10;#update certificate
vault write auth/cert/certs/vault-certificate display_name=vault-certificate policies=transit-policy certificate=@/vault/vault_unseal_cert.pem ttl=3650
&#10;# Remember the root token and leave the command line 
exit</code></pre>
</div>
</div></td>
</tr>
</tbody>
</table>

</div>

5.  **Restart Vault**

%% ai-graph-start %%

**Related notes:**
- [[How to unseal Vault Unseal]]
- [[luz-vault - Recovery key encryption with RSA Public Keys (draft - vault operator]]
- [[Security]]
- [[luz-vault - How to run Vault Benchmark]]
- [[Update ePost certificate - .epost.ch - in PROD]]

%% ai-graph-end %%