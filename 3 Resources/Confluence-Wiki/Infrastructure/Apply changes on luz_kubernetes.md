---
ai_hash: 4a678af059ae7bd7
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 0
depth: 2.67
entities: []
relevance: 0.716
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47120516477/Apply+changes+on+luz_kubernetes
space: LUZ
status: reference
tags:
- confluence
- infra
- space/luz
title: Apply changes on luz_kubernetes
topic: infra
type: source
updated: 2023-01-31
---

# Apply changes on luz_kubernetes

> [!info] Imported from Confluence
> Space **LUZ** · updated 2023-01-31 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47120516477/Apply+changes+on+luz_kubernetes)
> Relevance 0.716 · topic `infra`

The following configurations has been done to introduce Google Cloud Armor@KLARA on DEV.

→ apply to all environment

## Environment scripts

- create_environment_cloud_armor.sh (full): <a href="https://bitbucket.org/axonivy-prod/luz_kubernetes/src/master/cluster/gcp/create_environment_cloud_armor.sh" class="external-link" data-card-appearance="inline" rel="nofollow">https://bitbucket.org/axonivy-prod/luz_kubernetes/src/master/cluster/gcp/create_environment_cloud_armor.sh</a>  
  → how to integrate to create_environment.sh script?

- security policy name param: <a href="https://bitbucket.org/axonivy-prod/luz_kubernetes/src/57d8a163c6e59179ec7b4ea2dd1b16e8e280698c/configuration/cluster-gcp-nonprod/env.sh#lines-24" class="external-link" data-card-appearance="inline" rel="nofollow">https://bitbucket.org/axonivy-prod/luz_kubernetes/src/57d8a163c6e59179ec7b4ea2dd1b16e8e280698c/configuration/cluster-gcp-nonprod/env.sh#lines-24</a>

## Cloud Armor

klara-ingress-frontend-config.yaml (frontend config): <a href="https://bitbucket.org/axonivy-prod/luz_kubernetes/src/990fa5b67251/kubernetes-overlays/env-dev/ingress/klara-ingress-frontend-config.yaml?at=master" class="external-link" data-card-appearance="inline" rel="nofollow">https://bitbucket.org/axonivy-prod/luz_kubernetes/src/990fa5b67251/kubernetes-overlays/env-dev/ingress/klara-ingress-frontend-config.yaml?at=master</a>

→ move to /kubernetes/ingress and add to kustomization.yaml

## Design (only DEV)

- Domain: design-dev.klara.tech

klara-prototype-ingress-rule.yaml (ingress rule): <a href="https://bitbucket.org/axonivy-prod/luz_kubernetes/src/master/kubernetes-overlays/env-dev/ingress/klara-prototype-ingress-rule.yaml" class="external-link" data-card-appearance="inline" rel="nofollow">https://bitbucket.org/axonivy-prod/luz_kubernetes/src/master/kubernetes-overlays/env-dev/ingress/klara-prototype-ingress-rule.yaml</a>

→ no changes

klara-prototype-ingress-nginx-ingress.yaml (service): <a href="https://bitbucket.org/axonivy-prod/luz_kubernetes/annotate/990fa5b6725167f3a41cfa05c8cb2e8fffb7ac13/kubernetes-overlays/env-dev/ingress/klara-prototype-nginx-ingress.yaml?at=master#klara-prototype-nginx-ingress.yaml-182,183,184,185,186,194,195,196" class="external-link" data-card-appearance="inline" rel="nofollow">https://bitbucket.org/axonivy-prod/luz_kubernetes/annotate/990fa5b6725167f3a41cfa05c8cb2e8fffb7ac13/kubernetes-overlays/env-dev/ingress/klara-prototype-nginx-ingress.yaml?at=master#klara-prototype-nginx-ingress.yaml-182,183,184,185,186,194,195,196</a>  
→ no changes

klara-prototype-nginx-ingress-backend-config.yaml (full): <a href="https://bitbucket.org/axonivy-prod/luz_kubernetes/src/990fa5b67251/kubernetes-overlays/env-dev/klara-prototype/klara-prototype-nginx-ingress-backend-config.yaml?at=master" class="external-link" data-card-appearance="inline" rel="nofollow">https://bitbucket.org/axonivy-prod/luz_kubernetes/src/990fa5b67251/kubernetes-overlays/env-dev/klara-prototype/klara-prototype-nginx-ingress-backend-config.yaml?at=master</a>  
→ no changes

webclient-nginx-ingress.yaml: <a href="https://bitbucket.org/axonivy-prod/luz_kubernetes/annotate/master/kubernetes/ingress/webclient-nginx-ingress.yaml?at=master#webclient-nginx-ingress.yaml-205:215" class="external-link" data-card-appearance="inline" rel="nofollow">https://bitbucket.org/axonivy-prod/luz_kubernetes/annotate/master/kubernetes/ingress/webclient-nginx-ingress.yaml?at=master#webclient-nginx-ingress.yaml-205:215</a>

→ no changes

## Login

- Domain: login-dev.klara.tech / login-dev.klara-epost.tech

login-ingress-rule.yaml (ingress rule): <a href="https://bitbucket.org/axonivy-prod/luz_kubernetes/src/990fa5b67251/kubernetes-overlays/env-dev/ingress/login-ingress-rule.yaml?at=master" class="external-link" data-card-appearance="inline" rel="nofollow">https://bitbucket.org/axonivy-prod/luz_kubernetes/src/990fa5b67251/kubernetes-overlays/env-dev/ingress/login-ingress-rule.yaml?at=master</a>  
→ move to dev, dev-staging, test, prod (incl. kustomization.yaml s)

login-nginx-ingress.yaml (controller config map and health check): <a href="https://bitbucket.org/axonivy-prod/luz_kubernetes/annotate/990fa5b67251/kubernetes/ingress/login-nginx-ingress.yaml?at=master#login-nginx-ingress.yaml-108:132" class="external-link" data-card-appearance="inline" rel="nofollow">https://bitbucket.org/axonivy-prod/luz_kubernetes/annotate/990fa5b67251/kubernetes/ingress/login-nginx-ingress.yaml?at=master#login-nginx-ingress.yaml-108:132</a>

→ no changes

login-nginx-ingress-backend-config.yaml (service backend config): <a href="https://bitbucket.org/axonivy-prod/luz_kubernetes/src/990fa5b67251/kubernetes-overlays/env-dev/luz-login/login-nginx-ingress-backend-config.yaml?at=master" class="external-link" data-card-appearance="inline" rel="nofollow">https://bitbucket.org/axonivy-prod/luz_kubernetes/src/990fa5b67251/kubernetes-overlays/env-dev/luz-login/login-nginx-ingress-backend-config.yaml?at=master</a>

→ move to /kubernetes/ingress folder and updating kustomization.yaml file

## Webclient

- Domain: dev.klara.tech / dev.klara-epost.tech / rules-dev.klara.tech / online-dev.klara.tech / \*online-dev.klara.tech

webclient-ingress-rule.yaml (ingress rule): <a href="https://bitbucket.org/axonivy-prod/luz_kubernetes/src/master/kubernetes-overlays/env-dev/ingress/webclient-ingress-rule.yaml" class="external-link" data-card-appearance="inline" rel="nofollow">https://bitbucket.org/axonivy-prod/luz_kubernetes/src/master/kubernetes-overlays/env-dev/ingress/webclient-ingress-rule.yaml</a>

→ move to dev, dev-staging, test, prod (incl. kustomization.yaml s)

webclient-nginx-ingress.yaml (service): <a href="https://bitbucket.org/axonivy-prod/luz_kubernetes/annotate/master/kubernetes-overlays/env-dev/ingress/webclient-nginx-ingress.yaml?at=master#webclient-nginx-ingress.yaml-5:10,13,14,15,16" class="external-link" data-card-appearance="inline" rel="nofollow">https://bitbucket.org/axonivy-prod/luz_kubernetes/annotate/master/kubernetes-overlays/env-dev/ingress/webclient-nginx-ingress.yaml?at=master#webclient-nginx-ingress.yaml-5:10,13,14,15,16</a>  
→ move to /kubernetes/ingress folder and updating kustomization.yaml file

webclient-nginx-ingress-backend-config.yaml (service backend config): <a href="https://bitbucket.org/axonivy-prod/luz_kubernetes/src/990fa5b67251/kubernetes-overlays/env-dev/luz-webclient/webclient-nginx-ingress-backend-config.yaml?at=master" class="external-link" data-card-appearance="inline" rel="nofollow">https://bitbucket.org/axonivy-prod/luz_kubernetes/src/990fa5b67251/kubernetes-overlays/env-dev/luz-webclient/webclient-nginx-ingress-backend-config.yaml?at=master</a>

→ move to /kubernetes/ingress folder and updating kustomization.yaml file

webclient-nginx-ingress.yaml (controller config map): <a href="https://bitbucket.org/axonivy-prod/luz_kubernetes/annotate/master/kubernetes/ingress/webclient-nginx-ingress.yaml?at=master#webclient-nginx-ingress.yaml-199,200,201,202,203,204,205,206,207,208" class="external-link" data-card-appearance="inline" rel="nofollow">https://bitbucket.org/axonivy-prod/luz_kubernetes/annotate/master/kubernetes/ingress/webclient-nginx-ingress.yaml?at=master#webclient-nginx-ingress.yaml-199,200,201,202,203,204,205,206,207,208</a>

→ no changes

## Klara Website

- Own domains

No configs needed.

## KLARA Public API

- Domain: api-dev.klara.tech / api-dev-klara-epost.tech

public-api-ingress-rule.yaml (ingress rule): <a href="https://bitbucket.org/axonivy-prod/luz_kubernetes/src/master/kubernetes-overlays/env-dev/ingress/public-api-ingress-rule.yaml" class="external-link" data-card-appearance="inline" rel="nofollow">https://bitbucket.org/axonivy-prod/luz_kubernetes/src/master/kubernetes-overlays/env-dev/ingress/public-api-ingress-rule.yaml</a>

→ move to /kubernetes/ingress folder and updating kustomization.yaml file

patch-luz-public-api-kong-k8s.yaml (service): <a href="https://bitbucket.org/axonivy-prod/luz_kubernetes/annotate/c5934cc88c3cbf5075c283bb53a2dc2103be9185/kubernetes-overlays/env-dev/luz-public-api-kong/patch-luz-public-api-kong-k8s.yaml?at=master#patch-luz-public-api-kong-k8s.yaml-30:35" class="external-link" data-card-appearance="inline" rel="nofollow">https://bitbucket.org/axonivy-prod/luz_kubernetes/annotate/c5934cc88c3cbf5075c283bb53a2dc2103be9185/kubernetes-overlays/env-dev/luz-public-api-kong/patch-luz-public-api-kong-k8s.yaml?at=master#patch-luz-public-api-kong-k8s.yaml-30:35</a>

→ update dev, dev-staging, test and prod and kustomization.yamls

public-api-kong-proxy-service-backend-config.yaml (service backend config) <a href="https://bitbucket.org/axonivy-prod/luz_kubernetes/src/c5934cc88c3c/kubernetes-overlays/env-dev/luz-public-api-kong/public-api-kong-proxy-service-backend-config.yaml?at=master" class="external-link" data-card-appearance="inline" rel="nofollow">https://bitbucket.org/axonivy-prod/luz_kubernetes/src/c5934cc88c3c/kubernetes-overlays/env-dev/luz-public-api-kong/public-api-kong-proxy-service-backend-config.yaml?at=master</a>

→ moved to /kubernetes/ingress

%% ai-graph-start %%

**Related notes:**
- [[POS & myKLARA nginx ingress quick notes]]
- [[Recipe Introduce a Global HTTP(S) Load Balancer on GCP at KLARA]]
- [[Apply branch code of webclient-nginx-ingress.yaml]]
- [[Kubernetes knowledge]]
- [[Deploy luz-epc-redis-service on GCP]]

%% ai-graph-end %%