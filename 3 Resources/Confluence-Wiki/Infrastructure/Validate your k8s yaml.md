---
title: "Validate your k8s yaml"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/Arrow/pages/6921081995/Validate+your+k8s+yaml
space: "Arrow"
topic: infra
relevance: 0.726
depth: 2.72
updated: 2022-01-04
attachments: 0
tags:
  - confluence
  - infra
  - space/arrow
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
