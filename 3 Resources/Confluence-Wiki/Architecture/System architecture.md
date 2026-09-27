---
title: "System architecture"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/Arrow/pages/6903439129/System+architecture
space: "Arrow"
topic: architecture
relevance: 0.711
depth: 2.44
updated: 2020-05-22
attachments: 8
tags:
  - confluence
  - architecture
  - space/arrow
---

# System architecture

> [!info] Imported from Confluence
> Space **Arrow** · updated 2020-05-22 · [open original](https://axonivy.atlassian.net/wiki/spaces/Arrow/pages/6903439129/System+architecture)
> Relevance 0.711 · topic `architecture`

![[6903439129-system_architecture.png]]



  

The public API users interact with our system by requesting to the exposed Load balancer, Kong proxy **<span class="legacy-color-text-red2">(1)</span>**.

The requests then go through Kong ingress controller. It will check if the requests match any of the defined routes **<span class="legacy-color-text-red2">(2)</span>**.

Each route is defined as an ingress rule which map a path to a backend service.

Each plugin can be defined with a yaml file **<span class="legacy-color-text-red2">(3)</span>**. The plugins can then be applied on desired routes. Example: in the diagram above, the /comments route is being limited to 30 requests per minute by a plugin, while the /billing route is only being limit to 20 requests per minute.

To authorize with Keycloak, we need the OIDC plugin **<span class="legacy-color-text-red2">(4)</span>**. This plugin again can be applied for wanted routes and it will connect with the existing keycloak-service on GCP to authorize.

After verifying the route and authorizing successfully with Keycloak, Kong ingress controller then forward the requests to the client adapters for public API <span class="legacy-color-text-red2">**(5)**<span class="legacy-color-text-default">.</span></span>

<span class="legacy-color-text-red2"><span class="legacy-color-text-default">The client adapters will make the neccessary tokens (generic, full token) and call the existing backend modules <span class="legacy-color-text-red2">**(6)**<span class="legacy-color-text-default">.</span></span></span></span>

<span class="legacy-color-text-red2"><span class="legacy-color-text-default"><span class="legacy-color-text-red2"><span class="legacy-color-text-default">To get the statistics about our services that receive the requests through Kong, we need a plugin for Kong to exports metrics in <a href="https://github.com/prometheus/docs/blob/master/content/docs/instrumenting/exposition_formats.md" class="external-link" rel="nofollow" style="text-decoration: underline;">Prometheus Exposition format</a> <span class="legacy-color-text-red2">**(7)**</span>. This metrics can be then scraped and displayed using Grafana dashboard.</span></span></span></span>
