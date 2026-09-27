---
title: "Devportal - Authentication and Authorization"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47152824733/Devportal+-+Authentication+and+Authorization
space: "LUZ"
topic: security
relevance: 0.764
depth: 2.75
updated: 2023-04-12
attachments: 2
tags:
  - confluence
  - security
  - space/luz
---

# Devportal - Authentication and Authorization

> [!info] Imported from Confluence
> Space **LUZ** · updated 2023-04-12 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47152824733/Devportal+-+Authentication+and+Authorization)
> Relevance 0.764 · topic `security`

The authentication process in Devportal is currently handled by providers and the credential to login has to configure E.g. as environment variables. This means that we cannot login dynamically and it also cannot prevent unauthorized users to access Devportal. Then, we need to use something like Google IAP or VPN (as “Authorization service” as diagram below) to prevent unauthorized users from accessing into Devportal.


![[47152824733-Authen and Authorization.png]]



Backstage already provides a list of provider as built-in providers as below:

- **Google**

- **Google IAP**

- Atlassian

- Auth0

- Azure

- Bitbucket

- Cloudflare Access

- GitHub

- GitLab

- Okta

- OneLogin

- OAuth2Proxy
