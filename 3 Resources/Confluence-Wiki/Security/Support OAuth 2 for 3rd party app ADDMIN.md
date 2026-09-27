---
title: "Support OAuth 2 for 3rd party app ADDMIN"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/Arrow/pages/47032271178/Support+OAuth+2+for+3rd+party+app+ADDMIN
space: "Arrow"
topic: security
relevance: 0.701
depth: 2.5
updated: 2025-03-18
attachments: 19
tags:
  - confluence
  - security
  - space/arrow
---

# Support OAuth 2 for 3rd party app ADDMIN

> [!info] Imported from Confluence
> Space **Arrow** · updated 2025-03-18 · [open original](https://axonivy.atlassian.net/wiki/spaces/Arrow/pages/47032271178/Support+OAuth+2+for+3rd+party+app+ADDMIN)
> Relevance 0.701 · topic `security`

# Context


![[47032271178-image-20220228-070644.png]]

![[47032271178-image-20220228-073212.png]]



Currently, the 3rd app/webapp (Addmin) requests its users to provide its KLARA credentials to then manage their KLARA documents inside the apps via our public APIs. There are several problems here:

1.  The authentication/authorization process with KLARA account to KLARA system handling by the apps itself. Then, there is a security issue here that the apps might be having the KLARA credentials of its users. Then, users trust them less.

2.  Our KLARA refresh token has short living time. Then, the apps have to request its users to input KLARA credentials very frequently. it leads to the inconvenience to its users.

  
That the reason why the 3rd app (Addmin) wants Klara to support OAuth in order for it to have their end-users who **already have Klara account** to login and manage documents from Klara within the Addmin app with their trust and in the convenient way.

# Desired flow


![[47032271178-image-20220112-033314.png]]



<span class="inline-comment-marker" ref="03c5b555-47a2-493a-8cfc-ef9da835a412">Note</span>:

- End-user has his/her own account in the Addmin system

- Our Klara here might be one of it’s providers

# Solutions/Options

- Initialize refresh token and access token


![[47032271178-init refresh token and access token.png]]



- API calling  


![[47032271178-calling api.png]]



- Renew access token  


![[47032271178-api calling.png]]



# References

<span class="confluence-jim-macro jira-issue conf-macro output-block" client-id="SINGLE_d3f195c5-8684-3b17-b4f6-e9ee3a0b0fe2_47032271178_712020:87b0f7f1-aaab-4406-a25d-fa0fc075c4d4" hasbody="false" jira-key="LUZ-69044" macro-id="0690b836-11aa-4732-8308-0482d82cf4fb" macro-name="jira"> <a href="https://axonivy.atlassian.net/browse/LUZ-69044" class="jira-issue-key"><span class="aui-icon aui-icon-wait issue-placeholder"> </span>LUZ-69044</a> - <span class="summary">Getting issue details...</span> <span class="aui-lozenge aui-lozenge-subtle aui-lozenge-default issue-placeholder">STATUS</span> </span>

# [\[myKLARA\] app flows and offline sessions](https://axonivy.atlassian.net/wiki/spaces/Helios/pages/47009628801/myKLARA+app+flows+and+offline+sessions)
