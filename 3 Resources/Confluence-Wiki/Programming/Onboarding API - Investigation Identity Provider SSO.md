---
title: "Onboarding API - Investigation Identity Provider SSO"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/TS/pages/47642247207/Onboarding+API+-+Investigation+Identity+Provider+SSO
space: "TS"
topic: programming
relevance: 0.729
depth: 2.49
updated: 2024-04-19
attachments: 4
tags:
  - confluence
  - programming
  - space/ts
---

# Onboarding API - Investigation Identity Provider SSO

> [!info] Imported from Confluence
> Space **TS** · updated 2024-04-19 · [open original](https://axonivy.atlassian.net/wiki/spaces/TS/pages/47642247207/Onboarding+API+-+Investigation+Identity+Provider+SSO)
> Relevance 0.729 · topic `programming`

SSO for ePost (jump from the migration portal via a link to the ePost digital mailbox):

Based on OpenID Connect using a refresh token, which is valid for 45 days. This can then be used to create an access token, which then authenticates the user when they leave.

Both the design of the link to the jump and the management of the refresh token must be specified together.

**Detail implementation here:** [Identity Provider Configuration](https://axonivy.atlassian.net/wiki/spaces/TS/pages/47726657610/Identity+Provider+Configuration+DEV)

--

Draft idea:  
**1. OpenID Connect Federation (OIDC)**

This approach leverages OpenID Connect (OIDC) federation features in Keycloak to authenticate users with the Canton Bern Portal and transfer their identity to our system. Here's how it works:

- Configure OIDC client in Keycloak: Register Canton Bern Portal as an OIDC client in Keycloak. This involves providing Canton Bern Portal's login URL, redirect URI, and other details.

- Map user attributes: Configure user attribute mapping between the Canton Bern Portal and our system. This ensures matching user identities even if usernames differ.

- User logs in to Canton Bern Portal: The user authenticates through Canton Bern Portal's login portal.

- Canton Bern Portal sends an OIDC token to Keycloak: After successful login, Canton Bern Portal sends an OIDC token to Keycloak containing user information.

- Canton Bern Portal redirects to Keycloak: When a user tries to access our service through Canton Bern Portal, they are redirected to Keycloak for authentication.

- Keycloak authenticates and issues token: Keycloak validates the OIDC token from Canton Bern Portal and authenticates the user. **<span class="inline-comment-marker" ref="10aea10e-de4c-4737-8b56-77e8b38b198e">If the user is not present in our system, we can create a new user based on the mapped attributes.</span>** Keycloak then issues an access token for our API to the user.

- Redirect back to Canton Bern Portal with token: <span class="inline-comment-marker" ref="83e19ba3-88b9-429e-8ae6-cf9b1e844bb4">The user is redirected back to Canton Bern Portal with our API access token embedded in the URL or response.</span>

- Canton Bern Portal uses the token to call API: Canton Bern Portal can then use the received access token to call our public API on behalf of the user.

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="2363fb40-8234-4b46-97b2-5144ec9c369e" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
Start -> Canton Bern redirects to Keycloak
    | No user in your system?
    |   Yes -> Create user based on Canton Bern attributes
    | End
  | User logs in to Canton Bern
  | Canton Bern sends OIDC token to Keycloak
  | Keycloak validates token and authenticates user
  | Keycloak issues access token
  | Redirect back to Canton Bern with token
  | Canton Bern passes token to your application
  | Your application verifies token
  | (Optional) Your application retrieves user information
  | Your application grants access
  | User accesses your service's resources
  End
```

</div>

</div>

------------------------------------------------------------------------

**Context:**

- The user already has an account at Canton Bern

- The user may or may not have an account at KLARA/ePost with the same username (email)

- → If no, create an ePost (individual) tenant for them to use a limited Digital Letterbox

- → If yes, their account may have \> 1 tenant → Perform match, matching rule TBD

- Users only use Digital Letterbox functionality from the Canton Bern Portal (GUI is a working-in-progress from 3rd party), internally they will use an access token provided by the KLARA system to make calls to KLARA Public API for those functionality

- Sometime after they successfully log in with KLARA/ePost system through Canton Bern Portal (1 day?), we will send a promotion email and a link for them to allow them promoted to official ePost user → which means fully registered (username & password) and allow login directly at ePost/KLARA and use full Digital Letterbox.

Possible cases happen when attempting to create a user account & tenant inside KLARA/ePost when they using Canton Bern Portal:

<div>

<table>
<colgroup>
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
</colgroup>
<tbody>
<tr>
<th></th>
<th><p><strong>Use case</strong></p></th>
<th><p><strong>Idea?</strong></p></th>
<th><p><strong>Note</strong></p></th>
</tr>
&#10;<tr>
<td>1</td>
<td><p>User start from Canton Bern Portal:<br />
- User has account on Bern with email A<br />
- User don’t have account on ePost/KLARA</p></td>
<td><ul>
<li><p>Allow login with <strong>User Federation</strong><br />
-&gt; Add callback to create individual tenant &amp; limit their permission to limited version of Digital Letterbox?</p></li>
<li><p>Promotion email to win them as ePost user → If click on link → Redirect to setup password screen</p></li>
</ul></td>
<td></td>
</tr>
<tr>
<td>2</td>
<td><p>User start from Canton Bern Portal:</p>
<ul>
<li><p>User has account on Bern with email A</p></li>
<li><p>User have account on ePost/KLARA with email A - 1 individual tenant</p></li>
</ul></td>
<td><ul>
<li><p>Matching based on user attributes include in OIDC token</p></li>
<li><p>These user will have full functionality of Digital Letterbox (TBD - only when login = ePost/KLARA page?)</p></li>
</ul></td>
<td></td>
</tr>
<tr>
<td>3</td>
<td><p>User start from Canton Bern Portal:</p>
<ul>
<li><p>User has account on Bern with email A</p></li>
<li><p>User have account on ePost/KLARA with email A - 2+ tenants (can be any combination)</p></li>
</ul></td>
<td><ul>
<li><p>Matching based on user attributes include in OIDC token</p></li>
<li><p>Match with 1st individual tenant found (prefer match with Individual than Business?)</p></li>
<li><p>These user will have full functionality of Digital Letterbox (TBD - only when login = ePost/KLARA page?)</p></li>
</ul></td>
<td></td>
</tr>
<tr>
<td>4</td>
<td></td>
<td></td>
<td></td>
</tr>
</tbody>
</table>

</div>

Docs: <a href="https://www.keycloak.org/docs/latest/server_admin/#_identity_broker" class="external-link" data-card-appearance="inline" rel="nofollow">https://www.keycloak.org/docs/latest/server_admin/#_identity_broker</a>

Create an Identity Provider correspondence with Canton Bern login Portal


![[47642247207-image-20240219-082139.png]]

![[47642247207-image-20240219-082339.png]]



Create a new (public/confidential)? Client for Canton Bern, grant them the corresponding scope


![[47642247207-image-20240219-082608.png]]

![[47642247207-image-20240219-082637.png]]
