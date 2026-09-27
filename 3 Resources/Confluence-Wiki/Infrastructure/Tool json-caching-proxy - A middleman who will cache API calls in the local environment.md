---
ai_hash: 9583fb25efd67e94
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 0
depth: 2.69
entities: []
relevance: 0.794
source: https://axonivy.atlassian.net/wiki/spaces/TP2020/pages/47290876013/Tool+json-caching-proxy+-+A+middleman+who+will+cache+API+calls+in+the+local+environment
space: TP2020
status: reference
tags:
- confluence
- infra
- space/tp2020
title: '[Tool] json-caching-proxy - A middleman who will cache API calls in the local
  environment'
topic: infra
type: source
updated: 2023-02-09
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

%% ai-graph-start %%

**Related notes:**
- _(none above threshold)_

%% ai-graph-end %%