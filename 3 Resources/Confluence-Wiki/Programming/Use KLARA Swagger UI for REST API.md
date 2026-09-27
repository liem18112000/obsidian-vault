---
ai_hash: fc497a65ce74bca0
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 0
depth: 3
entities: []
relevance: 0.938
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20448093654/Use+KLARA+Swagger+UI+for+REST+API
space: LUZ
status: reference
tags:
- confluence
- programming
- space/luz
title: Use KLARA Swagger UI for REST API
topic: programming
type: source
updated: 2017-06-08
---

# Use KLARA Swagger UI for REST API

> [!info] Imported from Confluence
> Space **LUZ** · updated 2017-06-08 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20448093654/Use+KLARA+Swagger+UI+for+REST+API)
> Relevance 0.938 · topic `programming`

# Problem

You'd like to access the swagger-ui (<a href="http://swagger.io/swagger-ui/" class="external-link" rel="nofollow">http://swagger.io/swagger-ui/</a>) for KLARA to browse, examine and try-out the available REST resources, but you don't know how.

# Solution

In this article a step-by-step guide is provided to access the swagger-ui on the KLARA development environment. Find here useful parameters of this environment:

- Public access: <a href="https://klara-dev.axonivy.io" class="external-link" rel="nofollow">https://klara-dev.axonivy.io</a>
- Hostname of the KLARA application server, used throughout this example: `klaradev`
- IP of the KLARA application server: `10.124.0.20`
- swagger-ui addresses: 
  - Compensation (payroll): <a href="http://klaradev:8080/luz_compensation/" class="external-link" rel="nofollow">http://klaradev:8080/luz_compensation/</a>
  - Person (addresses, persons, companies): <a href="http://klaradev:8080/luz_person/" class="external-link" rel="nofollow">http://klaradev:8080/luz_person/</a>
  - Finance: <a href="http://klaradev:8080/luzfin_finance/" class="external-link" rel="nofollow">http://klaradev:8080/luzfin_finance/</a>

## Preparation

To be able to access the swagger-ui of a KLARA installation, you need to have direct access to the KLARA application server (WildFly). Usually that means, that you either should use a workstation connected to the AXON IVY LANs in Schwerzenbach or Bolligen, or that you have to establish a VPN connection into the infrastructure where the KLARA application server is located (usually VPN to AWS FRA).

Defining a host entry for the KLARA application server is strongly recommended to ease usage of URIs:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="bac57079-2237-4db7-89e9-6eb362a15cb6" macro-name="code" style="border-width: 1px;">

<div class="codeHeader panelHeader pdl" style="border-bottom-width: 1px;">

**/etc/hosts (e.g. for mac clients)**

</div>

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
# Host Database

127.0.0.1   localhost
::1         localhost 

10.124.0.20 klaradev
```

</div>

</div>

If needed, establish a VPN in order to be able to access the KLARA application server. You can easily check if you can reach the server with a ping:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="03bf19f1-b703-4094-b547-4321d1b4ddaa" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
> ping klaradev
PING klaradev (10.124.0.20): 56 data bytes
64 bytes from 10.124.0.20: icmp_seq=0 ttl=63 time=51.011 ms
64 bytes from 10.124.0.20: icmp_seq=1 ttl=63 time=60.770 ms
^C
--- klaradev ping statistics ---
2 packets transmitted, 2 packets received, 0.0% packet loss
round-trip min/avg/max/stddev = 51.011/55.891/60.770/4.879 ms
```

</div>

</div>

If the ping was not successful, please fix that before continuing.

## Tokens

Various calls to REST resources are needed to get a valid token to be used for the swagger-ui. The usage of Postman (<a href="https://www.getpostman.com" class="external-link" rel="nofollow">https://www.getpostman.com</a>) is recommended to execute these calls. To initialize the 3 calls needed to get the tokens, please get the postman collection <a href="https://bitbucket.org/axonivy-prod/luz_devops/raw/HEAD/klara-maintenance-scripts/src/main/postman/KLARA-Tokens.postman_collection.json" class="external-link" rel="nofollow">https://bitbucket.org/axonivy-prod/luz_devops/raw/HEAD/klara-maintenance-scripts/src/main/postman/KLARA-Tokens.postman_collection.json</a> and import it. You need to set the following environment variables in Postman:

- `luzserver`: e.g. `klaradev`
- `user`: e.g. `admin`
- `password`: e.g. `admin`
- `luzuser`: The user which has a role in the company you'd like to examine. Only needed to request the list of tenants. E.g. `daniel.gauch@axonivy.com`
- `companytenant`: if you already know. Needed to get the tenant-specific token. E.g. `331d568c-15a5-4696-b248-4aa957f44bb2`

To access the REST resources, a valid token is needed. There are two types of tokens, a public and a tenant-specific token. With the public token, only public REST resources may be accessed. With a tenant-specific token, all REST resources may be accessed. To access the swagger-ui, you have to have a valid tenant-specific token. To request a tenant-specific token, you need to have a valid role for the affected company, or you need the credentials for a user which is allowed to create access tokens for all tenants. Beside that you need to know the company tenant id.

With the following 3 steps, you can examine the company tenant ids of any given user (if you already know the company tenant id, you may skip this and directly request a tenant-specific token):

1.  *Get public token* request in the Postman collection: Request a public token with a POST call to the following url and with basic authentication: <span class="nolink">`http://klaradev:8080/luzsec/api/tokens`.</span>
2.  *Get tenants* request in the Postman collection: Get a list of all tenants for a given user with a GET request to: `http://klaradev:8080/luztenant/api/{{luzuser}}/tenants`
3.  Go through the list of company tenants, copy the id of the desired company and set the Postman environment variable `companytenant` to that id, e.g. `331d568c-15a5-4696-b248-4aa957f44bb2`

After examine the correct company tenant id, eventually the tenant-specific token can be requested:

1.  *Get company token* request in the Postman collection: Get the tenant-specific token git a POST call to `http://klaradev:8080/luzsec/api/{{companytenant}}/access/tokens`
2.  Copy the returned token (just the value for the key "token" without `"`) and use it for the swagger-ui in the field token. See the swagger-ui links above.

Enjoy REST resources through swagger-ui.

%% ai-graph-start %%

**Related notes:**
- [[How to use Public API to create update KLARA Business Company]]
- [[KLARA Integration (request access token & call API)]]
- [[OpenAPI UI (API on SwaggerUI)]]
- [[How to consume luz api]]
- [[How to call generic interface document API on dev]]

%% ai-graph-end %%