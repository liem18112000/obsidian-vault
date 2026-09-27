---
ai_hash: c73a9901f4990354
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 3
depth: 2.41
entities: []
relevance: 0.736
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20502829443/API+Gateway+Evaluation
space: LUZ
status: reference
tags:
- confluence
- programming
- space/luz
title: API Gateway Evaluation
topic: programming
type: source
updated: 2020-04-20
---

# API Gateway Evaluation

> [!info] Imported from Confluence
> Space **LUZ** · updated 2020-04-20 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20502829443/API+Gateway+Evaluation)
> Relevance 0.736 · topic `programming`

## **1. Criteria**

To start evaluating each API gateway, we need something to serve as a baseline to compare the API gateways against.  
Below are the criterias that we think should be considered when looking at a API gateway.

### Deployment:

- How easy it is to get the api gateway running?
- Is it easy to maintain? (add/remove/edit services, routing)
- **How easy it is to integrate with GCP, kubernetes** **(need to focus on this point, easy 3rd party integration for it on kubernetes?)**
- Seperated/integrated Datastore?

### Community support:

- Is there a good amount of tutorials/documents online?
- Is there many unresolved questions/bugs on Q&A sites (github, stackoverflow)?
- Is it easy to extend using plugins?

### Features:

- Does it have load balancing?
- Does it have rate limiting?
- What security measures does it support (with support for Keycloak)?
- Does it have monitoring/analytics capabilities (possibly through Admin GUI/Dashboard)?
- Could it be used with Zapier?

### Performance:

- What is it built on top of?
- Provide some info/charts about its performance?

### Paid support:

- Does it have free version?
- Does it have paid/enterprise version? How does it compared to the free version?
- What is the pricing plan for the paid/enterprise version?

### Popularity:

- When was it founded?
- List out some companies/projects that use it?

## **2. Evaluation Overview**

<div>

<table style="width: 100.0%;">
<colgroup>
<col style="width: 10%" />
<col style="width: 10%" />
<col style="width: 10%" />
<col style="width: 10%" />
<col style="width: 10%" />
<col style="width: 10%" />
<col style="width: 10%" />
<col style="width: 10%" />
<col style="width: 10%" />
<col style="width: 10%" />
</colgroup>
<tbody>
<tr>
<th colspan="5">Criteria/API GW</th>
<th>Kong</th>
<th>Ambassador</th>
<th><span><a href="http://Tyk.io" class="external-link" rel="nofollow">Tyk.io</a></span></th>
<th>Traefik</th>
<th><span class="legacy-color-text-default">NGINX Ingress</span></th>
</tr>
&#10;<tr>
<td rowspan="6"><p><br />
</p>
<p><br />
</p>
<p><br />
</p>
<p><strong>Deployment</strong></p></td>
<td rowspan="6"><br />
&#10;<p><br />
</p>
<p><br />
</p>
<p>Kubernetes support<br />
<br />
</p></td>
<td colspan="3" style="text-align: center;">Setup</td>
<td style="text-align: center;">via yaml file (3.5/5)</td>
<td style="text-align: center;">via yaml file (3/5)</td>
<td style="text-align: center;">via yaml file (4/5)</td>
<td style="text-align: center;">via yaml file (3.5/5)</td>
<td style="text-align: center;">via yaml file (3/5)</td>
</tr>
<tr>
<td rowspan="5" style="text-align: center;"><p><br />
</p>
<p><br />
</p>
<p>3rd party integration</p></td>
<td rowspan="3" style="text-align: center;"><p><br />
</p>
<p>Service mesh</p></td>
<td style="text-align: center;">Istio</td>
<td class="highlight-green confluenceTd" style="text-align: center;">Yes</td>
<td class="highlight-green confluenceTd" style="text-align: center;">Yes</td>
<td class="highlight-red confluenceTd" style="text-align: center;"><br />
</td>
<td class="highlight-red confluenceTd" style="text-align: center;"><br />
</td>
<td class="highlight-red confluenceTd" style="text-align: center;"><br />
</td>
</tr>
<tr>
<td style="text-align: center;">Consul</td>
<td class="highlight-red confluenceTd" style="text-align: center;"><br />
</td>
<td class="highlight-green confluenceTd" style="text-align: center;">Yes</td>
<td class="highlight-red confluenceTd" style="text-align: center;"><br />
</td>
<td class="highlight-green confluenceTd" style="text-align: center;">Yes</td>
<td class="highlight-red confluenceTd" style="text-align: center;"><br />
</td>
</tr>
<tr>
<td style="text-align: center;">Linkerd 2</td>
<td class="highlight-red confluenceTd" style="text-align: center;"><br />
</td>
<td class="highlight-green confluenceTd" style="text-align: center;">Yes</td>
<td class="highlight-red confluenceTd" style="text-align: center;"><br />
</td>
<td class="highlight-green confluenceTd" style="text-align: center;">Yes</td>
<td class="highlight-green confluenceTd" style="text-align: center;">Yes</td>
</tr>
<tr>
<td rowspan="2" style="text-align: center;"><br />
Dashboard</td>
<td style="text-align: center;">Prometheus</td>
<td class="highlight-green confluenceTd" style="text-align: center;">Yes</td>
<td class="highlight-green confluenceTd" style="text-align: center;">Yes</td>
<td class="highlight-green confluenceTd" style="text-align: center;">Yes</td>
<td class="highlight-green confluenceTd" style="text-align: center;">Yes</td>
<td class="highlight-green confluenceTd" style="text-align: center;">Yes</td>
</tr>
<tr>
<td style="text-align: center;">Grafana</td>
<td class="highlight-green confluenceTd" style="text-align: center;">Yes</td>
<td class="highlight-green confluenceTd" style="text-align: center;">Yes</td>
<td class="highlight-green confluenceTd" style="text-align: center;">Yes</td>
<td class="highlight-green confluenceTd" style="text-align: center;">Yes</td>
<td class="highlight-green confluenceTd" style="text-align: center;">Yes</td>
</tr>
<tr>
<td colspan="3" rowspan="13"><p><br />
</p>
<p><br />
</p>
<p><br />
</p>
<p><br />
</p>
<p><br />
</p>
<p><br />
</p>
<p><strong>Features</strong></p>
<p><br />
</p>
<p><br />
</p>
<p><br />
</p>
<p><br />
</p></td>
<td rowspan="5" style="text-align: center;"><p><br />
</p>
<p><br />
</p>
<p><br />
</p>
<p>Protocols</p></td>
<td style="text-align: center;">http</td>
<td class="highlight-green confluenceTd" style="text-align: center;">Yes</td>
<td class="highlight-green confluenceTd" style="text-align: center;">Yes</td>
<td class="highlight-green confluenceTd" style="text-align: center;">Yes</td>
<td class="highlight-green confluenceTd" style="text-align: center;">Yes</td>
<td class="highlight-green confluenceTd" style="text-align: center;">Yes</td>
</tr>
<tr>
<td style="text-align: center;">https</td>
<td class="highlight-green confluenceTd" style="text-align: center;">Yes</td>
<td class="highlight-green confluenceTd" style="text-align: center;">Yes</td>
<td class="highlight-green confluenceTd" style="text-align: center;">Yes</td>
<td class="highlight-green confluenceTd" style="text-align: center;">Yes</td>
<td class="highlight-green confluenceTd" style="text-align: center;">Yes</td>
</tr>
<tr>
<td style="text-align: center;">tcp</td>
<td class="highlight-green confluenceTd" style="text-align: center;">Yes</td>
<td class="highlight-green confluenceTd" style="text-align: center;">Yes</td>
<td class="highlight-green confluenceTd" style="text-align: center;">Yes</td>
<td class="highlight-green confluenceTd" style="text-align: center;">Yes</td>
<td class="highlight-green confluenceTd" style="text-align: center;">Yes</td>
</tr>
<tr>
<td style="text-align: center;">tcp+tls</td>
<td class="highlight-green confluenceTd" style="text-align: center;">Yes</td>
<td class="highlight-green confluenceTd" style="text-align: center;">Yes</td>
<td class="highlight-green confluenceTd" style="text-align: center;">Yes</td>
<td class="highlight-green confluenceTd" style="text-align: center;">Yes</td>
<td class="highlight-red confluenceTd" style="text-align: center;"><br />
</td>
</tr>
<tr>
<td style="text-align: center;">grpc</td>
<td class="highlight-green confluenceTd" style="text-align: center;">Yes</td>
<td class="highlight-green confluenceTd" style="text-align: center;">Yes</td>
<td class="highlight-green confluenceTd" style="text-align: center;">Yes</td>
<td class="highlight-green confluenceTd" style="text-align: center;">Yes</td>
<td class="highlight-green confluenceTd" style="text-align: center;">Yes</td>
</tr>
<tr>
<td colspan="2" style="text-align: center;">Scope</td>
<td style="text-align: center;"><span class="legacy-color-text-default">cross-namespace</span></td>
<td style="text-align: center;"><span class="legacy-color-text-default">cross-namespace</span></td>
<td style="text-align: center;"><span class="legacy-color-text-default">cross-namespace</span></td>
<td style="text-align: center;"><span class="legacy-color-text-default">cross-namespace</span></td>
<td style="text-align: center;"><span class="legacy-color-text-default">cross-namespace</span></td>
</tr>
<tr>
<td colspan="2" style="text-align: center;"><span class="legacy-color-text-default">Backend service discovery</span></td>
<td style="text-align: center;"><span class="legacy-color-text-default">dynamic</span></td>
<td style="text-align: center;"><span class="legacy-color-text-default">dynamic</span></td>
<td style="text-align: center;"><span class="legacy-color-text-default">dynamic</span></td>
<td style="text-align: center;"><span class="legacy-color-text-default">dynamic</span></td>
<td style="text-align: center;"><span class="legacy-color-text-default">dynamic</span></td>
</tr>
<tr>
<td colspan="2" style="text-align: center;">Rate limiting</td>
<td style="text-align: center;"><span class="legacy-color-text-default">rate limit, request size limit, request termination, response rate limit, advanced rate limit (enterprise version)</span></td>
<td style="text-align: center;"><span class="legacy-color-text-default">rate limit, load shedding</span></td>
<td style="text-align: center;"><span class="legacy-color-text-default">uptime tests, enforced timeouts, circuit breaker, throttling</span></td>
<td style="text-align: center;"><span class="legacy-color-text-default">max-conn, rate limit, ip whitelist</span></td>
<td style="text-align: center;"><span class="legacy-color-text-default">rate limit, rate-limit-burst</span></td>
</tr>
<tr>
<td colspan="2" style="text-align: center;"><span class="legacy-color-text-default">Traffic routing</span></td>
<td style="text-align: center;"><ul>
<li>host-based</li>
<li>path-based</li>
<li>method-based</li>
</ul></td>
<td style="text-align: center;"><ul>
<li>host-based</li>
<li>path-based</li>
<li>method-based</li>
<li>header-based</li>
</ul>
<p>(all with regex)</p></td>
<td style="text-align: center;"><ul>
<li>host-based</li>
<li>path-based</li>
<li>method-based</li>
<li>header-based</li>
<li><span class="legacy-color-text-default">RE2 Regexp</span></li>
</ul>
<p><br />
</p></td>
<td style="text-align: center;"><ul>
<li>host-based (with regex)</li>
<li>path-based (with regex)</li>
<li>method-based</li>
<li>header-based (with regex)</li>
<li><span class="legacy-color-text-default">query</span></li>
<li><span class="legacy-color-text-default">path prefix</span></li>
</ul></td>
<td style="text-align: left;"><ul>
<li><span class="legacy-color-text-default">host-based</span></li>
<li><span class="legacy-color-text-default">path-based (with regex)</span></li>
</ul></td>
</tr>
<tr>
<td colspan="2" style="text-align: center;"><span class="legacy-color-text-default">Traffic distribution</span></td>
<td style="text-align: center;"><span class="legacy-color-text-default">canary, acl, blue-green, proxy caching</span></td>
<td style="text-align: center;"><span class="legacy-color-text-default">canary, a/b, shadowing, http headers, acl, whitelist</span></td>
<td class="highlight-red confluenceTd" style="text-align: center;"><br />
</td>
<td style="text-align: center;"><span class="legacy-color-text-default">canary, blue-green, shadowing</span></td>
<td class="highlight-red confluenceTd" style="text-align: center;"><br />
</td>
</tr>
<tr>
<td colspan="2" style="text-align: center;"><span class="legacy-color-text-default">Load balancing</span></td>
<td style="text-align: left;"><ul>
<li><span class="legacy-color-text-default">round-robin</span></li>
<li><span class="legacy-color-text-default">hash</span></li>
<li><span class="legacy-color-text-default"> header</span></li>
<li><span class="legacy-color-text-default">cookie</span></li>
</ul></td>
<td style="text-align: left;"><ul>
<li><span class="legacy-color-text-default">round-robin, </span></li>
<li><span class="legacy-color-text-default">sticky sessions</span></li>
<li><span class="legacy-color-text-default">weighted-least-request</span></li>
<li><span class="legacy-color-text-default">ring hash</span></li>
<li><span class="legacy-color-text-default">maglev</span></li>
<li><span class="legacy-color-text-default">random</span></li>
</ul></td>
<td style="text-align: left;"><ul>
<li><span class="legacy-color-text-default">round-robin</span></li>
</ul></td>
<td style="text-align: left;"><ul>
<li><span class="legacy-color-text-default">weighted-round-robin</span></li>
<li><span class="legacy-color-text-default">dynamic-round-robin</span></li>
<li><span class="legacy-color-text-default">sticky sessions</span></li>
</ul></td>
<td style="text-align: left;"><ul>
<li><span class="legacy-color-text-default">round-robin,</span></li>
<li><span class="legacy-color-text-default"> least-conn, </span></li>
<li><span class="legacy-color-text-default">least-time, </span></li>
<li><span class="legacy-color-text-default">random</span></li>
<li><span class="legacy-color-text-default">sticky sessions</span></li>
</ul></td>
</tr>
<tr>
<td colspan="2" style="text-align: center;"><span class="legacy-color-text-default">Authentication</span></td>
<td style="text-align: center;"><span class="legacy-color-text-default"> Basic Auth, HMAC, JWT, Key, LDAP, OAuth 2.0, PASETO, plus paid Kong Enterprise options like OpenID Connect</span></td>
<td style="text-align: center;"><span class="legacy-color-text-default">Basic Auth, JWT, Oauth2, external auth (use with any service that implement ext_authz protocol)</span></td>
<td style="text-align: center;"><span class="legacy-color-text-default">Basic, Token, OpenID, HMAC, OAuth 2.0, Custom, mTLS, JWT</span></td>
<td style="text-align: center;"><span class="legacy-color-text-default">Basic, Digest, Forward auth</span></td>
<td style="text-align: center;"><span class="legacy-color-text-default">Basic, External basic, External OAuth</span></td>
</tr>
<tr>
<td colspan="2" style="text-align: center;"><span class="legacy-color-text-default">Base on</span></td>
<td style="text-align: center;"><span class="legacy-color-text-default">kong (nginx)</span></td>
<td style="text-align: center;"><span class="legacy-color-text-default">envoy</span></td>
<td style="text-align: center;"><span class="legacy-color-text-default">Golang - Tyk</span></td>
<td style="text-align: center;"><span class="legacy-color-text-default">traefik</span></td>
<td style="text-align: center;"><span class="legacy-color-text-default">nginx</span></td>
</tr>
<tr>
<td colspan="5" style="text-align: center;"><p><br />
</p>
<p><br />
</p>
<p><br />
<strong>Paid support</strong></p>
<p><br />
</p></td>
<td style="text-align: center;"><p>Have free version</p>
<p>Enterprise version comes with:</p>
<ul>
<li>a built in Dashboard</li>
<li>developer portal</li>
</ul>
<p><a href="https://konghq.com/subscriptions/" class="external-link" rel="nofollow">https://konghq.com/subscriptions/</a></p>
<p><strong>Need to contact for detail pricing</strong></p>
<p><br />
</p></td>
<td style="text-align: center;"><p>Community Edition with <span>free up to 5 RPS per production cluster.</span></p>
<p><span>Enterprise Edge Stack for more than 5RPS per production cluster</span></p>
<p><span><a href="https://www.getambassador.io/editions/" class="external-link" rel="nofollow">https://www.getambassador.io/editions/</a></span></p>
<p><strong><span>Need to contact for detail pricing</span></strong></p></td>
<td style="text-align: center;"><div class="content-wrapper">
<p>Have free version with 1 month POC trial license</p>

![[20502829443-tyk_free_license.PNG]]


<p><br />
</p>
<p><br />
</p>
<p><br />
</p>
<p><br />
</p>
<p><br />
</p>
<p><br />
</p>
<p>Need to purchase to use for business</p>

![[20502829443-tyk_pricing.png]]


</div></td>
<td style="text-align: center;"><p>Traefik itself is a free product.</p>
<p>Enterprise version: TraefikEE, with support for</p>
<ul>
<li> <span>distributed Traefik instances deployment</span></li>
<li><span>JWTauthentication</span></li>
<li><span>LDAP authentication </span></li>
<li><span>oAuth2</span></li>
<li><span>Custom rate limiting</span></li>
<li><span>Distributed rate limiting (per-IP, per-host, per-header)</span></li>
<li><span>Backup &amp; restore</span></li>
</ul>
<p><strong><span>Need to contact for detail pricing</span></strong></p></td>
<td style="text-align: center;"><p>Have free version</p>
<p>Enterprise version comes with additional features under the form of plugins:</p>
<ul>
<li>Real-time metrics</li>
<li>Additional load balancing methods</li>
<li>Session persistence - sticky cookie</li>
<li>Active health checks</li>
<li>JWT validation</li>
</ul>
<p><strong>Need to contact for detail pricing</strong></p></td>
</tr>
</tbody>
</table>

</div>

## **3. Further Details**

<div>

<table style="width: 100.0%;">
<colgroup>
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
</colgroup>
<tbody>
<tr>
<th>Criteria/API GW</th>
<th>Kong</th>
<th>Ambassador</th>
<th><span><a href="http://Tyk.io" class="external-link" rel="nofollow">Tyk.io</a></span></th>
<th>Traefik</th>
</tr>
&#10;<tr>
<td>Deployment</td>
<td><p>Could be intergrated with <strong>kubernetes via yaml file</strong> (all-in-one file provided)</p>
<p>Mapping configuration is done through <strong>Kong Ingress </strong>which extends <strong>kubernetes ingress</strong>, this extension allows for <strong>fine grained API management</strong>.</p>
<p>As ingresses are added, Kong will pick up those rules.</p>
<p><strong>When running on kubernetes, database is optional (headless mode)</strong> to hold configurations as routing configs can be specified inside yaml file and saved in kubernetes control plane.</p></td>
<td><p>Described as a Kubernetes-native API gateway for microservices.</p>
<p>Install with GKE documented <a href="https://www.getambassador.io/docs/latest/topics/running/ambassador-with-gke/" class="external-link" rel="nofollow">here</a></p>
<p>Configuration mapping <span>is defined as Kubernetes Custom Resource Definitions and can be managed using the same workflow as any other Kubernetes resources (e.g., service, deployment)</span></p>
<p><a href="https://www.getambassador.io/docs/latest/topics/using/intro-mappings/" class="external-link" rel="nofollow">https://www.getambassador.io/docs/latest/topics/using/intro-mappings/</a></p></td>
<td><p>Need to deploy Redis before setting up Tyk gateway.</p>
<p>If need to use Dashboard, have to setup MongoDB for analytics storing.</p>
<p>Support for <strong>Kubernetes integration</strong><br />
<a href="https://github.com/TykTechnologies/tyk-kubernetes" class="external-link" rel="nofollow">https://github.com/TykTechnologies/tyk-kubernetes</a></p></td>
<td><p>Could be intergrated with <strong>kubernetes via yaml file</strong></p>
<p>Routing is done through <strong>IngressRoute, a <span>Kubernetes Custom Resource</span></strong></p>
<p><span><a href="https://docs.traefik.io/user-guides/crd-acme/" class="external-link" rel="nofollow">https://docs.traefik.io/user-guides/crd-acme/</a></span></p></td>
</tr>
<tr>
<td><p>Community</p>
<p>support</p></td>
<td><p>Most detailed document</p>
<p>Frequent answers for bugs/questions on different sites and on their own forum</p></td>
<td><p>Detailed document with how-to guide for specific configuration</p>
<p><br />
</p></td>
<td>Detailed and extensive document</td>
<td><p>Detailed official document</p>
<p>Not many results/articles for Traefik as it is quite young</p></td>
</tr>
<tr>
<td>Features</td>
<td><ul>
<li>Load balancing</li>
<li>Rate limiting (with Redis)</li>
<li><p>Basic Auth, HMAC, JWT, Key, LDAP, OAuth 2.0, PASETO, plus paid Kong Enterprise options like OpenID Connect</p></li>
<li>Support for Keycloak via OIDC plugin, TLS, HTTPs, gRPC</li>
<li>Support monitoring/analytics through Dashboard (third party dashboard Konga or native dashboard in Enterprise version)</li>
<li><p><span>Service Mesh Integration with Kuma and Istio</span></p></li>
</ul></td>
<td><ul>
<li>Load Balancer: Load balancing, TLS, protocol support, and Availability.</li>
<li>Kubernetes Integration Ingress support.</li>
<li>API Gateway (Enterprise Edge Stack for &gt; 5RPS per production cluster)
<ul>
<li>Rate Limiting</li>
<li><p>OAuth/OpenID Connect integration<br />
Auth0, Keycloak, Okta, Azure AD, on website</p>
<p>Authentication<br />
Access controls, Multidomain authentication, JWT validation</p></li>
</ul></li>
<li>Full Cycle Development Tooling: Canary releases, Traffic shadowing, Service Preview for API Testing.</li>
<li>3rd Party Integrations:</li>
</ul>
<p>Observability: Prometheus, Datadog, StatsD</p>
<p>Distributed Tracing: Zipkin, Lightstep, Jaeger, Datadog APM</p>
<p>Service Mesh: Istio, Linkerd, Consul</p>
<p><br />
</p></td>
<td><ul>
<li>Load balancing</li>
<li>Rate limiting</li>
<li>Security measures: Bearer Tokens, HMAC, JSON Web Tokens (JWT), Multi Chained Authentication, OAuth 2.0, OpenID Connect. with support for Keycloak</li>
<li>Support monitoring/analytics through Dashboard (paid version)</li>
</ul></td>
<td><ul>
<li>Rate limiting</li>
<li>Load balancing</li>
<li><span class="legacy-color-text-default">Basic, digest and forward auth</span></li>
<li>Automatic HTTPS/TLS support with LetsEncrypt</li>
<li>Support for dashboard</li>
<li>Support for Keycloak via middleware</li>
</ul></td>
</tr>
<tr>
<td>Performance</td>
<td><p>Built on top of NGINX (known for high performance while keeping a small footprint) and Lua (<span>CloudFlare for example is built on the same stack)</span></p>
<p><a href="https://konghq.com/whitepaper/api-microservices-management-benchmark-apigee/" class="external-link" rel="nofollow">https://konghq.com/whitepaper/api-microservices-management-benchmark-apigee/</a></p>
<p><a href="https://discuss.konghq.com/t/api-gateway-comparison-kong-vs-tyk/534/2" class="external-link" rel="nofollow">https://discuss.konghq.com/t/api-gateway-comparison-kong-vs-tyk/534/2</a></p></td>
<td><p>Built on top of Envoy</p>
<p><a href="https://www.getambassador.io/resources/envoyproxy-performance-on-k8s/" class="external-link" rel="nofollow">https://www.getambassador.io/resources/envoyproxy-performance-on-k8s/</a></p></td>
<td><p>Built using Golang</p>
<p><a href="https://searchapparchitecture.techtarget.com/tip/API-gateway-comparison-Kong-vs-Tyk" class="external-link" rel="nofollow">https://searchapparchitecture.techtarget.com/tip/API-gateway-comparison-Kong-vs-Tyk</a></p>
<p><a href="https://www.bbva.com/en/api-gateways-kong-vs-tyk/" class="external-link" rel="nofollow">https://www.bbva.com/en/api-gateways-kong-vs-tyk/</a></p></td>
<td><p>Built using Golang, with average performance compared to nginx and envoy</p>
<p><a href="https://www.loggly.com/blog/benchmarking-5-popular-load-balancers-nginx-haproxy-envoy-traefik-and-alb/" class="external-link" rel="nofollow">https://www.loggly.com/blog/benchmarking-5-popular-load-balancers-nginx-haproxy-envoy-traefik-and-alb/</a></p></td>
</tr>
<tr>
<td>Pricing</td>
<td><p>Have free version</p>
<p>Enterprise version comes with a built in Dashboard, Developer potal</p></td>
<td><p>Community Edition with <span>free up to 5 RPS per production cluster.</span></p>
<p><span>Enterprise Edge Stack for &gt; 5RPS per production cluster</span></p>
<p>See more: </p>
<p><a href="https://www.getambassador.io/editions/" class="external-link" rel="nofollow">https://www.getambassador.io/editions/</a></p>
<p><a href="https://www.getambassador.io/contact/" class="external-link" rel="nofollow">https://www.getambassador.io/contact/</a></p></td>
<td><div class="content-wrapper">
<p>Have free version with 1 month POC trial license</p>

![[20502829443-tyk_free_license.PNG]]


<p>Need to purchase to use for business</p>

![[20502829443-tyk_pricing.png]]


</div></td>
<td><div class="content-wrapper">
<p>Traefik itself is a free product.</p>
<p>Enterprise version: TraefikEE, with support for <span>distributed Traefik instances deployment</span></p>
<p><span>

![[20502829443-compare.PNG]]

</span></p>
</div></td>
</tr>
<tr>
<td>Popularity</td>
<td><p>Traditional API GW Founded in 2009, Ingress Controller for kubernetes introduced in 2018 (changelog on github)</p>
<p>Used by Fujitsu, Giphy, Yahoo, Karhoo, Digicel, Cargrill, Just Eat...</p></td>
<td><p>Releases changelog on github dated back to 2017</p>
<p>Used by <span>Chick-Fil-A, ADP, Microsoft, NVidia, AppDirect</span></p></td>
<td><p>Founded in 2014</p>
<p>Used by: SK (in South Korea), Chợ Tốt (VietNam), PA Digital (Spain), true (Thailand), Coliquio (Germany)</p></td>
<td><p>Founded in 2016</p>
<p>Trusted by: Mozilla, Apple, Bose, Bloomberg</p></td>
</tr>
</tbody>
</table>

</div>

  

  

References:  
<a href="https://medium.com/@mahesh.mahadevan/my-experiences-with-api-gateways-8a93ad17c4c4" class="external-link" rel="nofollow">https://medium.com/@mahesh.mahadevan/my-experiences-with-api-gateways-8a93ad17c4c4</a>

%% ai-graph-start %%

**Related notes:**
- [[API Gateway Evaluation Discussion]]
- [[System architecture]]
- [[Fix evaluation criteria before looking at candidates, and say which one you weight]]
- [[GKE Kubernetes Gateway API]]
- [[Infrastructure]]

%% ai-graph-end %%