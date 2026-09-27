---
title: "POS & myKLARA nginx ingress quick notes"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/Helios/pages/47573663885/POS+myKLARA+nginx+ingress+quick+notes
space: "Helios"
topic: infra
relevance: 0.741
depth: 2.66
updated: 2023-12-14
attachments: 0
tags:
  - confluence
  - infra
  - space/helios
---

# POS & myKLARA nginx ingress quick notes

> [!info] Imported from Confluence
> Space **Helios** · updated 2023-12-14 · [open original](https://axonivy.atlassian.net/wiki/spaces/Helios/pages/47573663885/POS+myKLARA+nginx+ingress+quick+notes)
> Relevance 0.741 · topic `infra`

In this page, we will try to summary what we have for POS & myKlara nginx ingress:

- POS & myKLARA will use Internal ingress, the request will come through Cloud Armor

- old version of POS & myKLARA still use webclient ingress; new version will use new ingress.

Pull Requests:

1.  <a href="https://bitbucket.org/axonivy-prod/luz_kubernetes/pull-requests/5211" class="external-link" data-card-appearance="inline" rel="nofollow">https://bitbucket.org/axonivy-prod/luz_kubernetes/pull-requests/5211</a>

2.  <a href="https://bitbucket.org/axonivy-prod/luz_kubernetes/pull-requests/5242" class="external-link" data-card-appearance="inline" rel="nofollow">https://bitbucket.org/axonivy-prod/luz_kubernetes/pull-requests/5242</a>

3.  <a href="https://bitbucket.org/axonivy-prod/luz_kubernetes/pull-requests/5238?t=1" class="external-link" data-card-appearance="inline" rel="nofollow">https://bitbucket.org/axonivy-prod/luz_kubernetes/pull-requests/5238?t=1</a>

4.  <a href="https://bitbucket.org/axonivy-prod/luz_kubernetes/pull-requests/5246" class="external-link" data-card-appearance="inline" rel="nofollow">https://bitbucket.org/axonivy-prod/luz_kubernetes/pull-requests/5246</a>

5.  <a href="https://bitbucket.org/axonivy-prod/luz_kubernetes/pull-requests/5296" class="external-link" data-card-appearance="inline" rel="nofollow">https://bitbucket.org/axonivy-prod/luz_kubernetes/pull-requests/5296</a>

Relates PRs:

1.  <a href="https://bitbucket.org/axonivy-prod/luz_kubernetes/pull-requests/5191" class="external-link" data-card-appearance="inline" rel="nofollow">https://bitbucket.org/axonivy-prod/luz_kubernetes/pull-requests/5191</a>

References:

- [KLARA DNS mappings](https://axonivy.atlassian.net/wiki/spaces/TeamInvisible/pages/47457075353/KLARA+DNS+mappings)

- <a href="https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47120516477/Apply+changes+on+luz+kubernetes" data-card-appearance="inline" rel="nofollow">https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47120516477/Apply+changes+on+luz+kubernetes</a>

- [Recipe: GCP Cloud Armor (WAF) step by step guide](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47063531691/Recipe+GCP+Cloud+Armor+WAF+step+by+step+guide)
