---
ai_hash: 95ea149deb79f8c2
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 7
depth: 2.44
entities: []
relevance: 0.738
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47975924204/luz-vault+-+How+to+run+Vault+Benchmark
space: LUZ
status: reference
tags:
- confluence
- infra
- space/luz
title: '[luz-vault] - How to run Vault Benchmark'
topic: infra
type: source
updated: 2024-08-12
---

# [luz-vault] - How to run Vault Benchmark

> [!info] Imported from Confluence
> Space **LUZ** · updated 2024-08-12 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47975924204/luz-vault+-+How+to+run+Vault+Benchmark)
> Relevance 0.738 · topic `infra`

# I. Preparation

<div>

|  |  |  |
|----|----|----|
|  |  |  |
| **Kubernetes Branch** 

![[47975924204-star_yellow.png]]

 | kepler/sprint-110/LUZ-122044-load-test-vault | Contains the vault benchmark deployment |

</div>

------------------------------------------------------------------------

## II. How to run on GCP

1.  Checkout to Kubernetes Branch 

![[47975924204-star_yellow.png]]

 - kepler/sprint-110/LUZ-122044-load-test-vault

2.  Go to ***kubernetes-overlays/env-dev-vn/vault-benchmark/config.hcl***

    1.  

![[47975924204-image-20240812-012246.png]]



3.  Adjust the configuration:

    1.  

<div>

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr>
<th></th>
<td><p>NOTE</p></td>
</tr>
<tr>
<th><p><code>vault_addr</code></p></th>
<td><p>Vault address want to be tested.</p>
<p>Run on GCP: <code>http://luz-vault:8200</code></p></td>
</tr>
<tr>
<th><p><code>vault_token</code></p></th>
<td><p>Root vault token.</p>
<p>Reference at: <a href="https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/46925086807/luz-vault+Recovery+Keys+Secret">[luz-vault] Recovery Keys Secret</a></p></td>
</tr>
<tr>
<th><p><code>duration</code></p></th>
<td><p>Test duration</p></td>
</tr>
<tr>
<th><p><code>cleanup</code></p></th>
<td><p>Clean data test after finish or not</p></td>
</tr>
</tbody>
</table>

</div>

4.  **Encrypt the config.hcl** to a secret file. - <sub>How to encrypt:</sub> [Encrypt/Decrypt Kubernetes secret with SOPS](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20513307940/Encrypt+Decrypt+Kubernetes+secret+with+SOPS)

    1.  Open CMD

    2.  <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="95bd3544-45c0-422d-b179-c85953ab45fb" macro-name="code" style="border-width: 1px;">

        <div class="codeContent panelContent pdl">

        ``` syntaxhighlighter-pre
        docker run --rm -it -v <path_to_luz_kubernetes_folder>:/root/development/luz_kubernetes gcr.io/klara-repo/luz-deploy:0.0.1
        ex: docker run --rm -it -v C:/work/project/luz_kubernetes:/root/development/luz_kubernetes gcr.io/klara-repo/luz-deploy:0.0.1
        ```

        </div>

        </div>

    3.  <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="5e362dfb-047f-41d0-b5db-8d8ffd4945d9" macro-name="code" style="border-width: 1px;">

        <div class="codeContent panelContent pdl">

        ``` syntaxhighlighter-pre
        cd root/development/luz_kubernetes/
        ```

        </div>

        </div>

    4.  <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="58dda626-e33d-4d13-a9fb-b3fa79cbbdb5" macro-name="code" style="border-width: 1px;">

        <div class="codeContent panelContent pdl">

        ``` syntaxhighlighter-pre
        cd sops/scripts
        ```

        </div>

        </div>

    5.  <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="c782de73-e7de-4bf3-aae0-4399b9b7e647" macro-name="code" style="border-width: 1px;">

        <div class="codeContent panelContent pdl">

        ``` syntaxhighlighter-pre
        ./vault-bench-mark.sh <env> ../../kubernetes-overlays/env-<env>/vault-benchmark/config.hcl
        ex: ./vault-bench-mark.sh dev-vn ../../kubernetes-overlays/env-dev-vn/vault-benchmark/config.hcl
        ```

        </div>

        </div>

    6.  A new file ***vault-bench-mark-config-env-secret.sops.yaml*** is generated

        1.  

![[47975924204-image-20240812-013955.png]]



5.  Deploy to Kubernetes

    1.  Open CMD

    2.  <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="9e882999-72a5-4682-b2d1-6c78d2eef9cf" macro-name="code" style="border-width: 1px;">

        <div class="codeContent panelContent pdl">

        ``` syntaxhighlighter-pre
        docker run --rm -it -v <path_to_luz_kubernetes_folder>:/root/development/luz_kubernetes gcr.io/klara-repo/luz-deploy:0.0.1
        ex: docker run --rm -it -v C:/work/project/luz_kubernetes:/root/development/luz_kubernetes gcr.io/klara-repo/luz-deploy:0.0.1
        ```

        </div>

        </div>

    3.  <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="4af4b234-a99e-475a-ac07-7a1e39818c96" macro-name="code" style="border-width: 1px;">

        <div class="codeContent panelContent pdl">

        ``` syntaxhighlighter-pre
        gcloud auth login
        ```

        </div>

        </div>

    4.  <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="f1d6f9d4-2f9e-49fc-b173-2eef1a8d1800" macro-name="code" style="border-width: 1px;">

        <div class="codeContent panelContent pdl">

        ``` syntaxhighlighter-pre
        Run on DEV_VN:
        gcloud container clusters get-credentials klara-dev-vn --zone asia-southeast1-a --project klara-nonprod
        ----------------------
        Run on DEV
        gcloud container clusters get-credentials klara-nonprod --zone europe-west6-a --project klara-nonprod
        ```

        </div>

        </div>

    5.  <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="c417f171-8519-47fb-b820-1170b9b6c954" macro-name="code" style="border-width: 1px;">

        <div class="codeContent panelContent pdl">

        ``` syntaxhighlighter-pre
        cd /root/development/luz_kubernetes
        ```

        </div>

        </div>

    6.  <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="bc81a0e4-4ef7-47ef-b3ad-b3ee2f68e381" macro-name="code" style="border-width: 1px;">

        <div class="codeContent panelContent pdl">

        ``` syntaxhighlighter-pre
        ./deploy_to_stdout.sh <env> | kubectl apply -l klara.ch/module=vault-benchmark -f -

        ex: ./deploy_to_stdout.sh dev-vn | kubectl apply -l klara.ch/module=vault-benchmark -f -
        ```

        </div>

        </div>

6.  A vault-benchmark is deployed

    1.  

![[47975924204-image-20240812-015027.png]]



7.  Check the result on the log

    1.  

![[47975924204-image-20240812-015254.png]]



# III. How to run on Local

1.  Clone source code at <a href="https://github.com/hashicorp/vault-benchmark" class="external-link" data-card-appearance="inline" rel="nofollow">https://github.com/hashicorp/vault-benchmark</a>

2.  Create a ***configs*** folder at Root

3.  Copy file <span class="confluence-embedded-file-wrapper conf-macro output-inline" hasbody="false" macro-id="5139d624-887f-482a-99bf-87fbf519baab" macro-name="view-file"><a href="../_attachments/47975924204-config.hcl" class="confluence-embedded-file" data-nice-type="null" data-file-src="/wiki/download/attachments/47975924204/config.hcl?version=1&amp;modificationDate=1723428089943&amp;cacheVersion=1&amp;api=v2" data-mime-type="binary/octet-stream" data-has-thumbnail="true">

![[47975924204-config.hcl]]

</a></span> to the configs folder

4.  Adjust the configuration:

    1.  

<div>

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr>
<th></th>
<td><p>NOTE</p></td>
</tr>
<tr>
<th><p><code>vault_addr</code></p></th>
<td><p>Vault address want to be tested.</p>
<p>Run on local: <code>http://host.docker.internal:8200</code></p></td>
</tr>
<tr>
<th><p><code>vault_token</code></p></th>
<td><p>Root vault token.</p>
<p>Reference at: <a href="https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/46925086807/luz-vault+Recovery+Keys+Secret">[luz-vault] Recovery Keys Secret</a></p></td>
</tr>
<tr>
<th><p><code>duration</code></p></th>
<td><p>Test duration</p></td>
</tr>
<tr>
<th><p><code>cleanup</code></p></th>
<td><p>Clean data test after finish or not</p></td>
</tr>
</tbody>
</table>

</div>

5.  Adapt the ***docker-compose.yaml file***

    1.  <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="80632f15-db84-4791-9be6-1ae8feffdfb4" macro-name="code" style="border-width: 1px;">

        <div class="codeContent panelContent pdl">

        ``` syntaxhighlighter-pre
        # Copyright (c) HashiCorp, Inc.
        # SPDX-License-Identifier: MPL-2.0

        version: "3.8"
        services:

          vault-benchmark:
            image: hashicorp/vault-benchmark:latest
            container_name: vault-benchmark
            hostname: vault-benchmark
            volumes:
              - ./configs/:/opt/vault-benchmark/configs
            command: ["vault-benchmark", "run", "-config", "/opt/vault-benchmark/configs/config.hcl"]
        ```

        </div>

        </div>

6.  Run

    <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="cc86750c-aaed-4d3a-9421-fb4ccdc3bc5e" macro-name="code" style="border-width: 1px;">

    <div class="codeContent panelContent pdl">

    ``` syntaxhighlighter-pre
    docker-compose up --build
    ```

    </div>

    </div>

7.  Check the result on the log

    1.  

![[47975924204-image-20240812-020645.png]]

%% ai-graph-start %%

**Related notes:**
- [[Deploy luz-epc-redis-service on GCP]]
- [[luz-vault - Recovery key encryption with RSA Public Keys (draft - vault operator]]
- [[Setup Redis and DNS on TEST and PROD]]
- [[Kubernetes knowledge]]
- [[Introduction of Hashicorp Vault]]

%% ai-graph-end %%