---
title: "SAML documentation"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/Arrow/pages/48186654802/SAML+documentation
space: "Arrow"
topic: security
relevance: 0.701
depth: 2.5
updated: 2024-12-05
attachments: 9
tags:
  - confluence
  - security
  - space/arrow
---

# SAML documentation

> [!info] Imported from Confluence
> Space **Arrow** · updated 2024-12-05 · [open original](https://axonivy.atlassian.net/wiki/spaces/Arrow/pages/48186654802/SAML+documentation)
> Relevance 0.701 · topic `security`

<div class="toc-macro client-side-toc-macro conf-macro output-block" cssliststyle="none" hasbody="false" headerelements="H1,H2,H3,H4,H5,H6,H7" macro-id="787a3b36-926c-4dfe-bc30-cc61933adf0b" macro-name="toc">

</div>

# SAML definition

- Security Assertion Markup Language (SAML) is an open standard for transferring authorization information between identity providers (IdPs) and service providers (SPs).

- This XML-based protocol allows users to use a single set of credentials to access multiple applications.

- Thus, SAML helps realize single sign-on (SSO) technology, where users can access multiple applications or web services without using separate credentials for each service.

# 3 entities in saml flow

- **The principal** is the entity that initiates the request to access resources, the user agent (Ex: Liva and her browser).

- **The Identity Provider** (**IdP**) is the entity that verifies the identity of a user, generating SAML assertions that contain identity information during a typical SSO process. (can be keycloak in our finform side in this case, this is to be implemented, not available).

- **The Service Provider** (**SP**) is the entity that authorizes the user to access the required resource. In SAML-based SSO, the service provider (SP) is the entity that provides the resources that users want to access. The SP integrates with the IdP to facilitate the SSO process. The SP also builds a trusted relationship with the IdP. (Fidentity in this case).

# The behavior we want to achieve

<div>

|  |  |
|----|----|
| **Now** | **What we want** |
| 

![[48186654802-image-20241202-065936.png]]

 | 

![[48186654802-image-20241202-070110.png]]

 |

</div>

For now, the user of routing finform is different from the user of fidentity, for example:

- finform user: liv.servan

- fidentity user: liv.servan@finform.ch

So to make the SSO work, we have to centralize and trust just one source of the user, either finform mongo db or fidentity’s user database.

- It mean fidentity have to trust our IdP (keycloak we will implement in our side)

- OR we trust IdP from fidentity (fidentity’s user database from their side)

It is delegate authentication.

# Diagram flow for what we want

Because we log in into our routing tool first, so we will use IdP initial flow. (the other flow is SP initial flow)


![[48186654802-image-20241202-070947.png]]



The data IdP and SP exchange to integrate:

<div>

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr>
<th><p><strong>IdP</strong></p></th>
<th><p><strong>SP</strong></p></th>
</tr>
&#10;<tr>
<td>

![[48186654802-image-20241202-081927.png]]

</td>
<td>

![[48186654802-image-20241202-082245.png]]

</td>
</tr>
<tr>
<td><p>xml metadata example:</p>

![[48186654802-image-20241202-082031.png]]

</td>
<td><p>xml metadata example:</p>

![[48186654802-image-20241202-082226.png]]

</td>
</tr>
</tbody>
</table>

</div>

# Demo with keycloak

- Keycloak IdP (finform side): <a href="http://host.docker.internal:10999/realms/IdPFinform/account/" class="external-link" rel="nofollow">http://host.docker.internal:10999/realms/IdPFinform/account/</a> (simulate routing tool).

- Routing tool front end dashboard: <a href="http://localhost:4200/dashboard" class="external-link" rel="nofollow">http://localhost:4200/dashboard</a>

- Keycloak SP (fidentity): <a href="http://host.docker.internal:8080/realms/SPFidentity/account/#/personal-info" class="external-link" rel="nofollow">http://host.docker.internal:8080/realms/SPFidentity/account/#/personal-info</a> (simulate when click on task in routing tool front end, open this link in fidentity).

# What should each sides should do for ready if we implement

## Current and after integrate keycloak:


![[48186654802-routing now and with keycloak integrate.png]]



## Fidentity:

Their system should already have ability to add a new IdP because when we ready, their side also can integrate with us, but if their side not support and have to implement from scratch then both side have to wait.

## Finform (us):

### If we choose to use keycloak as saml integrate solution:

- Implement keycloak to do the role IdP.

- Migrate existing users collection from mongodb to postgresql db.

- Adapt the code of the routing tool FE (angular) and BE (nodejs) to integrate with keycloak.

### If we implement saml from scratch with nodejs and angular:

- Can keep using mongodb, but implement everything that an IdP provide like keycloak (nodejs and angular)

# Tryout

cob-routing: <a href="https://scm.axonfintech.io/cob/cob-routing/compare/master...ARROW/FA-1608_tryout_integrate" class="external-link" rel="nofollow">https://scm.axonfintech.io/cob/cob-routing/compare/master...ARROW/FA-1608_tryout_integrate</a>
