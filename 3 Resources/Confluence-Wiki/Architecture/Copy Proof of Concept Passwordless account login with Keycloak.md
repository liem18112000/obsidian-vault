---
title: "Copy [Proof of Concept] Passwordless account/login with Keycloak"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/49307550863/Copy+Proof+of+Concept+Passwordless+account+login+with+Keycloak
space: "LUZ"
topic: architecture
relevance: 0.711
depth: 2.44
updated: 2026-04-08
attachments: 20
tags:
  - confluence
  - architecture
  - space/luz
---

# Copy [Proof of Concept] Passwordless account/login with Keycloak

> [!info] Imported from Confluence
> Space **LUZ** · updated 2026-04-08 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/49307550863/Copy+Proof+of+Concept+Passwordless+account+login+with+Keycloak)
> Relevance 0.711 · topic `architecture`

### I. Business Requirements

EPost Forced Onboarding users have a request to be able to receive onboarding documents **without** downloading the ePost App and going through registration process

Use case:

1.  User receives the onboarding request email. The onboarding request email includes an option to see the delivery without registering (link).

2.  The user clicks on the link (option to see the delivery without registering) → Keycloak

3.  Keycloak creates a new user without a password

4.  Keycloak shows the user a button to send the OTP Link to his email address

5.  The user receives the OTP link in his email client

6.  The user clicks on the OTP link

7.  The user is logged in to his passwordless account

8.  Onboarding process for the new individual tenant → Creating new individual tenant (incl. accepting T&C…)

9.  Trigger delivery of the Forced Onboarding eLetter to the new tenant (as new tenant is now fully created)

10. The user can see the eLetter in the digtial letterbox

**Note: For the purpose of this research, only the configuration and demonstration of Passwordless login will be covered**

**This POC is using open-source** <a href="https://github.com/p2-inc" class="external-link" rel="nofollow"><strong>p2-inc</strong></a>**/**<a href="https://github.com/p2-inc/keycloak-magic-link" class="external-link" rel="nofollow"><strong>keycloak-magic-link</strong></a> **as a dependency.**

### II. Keycloak Configuration

**Step 1: Start simple Keycloak service:** <a href="https://www.keycloak.org/getting-started/getting-started-zip" class="external-link" data-card-appearance="inline" rel="nofollow">https://www.keycloak.org/getting-started/getting-started-zip</a>

**Step 2: Add MagicLink extension into keycloak providers, restart service and check for successful sign:**


![[49307550863-image-20240917-074247.png]]



**Step 3: Create keycloak user and login**


![[49307550863-image-20240917-074446.png]]



**Step 4: Create Klara realm**


![[49307550863-image-20240917-074630.png]]



**Step 5: Create Passwordless authentication flow**


![[49307550863-image-20240917-075106.png]]



**Step 6: Config Passwordless support browser flow as below.**


![[49307550863-image-20240917-112914.png]]



Passwordless authentication flow should look like this:


![[49307550863-image-20240917-091815.png]]



**Step 7: Create new client named onboarding**

Note: Use the below file to create: <span class="confluence-embedded-file-wrapper conf-macro output-inline" hasbody="false" macro-id="c8dbbc44-1e3c-43ce-91f7-18f9b32771ad" macro-name="view-file"><a href="../_attachments/49307550863-onboarding.json" class="confluence-embedded-file" data-nice-type="null" data-file-src="/wiki/download/attachments/49307550863/onboarding.json?version=1&amp;modificationDate=1775630531176&amp;cacheVersion=1&amp;api=v2" data-mime-type="application/json" data-has-thumbnail="true">

![[49307550863-onboarding.json]]

</a></span>


![[49307550863-image-20240917-091933.png]]



**Step 8: Config this new onboarding client to use passwordless authenticate flow**


![[49307550863-image-20240917-092314.png]]




![[49307550863-image-20240917-092351.png]]



### III. Demonstration

<span class="confluence-embedded-file-wrapper image-center-wrapper confluence-embedded-manual-size">[[49307550863-scrnli_9_17_2024_4-46-10 PM.mp4|scrnli_9_17_2024_4-46-10 PM.mp4]]</span>


![[49307550863-image-20240917-095008.png]]



### IV/ Open questions:

1.  What are some potential security issues? (Investigating)

2.  Should this authentication flow be applied for all Keycloak clients or create a specific client to support our uses cases? (Investigating)
