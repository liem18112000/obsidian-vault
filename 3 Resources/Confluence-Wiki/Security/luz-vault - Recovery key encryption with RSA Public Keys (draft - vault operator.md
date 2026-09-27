---
ai_hash: 9b8d9505eb580539
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 0
depth: 3
entities: []
relevance: 0.802
source: https://axonivy.atlassian.net/wiki/spaces/IO/pages/48572399662/luz-vault+-+Recovery+key+encryption+with+RSA+Public+Keys+draft+-+vault+operator+generate-recovery-keys+only+avail+in+Enterprise+edition
space: IO
status: reference
tags:
- confluence
- security
- space/io
title: 'luz-vault - Recovery key encryption with RSA Public Keys (draft - vault operator
  generate-recovery-keys : only avail in Enterprise edition)'
topic: security
type: source
updated: 2025-07-11
---

# luz-vault - Recovery key encryption with RSA Public Keys (draft - vault operator generate-recovery-keys : only avail in Enterprise edition)

> [!info] Imported from Confluence
> Space **IO** · updated 2025-07-11 · [open original](https://axonivy.atlassian.net/wiki/spaces/IO/pages/48572399662/luz-vault+-+Recovery+key+encryption+with+RSA+Public+Keys+draft+-+vault+operator+generate-recovery-keys+only+avail+in+Enterprise+edition)
> Relevance 0.802 · topic `security`

The main issue is that luz-vault pod has no tools installed to encrypt the recovery keys. So we have to execute the key rotation outside the pod with Vault CLI.

# **RSA Public Keys with OpenSSL - Generate RSA Key Pair**

This has to be done by the key holders. Example of key creation:

Generate a private key (2048 bits)

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="30b0c995-c0ae-488a-bc99-566d34bc5ce7" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
openssl genpkey -algorithm RSA -out private_key.pem -pkeyopt rsa_keygen_bits:2048
```

</div>

</div>

Extract the public key from the private key

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="0cdc0cb1-1494-4661-a131-9712d9f0b6bc" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
openssl rsa -pubout -in private_key.pem -out public_key.pem
```

</div>

</div>

# Encryption Approach: OpenSSL + RSA Public Keys

Prerequisite Assume you already have these files:

- `user1.pub.pem`, ..., `user5.pub.pem` — RSA public keys for each user.

Prerequisite Vault CLI

install for Ubuntu/Debian

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="de395795-dbae-49ac-9840-67608f5f3ace" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
curl -fsSL https://apt.releases.hashicorp.com/gpg | sudo gpg --dearmor -o /usr/share/keyrings/hashicorp-archive-keyring.gpg
echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/hashicorp.list
sudo apt update && sudo apt install vault
```

</div>

</div>

install for macOS

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="b2650824-f57c-4afd-aaa3-497dc95d3303" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
brew tap hashicorp/tap
brew install hashicorp/tap/vault
```

</div>

</div>

Config

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="14dcd78f-4edc-4bac-b34d-291f6ac760dc" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
export VAULT_ADDR=http://127.0.0.1:8200
kubectl port-forward luz-vault-0 8200:8200 -n dev
```

</div>

</div>

Check status

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="650ad68c-6825-455a-8716-7dfbdb3d2f5a" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
vault status
```

</div>

</div>

## Secure Distribution

### 1. Run script to rotate key and encrypt

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="840e3231-78e7-468d-b8b8-191c290dca9c" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
#!/bin/bash

# Step 1: Run init and store full output
init_output=$(vault operator generate-recovery-keys -recovery-shares=5 -recovery-threshold=3 -format=json)

# Step 2: Extract recovery keys using jq
for i in {0..4}; do
  share=$(echo "$init_output" | jq -r ".recovery_keys_b64[$i]")
  pubkey="user$((i+1))_pub.pem"
  outfile="enc_user$((i+1))_recovery_key.bin"

  # Encrypt the share immediately
  echo -n "$share" | openssl rsautl -encrypt -pubin -inkey "$pubkey" -out "$outfile"
  echo "Encrypted share saved to $outfile"
done

# Step 3: Output root token (if needed)
root_token=$(echo "$init_output" | jq -r ".root_token")
echo "Initial Root Token: $root_token"
```

</div>

</div>

### How a User Decrypts Their Share

Each user can decrypt with their private key:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="b4761c77-5049-4509-8aa0-af2f72dca47c" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
openssl rsautl -decrypt -inkey user1_private.pem -in enc_user1_recovery_key.bin
```

</div>

</div>

%% ai-graph-start %%

**Related notes:**
- [[Update Vault Unseal self-signed certificate]]
- [[How to unseal Vault Unseal]]
- [[luz-vault - How to run Vault Benchmark]]
- [[Introduction of Hashicorp Vault]]
- [[Encryption and decryption flows with Vault]]

%% ai-graph-end %%