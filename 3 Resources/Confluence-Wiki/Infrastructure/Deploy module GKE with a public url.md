---
title: "Deploy module GKE with a public url"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/Helios/pages/48923050045/Deploy+module+GKE+with+a+public+url
space: "Helios"
topic: infra
relevance: 0.757
depth: 2.84
updated: 2025-12-01
attachments: 13
tags:
  - confluence
  - infra
  - space/helios
---

# Deploy module GKE with a public url

> [!info] Imported from Confluence
> Space **Helios** · updated 2025-12-01 · [open original](https://axonivy.atlassian.net/wiki/spaces/Helios/pages/48923050045/Deploy+module+GKE+with+a+public+url)
> Relevance 0.757 · topic `infra`

After your module already deployed on GCP.


![[48923050045-image-20251201-032253.png]]



To create an public url for the module we need config Gateway and HTTPRoute following step below:

1.  **Create a Gateway** — defines the public entry point (port 80/443).

2.  **Create an HTTPRoute** — connects the Gateway to your module/service and defines hostname/path routing.

3.  **Add healthCheck probes (liveness & readiness)**

4.  **Apply the manifests** .

5.  **Get the public URL** — use the hostname you set or the Gateway’s external IP.

For example,

1.  **we reuse the existing Gateway** <a href="https://client-dev.klara-epost.tech/start-websocket" class="external-link" rel="nofollow">https://client-dev.klara-epost.tech</a>, it already configured with name webclient-gateway **(Step 1)**


![[48923050045-image-20251201-032923.png]]



2.  **we will config HttpRoute for this Gateway.**

- Go to file webclient-gateway in module luz-kubernetes

- Find the name of the configured route by search the hostname: “<a href="https://client-dev.klara-epost.tech/start-websocket" class="external-link" rel="nofollow">client-dev.klara-epost.tech</a>”


![[48923050045-image-20251201-034625.png]]



- In the `rules`, add new `matches` config to define your URL as below


![[48923050045-image-20251201-033929.png]]



**Note:**

`type: PathPrefix means: any request whose path starts with /start-websocket will match.`

`backendRefs: This defines where the traffic should be sent if the path matches.`

- After that, we will scan that Route configuration block and paste it into another file to deploy it independently.


![[48923050045-image-20251201-033605.png]]



- Create a new YAML file and paste that configuration block inside

- 

![[48923050045-image-20251201-033719.png]]



3.  **Add healthCheck probes (liveness & readiness)**

- Create new .yaml file and add health check block as below

- 

![[48923050045-image-20251201-035750.png]]



4.  Apply the changes

- Run this command to create new heathcheck config: `kubectl apply -f <your-filename>.yaml`


![[48923050045-image-20251201-035847.png]]



- Run this command to apply the config: `kubectl apply -f <your-filename>.yaml`


![[48923050045-image-20251201-040217.png]]



- Wait for 1-2 minutes, go to GCP to check your config


![[48923050045-image-20251201-040511.png]]



**When everything is done, we can paste your config into webclient-gateway.yaml and commit it.**
