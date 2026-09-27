---
ai_hash: cc73d9502aa43a86
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 0
depth: 2.72
entities: []
relevance: 0.726
source: https://axonivy.atlassian.net/wiki/spaces/Arrow/pages/6921081995/Validate+your+k8s+yaml
space: Arrow
status: reference
tags:
- confluence
- infra
- space/arrow
title: Validate your k8s yaml
topic: infra
type: source
updated: 2022-01-04
---

# Validate your k8s yaml

> [!info] Imported from Confluence
> Space **Arrow** · updated 2022-01-04 · [open original](https://axonivy.atlassian.net/wiki/spaces/Arrow/pages/6921081995/Validate+your+k8s+yaml)
> Relevance 0.726 · topic `infra`

~~**docker run --rm -v D:/work/KLARA/<a href="http://luz_kubernetes/opt/development" class="external-link" rel="nofollow">luz_kubernetes:/opt/development</a> -it <a href="http://gcr.io/klara-repo/luz-deploy" class="external-link" rel="nofollow">gcr.io/klara-repo/luz-deploy</a> bash ( D:/work/KLARA/<a href="http://luz_kubernetes/opt/development" class="external-link" rel="nofollow">luz_kubernetes</a> depends on your machine)**~~

~~**<a href="http://luz_kubernetes/opt/development" class="external-link" rel="nofollow">opt/development/deploy_to_stdout.sh dev &gt; test.yaml</a> (dev is eviroment, maybe test, dev-staging,...)**~~

~~<span class="legacy-color-text-default">Open cmd on your host folder contain luz_kubernetes (D:/work/KLARA/<a href="http://luz_kubernetes/opt/development" class="external-link" rel="nofollow">luz_kubernetes</a>)</span>~~

~~**kubectl apply --validate=true --dry-run=client --filename=test.yaml**~~

~~<span class="legacy-color-text-default">(Not sure, you may need to login gcp first)</span>~~

<span class="legacy-color-text-default">Newer version: [Validate Kubernetes configurations in local machine](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47006253620/Validate+Kubernetes+configurations+in+local+machine)</span>

%% ai-graph-start %%

**Related notes:**
- [[Recipe Deploy with Terraform]]
- [[Kubernetes knowledge]]
- [[Apply branch code of webclient-nginx-ingress.yaml]]
- [[Verify kubectl context before GKE rollout - _context file can disagree]]
- [[Apply changes on luz_kubernetes]]

%% ai-graph-end %%