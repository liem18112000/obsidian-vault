---
ai_hash: 74f4baa2a0a45405
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 20
depth: 2.44
entities: []
relevance: 0.711
source: https://axonivy.atlassian.net/wiki/spaces/HACKA/pages/48039723268/Proof+of+Concept+Passwordless+account+login+with+Keycloak
space: HACKA
status: reference
tags:
- confluence
- architecture
- space/hacka
title: '[Proof of Concept] Passwordless account/login with Keycloak'
topic: architecture
type: source
updated: 2024-09-17
---

# [Proof of Concept] Passwordless account/login with Keycloak

> [!info] Imported from Confluence
> Space **HACKA** · updated 2024-09-17 · [open original](https://axonivy.atlassian.net/wiki/spaces/HACKA/pages/48039723268/Proof+of+Concept+Passwordless+account+login+with+Keycloak)
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


![[48039723268-image-20240917-074247.png]]



**Step 3: Create keycloak user and login**


![[48039723268-image-20240917-074446.png]]



**Step 4: Create Klara realm**


![[48039723268-image-20240917-074630.png]]



**Step 5: Create Passwordless authentication flow**


![[48039723268-image-20240917-075106.png]]



**Step 6: Config Passwordless support browser flow as below.**


![[48039723268-image-20240917-112914.png]]



Passwordless authentication flow should look like this:


![[48039723268-image-20240917-091815.png]]



**Step 7: Create new client named onboarding**

Note: Use the below file to create: <span class="confluence-embedded-file-wrapper conf-macro output-inline" hasbody="false" macro-id="c8dbbc44-1e3c-43ce-91f7-18f9b32771ad" macro-name="view-file"><a href="../_attachments/48039723268-onboarding.json" class="confluence-embedded-file" data-nice-type="null" data-file-src="/wiki/download/attachments/48039723268/onboarding.json?version=2&amp;modificationDate=1726565793904&amp;cacheVersion=1&amp;api=v2" data-mime-type="application/json" data-has-thumbnail="true">

![[48039723268-onboarding.json]]

</a></span>


![[48039723268-image-20240917-091933.png]]



**Step 8: Config this new onboarding client to use passwordless authenticate flow**


![[48039723268-image-20240917-092314.png]]




![[48039723268-image-20240917-092351.png]]



### III. Demonstration

<span class="confluence-embedded-file-wrapper image-center-wrapper confluence-embedded-manual-size">[[48039723268-scrnli_9_17_2024_4-46-10 PM.mp4|scrnli_9_17_2024_4-46-10 PM.mp4]]</span>


![[48039723268-image-20240917-095008.png]]



### IV/ Open questions:

1.  What are some potential security issues? (Investigating)

2.  Should this authentication flow be applied for all Keycloak clients or create a specific client to support our uses cases? (Investigating)

%% ai-graph-start %%

**Related notes:**
- [[Copy Proof of Concept Passwordless account login with Keycloak]]
- [[Onboarding API - Investigation Identity Provider SSO]]
- [[Proof of Concept Auto login with Keycloak]]
- [[Understanding Keycloak Authorization Code flow]]
- [[Keycloak action tokens bridge an app session into a browser login]]

%% ai-graph-end %%