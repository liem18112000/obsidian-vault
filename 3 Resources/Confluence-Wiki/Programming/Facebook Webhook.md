---
ai_hash: 777ef6588f402cc2
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 3
depth: 3
entities: []
relevance: 0.792
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20502828293/Facebook+Webhook
space: LUZ
status: reference
tags:
- confluence
- programming
- space/luz
title: Facebook Webhook
topic: programming
type: source
updated: 2020-03-19
---

# Facebook Webhook

> [!info] Imported from Confluence
> Space **LUZ** · updated 2020-03-19 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20502828293/Facebook+Webhook)
> Relevance 0.792 · topic `programming`

## Step to enable webhook on face book app.

- Add the Webhook product to your Facebook app. 
- Select the object from dropdown  menu you want to subscribe   (ex .user,page,permission) 

  

 

![[20502828293-image2020-3-19_12-3-37.png]]



- Click on Superscribe to this object.


![[20502828293-image2020-3-19_12-7-13.png]]



- In the 'Callback URL' field, enter the public URL for your webhook.
- In the 'Verify Token' field, enter the verify token for your webhook.
- click on Verify and save.

**Verify Token **: This is a random string of your choosing, hardcoded into your webhook. The Facebook Platform sends a `GET` request to your webhook with the token in the `hub.verify` parameter of the query string.

Reference : <a href="https://developers.facebook.com/docs/messenger-platform/getting-started/webhook-setup/" class="external-link" rel="nofollow">https://developers.facebook.com/docs/messenger-platform/getting-started/webhook-setup/</a>

If your subscription is on page then the  subscribe the filed from page. Description of fields is on <a href="https://developers.facebook.com/docs/graph-api/webhooks/reference/page/" class="external-link" rel="nofollow">https://developers.facebook.com/docs/graph-api/webhooks/reference/page/</a> 


![[20502828293-screencapture-developers-facebook-apps-1526700620813857-webhooks-2020-03-16-11_2.png]]



  

**note : **Applications will only be able to receive test webhooks sent from the app dashboard while they are in development. No production data, including that of app admins, developers, and testers, will be delivered unless the app is live.

%% ai-graph-start %%

**Related notes:**
- _(none above threshold)_

%% ai-graph-end %%