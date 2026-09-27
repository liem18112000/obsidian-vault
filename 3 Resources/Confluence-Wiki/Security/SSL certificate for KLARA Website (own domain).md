---
ai_hash: 18e6593741c7b1a3
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 0
depth: 2.27
entities: []
relevance: 0.706
source: https://axonivy.atlassian.net/wiki/spaces/IO/pages/48352821258/SSL+certificate+for+KLARA+Website+own+domain
space: IO
status: reference
tags:
- confluence
- security
- space/io
title: SSL certificate for KLARA Website (own domain)
topic: security
type: source
updated: 2025-02-20
---

# SSL certificate for KLARA Website (own domain)

> [!info] Imported from Confluence
> Space **IO** · updated 2025-02-20 · [open original](https://axonivy.atlassian.net/wiki/spaces/IO/pages/48352821258/SSL+certificate+for+KLARA+Website+own+domain)
> Relevance 0.706 · topic `security`

This page outlines the process for renewing the SSL certificates for the KLARA website own domain.

## How it works

TLS/SSL certificates are automatically renewed on PROD. No certificates have been issued for the test environments, e.g. DEV, DEV-STAGING and TEST.

To ensure the successful auto-renewal of SSL certificates for all our domains, the KLARA account at HEXONET (domain hosting provider) must have a sufficient balance.

## Who is in charge?

Manuel Fuchs (financial accountant) is responsible for ensuring there is a sufficient balance in the KLARA account at HEXONET. He will receive an alert from HEXONET if the balance falls below 200 Euros. The notification will be sent to him and to <a href="mailto:owndomain@klara.ch" class="external-link" rel="nofollow">owndomain@klara.ch</a>.

Sebastian Metzger and his team Hacka is responsible for the technical implementation.

## Links

- Technical documentations: [Own Domain](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20490598904/Own+Domain)

%% ai-graph-start %%

**Related notes:**
- [[SSL certificate overview]]
- [[SSL renewal issue]]
- [[mcp.klara.ch current evaluation]]

%% ai-graph-end %%