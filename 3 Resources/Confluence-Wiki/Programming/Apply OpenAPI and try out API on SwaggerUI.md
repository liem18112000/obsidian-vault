---
ai_hash: b5ffa3ffd1256f1b
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 2
depth: 2.84
entities: []
relevance: 0.804
source: https://axonivy.atlassian.net/wiki/spaces/Helios/pages/47057600521/Apply+OpenAPI+and+try+out+API+on+SwaggerUI
space: Helios
status: reference
tags:
- confluence
- programming
- space/helios
title: Apply OpenAPI and try out API on SwaggerUI
topic: programming
type: source
updated: 2022-02-15
---

# Apply OpenAPI and try out API on SwaggerUI

> [!info] Imported from Confluence
> Space **Helios** · updated 2022-02-15 · [open original](https://axonivy.atlassian.net/wiki/spaces/Helios/pages/47057600521/Apply+OpenAPI+and+try+out+API+on+SwaggerUI)
> Relevance 0.804 · topic `programming`

To prepare parameters for API and run on SwaggerUI, please follow this PR below:  
<a href="https://bitbucket.org/axonivy-prod/luz_notification_center/pull-requests/75/helios-luz-70951-wildfly21-migration" class="external-link" data-card-appearance="inline" rel="nofollow">https://bitbucket.org/axonivy-prod/luz_notification_center/pull-requests/75/helios-luz-70951-wildfly21-migration</a>

Follow the step below to try out APIs on SwaggerUI

1\. Run this command on command line:

- kubectl port-forward service/luz-api-ui 8080:8080 -n dev-vn

- kubectl port-forward service/luz-notification-center 8083:8080 -n dev-vn

(open new window for each command)

2\. open URL:

<a href="http://localhost:8080/luz-api-ui/#/%2Fnotifications-center/delete_api_notifications_center_revoke" class="external-link" rel="nofollow">http://localhost:8080/luz-api-ui/</a>

3\. search with value:

<a href="http://localhost:8083/luz_notification_center/api/openapi" class="external-link" rel="nofollow">http://localhost:8083/luz_notification_center/api/openapi</a>

4\. Put the token into the lock icon (icon on the top right for each API)

5\. For API register click button “Try it out” and try with following request body:

{     "deviceToken": "eBCYuj68skRgvCdEASur9g:APA91bEnE4P7zy3BNfmm6jpTJyyaRIEdtLiKGhy34qUPQJltcrmV076p82n-vHyRwVNh-FXZJyHPQw5BD_9IAEFcFPLNO6Z5rQi9v8qYSc30I9z5bjIB7Pi9DUeMuWW2ppEo0ziIba7B",     "userName": "<a href="mailto:hcmc-helios@axonactive.com" class="external-link" rel="nofollow">hcmc-helios@axonactive.com</a>",     "platform": "ios",    <span rel="nofollow"> "projectName": "myklara" }</span>

 


![[47057600521-21070ee2-38aa-4c55-9c28-11c0126d60be#media-blob-url=true&id=bf9600d9-b7ba-499a-9.png]]



6\. Click button “execute” and see the response.

Note: if the response throw exception with CORS issue please do the step below

1.  Turn off the browser

2.  Disable web security:

- Add this command on the target of the browser shortcut: (Ex: Chrome)  
  --disable-web-security --disable-gpu --user-data-dir=~/chromeTemp

- 

![[47057600521-3c7c3991-c56e-4498-aa88-30f20213698c#media-blob-url=true&id=35f2f65e-6e82-4c62-8.png]]



3\. Open the browser again and try again.

%% ai-graph-start %%

**Related notes:**
- [[OpenAPI UI (API on SwaggerUI)]]
- [[Swagger UI]]
- [[Microprofile OpenAPI config]]
- [[Port Forward to call GCP API in localhost]]
- [[Swagger with api explorer]]

%% ai-graph-end %%