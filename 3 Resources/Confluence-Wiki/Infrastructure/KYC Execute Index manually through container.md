---
title: "KYC | Execute Index manually through container"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/Arrow/pages/48387817515/KYC+Execute+Index+manually+through+container
space: "Arrow"
topic: infra
relevance: 0.779
depth: 2.83
updated: 2025-03-20
attachments: 0
tags:
  - confluence
  - infra
  - space/arrow
---

# KYC | Execute Index manually through container

> [!info] Imported from Confluence
> Space **Arrow** · updated 2025-03-20 · [open original](https://axonivy.atlassian.net/wiki/spaces/Arrow/pages/48387817515/KYC+Execute+Index+manually+through+container)
> Relevance 0.779 · topic `infra`

**Run the command below to trigger Indexer manually:**  
docker exec -it **\${DOCKER_CONTAINER_ID/DOCKER_CONTAINER_NAME}** bash /app/kycindexer/startIndexer

**Beside trigger Indexer manually, we can trigger for Esc-Service to reload index manually using:**

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="08dd8284-0876-4c64-b4f5-234aff1474cd" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
curl --location '<KYC_DOCKER_DOMAIN>:<KYC_DOCKER_PORT>/api/v1/reload'
```

</div>

</div>

Check the returned value is RELOADED or not

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="8c9dc6c5-bc26-48e4-b103-4e972408e235" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
{
    "status": "RELOADED"
}
```

</div>

</div>
