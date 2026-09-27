---
title: "Fork the Synapse server and matrix-js-sdk for DEV"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/TS/pages/48389456074/Fork+the+Synapse+server+and+matrix-js-sdk+for+DEV
space: "TS"
topic: programming
relevance: 0.738
depth: 2.65
updated: 2025-03-07
attachments: 0
tags:
  - confluence
  - programming
  - space/ts
---

# Fork the Synapse server and matrix-js-sdk for DEV

> [!info] Imported from Confluence
> Space **TS** · updated 2025-03-07 · [open original](https://axonivy.atlassian.net/wiki/spaces/TS/pages/48389456074/Fork+the+Synapse+server+and+matrix-js-sdk+for+DEV)
> Relevance 0.738 · topic `programming`

# Synapse server

### Information

- Bitbucket repository<a href="https://bitbucket.org/axonivy-prod/synapse" class="external-link" data-card-appearance="inline" rel="nofollow">https://bitbucket.org/axonivy-prod/synapse</a> (Fork from <a href="https://github.com/element-hq/synapse" class="external-link" rel="nofollow">original source code from Github</a>)

- Our patched version will be pushed to `epost-develop` branch, then the CI pinepile will be triggered to build a new synapse image and push to the artifact registry

- Artifact registry: <a href="https://console.cloud.google.com/artifacts/docker/klara-repo/europe-west6/artifact-registry-container-images/epost-synapse?invt=AbrXUw&amp;project=klara-repo" class="external-link" rel="nofollow">https://console.cloud.google.com/artifacts/docker/klara-repo/europe-west6/artifact-registry-container-images/epost-synapse?invt=AbrXUw&amp;project=klara-repo</a>

- Cloud build trigger: <a href="https://console.cloud.google.com/cloud-build/triggers;region=europe-west6?inv=1&amp;invt=AbrXVA&amp;project=klara-infra&amp;pageState=(%22triggers%22:(%22f%22:%22%255B%257B_22k_22_3A_22_22_2C_22t_22_3A10_2C_22v_22_3A_22_5C_22synapse-develop-branch_5C_22_22_2C_22s_22_3Atrue%257D%255D%22))" class="external-link" rel="nofollow">https://console.cloud.google.com/cloud-build/triggers;region=europe-west6?inv=1&amp;invt=AbrXVA&amp;project=klara-infra&amp;pageState=("triggers":("f":"%255B%257B_22k_22_3A_22_22_2C_22t_22_3A10_2C_22v_22_3A_22_5C_22synapse-develop-branch_5C_22_22_2C_22s_22_3Atrue%257D%255D"))</a>

### Development process

# Matrix-js-sdk
