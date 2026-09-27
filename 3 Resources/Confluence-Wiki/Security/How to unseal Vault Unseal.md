---
ai_hash: faee61f042e815b4
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 0
depth: 2.65
entities: []
relevance: 0.711
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47224554424/How+to+unseal+Vault+Unseal
space: LUZ
status: reference
tags:
- confluence
- security
- space/luz
title: How to unseal Vault Unseal
topic: security
type: source
updated: 2023-11-08
---

# How to unseal Vault Unseal

> [!info] Imported from Confluence
> Space **LUZ** · updated 2023-11-08 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47224554424/How+to+unseal+Vault+Unseal)
> Relevance 0.711 · topic `security`

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="b4d6e312-4190-4736-89fd-e20ab5fe98e3" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
# Log into the vault container
kubectl exec -ti --namespace test deployments/luz-vault-unseal -- /bin/sh
 
# Execute on the command line of the container
export VAULT_ADDR=https://127.0.0.1:8200

# Skip Verification
export VAULT_SKIP_VERIFY='true'

# Check the status of the vault. It should be sealed (Sealed - true)
vault status -tls-skip-verify

# Provide the 3 unseal keys by repeating the following command 3 times
vault operator unseal $KEY1
vault operator unseal $KEY2
vault operator unseal $KEY3
```

</div>

</div>

Vault Key:

<a href="https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/46925086807/Recovery+Keys+Secret?search_id=c6858f29-4995-4584-95d2-32890f6f5dd3" data-card-appearance="inline" rel="nofollow">https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/46925086807/Recovery+Keys+Secret?search_id=c6858f29-4995-4584-95d2-32890f6f5dd3</a>

%% ai-graph-start %%

**Related notes:**
- [[Update Vault Unseal self-signed certificate]]
- [[luz-vault - Recovery key encryption with RSA Public Keys (draft - vault operator]]
- [[luz-vault - How to run Vault Benchmark]]
- [[Introduction of Hashicorp Vault]]
- [[Security]]

%% ai-graph-end %%