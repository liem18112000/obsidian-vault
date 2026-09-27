---
title: "[Tool] json-caching-proxy - A middleman who will cache API calls in the local environment"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/TP2020/pages/47290876013/Tool+json-caching-proxy+-+A+middleman+who+will+cache+API+calls+in+the+local+environment
space: "TP2020"
topic: infra
relevance: 0.794
depth: 2.69
updated: 2023-02-09
attachments: 0
tags:
  - confluence
  - infra
  - space/tp2020
---

# [Tool] json-caching-proxy - A middleman who will cache API calls in the local environment

> [!info] Imported from Confluence
> Space **TP2020** · updated 2023-02-09 · [open original](https://axonivy.atlassian.net/wiki/spaces/TP2020/pages/47290876013/Tool+json-caching-proxy+-+A+middleman+who+will+cache+API+calls+in+the+local+environment)
> Relevance 0.794 · topic `infra`

<a href="https://github.com/sonyseng/json-caching-proxy" class="external-link" data-card-appearance="block" rel="nofollow">https://github.com/sonyseng/json-caching-proxy</a>

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="2da19b43-90c8-4f95-a8a4-7e05dc31f440" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
json-caching-proxy -u http://localhost:13080 -p 12080 -l -a
```

</div>

</div>

- `http://localhost:13080`: the real/remote API host URL.

- `12080`: the caching server port.

- `-l`: print log output to console.

- `-a`: cache everything from the remote server.
