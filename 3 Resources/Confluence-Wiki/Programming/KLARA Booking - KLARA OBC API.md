---
ai_hash: 6d8343b40512a5e9
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 0
depth: 2.73
entities: []
relevance: 0.731
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20490592551/KLARA+Booking+-+KLARA+OBC+API
space: LUZ
status: reference
tags:
- confluence
- programming
- space/luz
title: KLARA Booking - KLARA OBC API
topic: programming
type: source
updated: 2019-06-25
---

# KLARA Booking - KLARA OBC API

> [!info] Imported from Confluence
> Space **LUZ** · updated 2019-06-25 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20490592551/KLARA+Booking+-+KLARA+OBC+API)
> Relevance 0.731 · topic `programming`

`/*==============================================`  
`            ``Klara Booking API            `  
`=============================================*/`  
`-------------------------------------------------------------------------------`  
`EXAMPLE CALL URL : https:``//`<a href="http://dev.klara-vcard.ch/vcard_rest/public/api/klarabooking/store" class="external-link" rel="nofollow"><code class="sourceCode javascript" style="text-align: left;">dev<span class="op">.</span><span class="at">klara</span><span class="op">-</span>vcard<span class="op">.</span><span class="at">ch</span><span class="op">/</span>vcard_rest<span class="op">/</span><span class="kw">public</span><span class="ss">/api/klarabooking/store</span></code></a>  
`-------------------------------------------------------------------------------`  
   
`CALL METHOD : POST`  
`-------------------------------------------------------------------------------`  
   
`JSON PAYLOAD SKELETON :`  
`-------------------------------------------------------------------------------`  
`{`  
`    ``"klara_booking_email"``:``"avdevs@`<a href="http://gmail.com" class="external-link" rel="nofollow"><code class="sourceCode javascript" style="text-align: left;">gmail<span class="op">.</span><span class="at">com</span></code></a>`"``,`  
`    ``"company_name"``:``"AV DEVS Solutions Testing"``,`  
`    ``"worker_uuid"``:``"1c9cdba0-16ac-4b48-a03d-f379ffe2087b"``,`  
`    ``"subscription_type_id"``:``"1"``,`  
`}`  
   
   
`DESCRIPTION OF JSON PAYLOAD :`  
`-------------------------------------------------------------------------------`  
` ``=> klara_booking_email  : contains klara user email address / email from the customer`  
` ``=> company_name : contains name of company`  
` ``=> worker_uuid      : contain alphanumeric string, the uuid which will be different``for``every booking script`  
` ``=> subscription_type_id: contains integer``for``Calenso/KLARA Booking Suscription types`  
   
   
`-------------------------------------------------------------------------------`  
`FAILURE/ERROR RESPONSES (Allow only POST method) :`  
`-------------------------------------------------------------------------------`  
`{`  
`    ``"status"``:``"Error"``,`  
`    ``"status_code"``: 405,`  
`    ``"result"``: [`  
`        ``"Method not allowed."`  
`    ``]`  
`}`  
   
`-------------------------------------------------------------------------------`  
`FAILURE/ERROR RESPONSES (Check IP security) :`  
`-------------------------------------------------------------------------------`  
`{`  
`    ``"status"``:``"Error"``,`  
`    ``"status_code"``: 401,`  
`    ``"result"``: [`  
`        ``"IP not authorized to make request."`  
`    ``]`  
`}`  
   
`-------------------------------------------------------------------------------`  
`FAILURE/ERROR RESPONSES (If empty POST) :`  
`-------------------------------------------------------------------------------`  
`{`  
`    ``"status"``:``"Error"``,`  
`    ``"status_code"``: 400,`  
`    ``"result"``: [`  
`        ``"No data received in post request."`  
`    ``]`  
`}`  
   
`-------------------------------------------------------------------------------`  
`FAILURE/ERROR RESPONSES (POST data vaidation) :`  
`-------------------------------------------------------------------------------`  
`{`  
`    ``"status"``:``"Error"``,`  
`    ``"status_code"``: 400,`  
`    ``"result"``: [`  
`        ``"klara_booking_email field required."``,`  
`        ``"klara_booking_email field is not valid email address."``,`  
`        ``"company_name field required."``,`  
`        ``"worker_uuid field required."`  
`    ``]`  
`}`  
   
`-------------------------------------------------------------------------------`  
`FAILURE/ERROR RESPONSES (Company not found``in``database) :`  
`-------------------------------------------------------------------------------`  
`{`  
`    ``"status"``:``"Error"``,`  
`    ``"status_code"``: 503,`  
`    ``"result"``: [`  
`        ``"Company does not exist."`  
`    ``]`  
`}`  
   
`-------------------------------------------------------------------------------`  
`SUCCESS RESPONSES (Company exist but OBC not exist``with``requested emainId. obcgenerated status = 0) :`  
`-------------------------------------------------------------------------------`  
`{`  
`    ``"status"``:``"OK"``,`  
`    ``"status_code"``: 200,`  
`    ``"result"``: [`  
`        ``"OBC not generated yet..!"``,`  
`        ``"Requested data store into cold storage."`  
`    ``]`  
`}`  
   
`-------------------------------------------------------------------------------`  
`SUCCESS RESPONSES (OBC exist(obcgenerated status = 1)``with``emailId. but Still booking status is no = 0) :`  
`-------------------------------------------------------------------------------`  
`{`  
`    ``"status"``:``"OK"``,`  
`    ``"status_code"``: 200,`  
`    ``"result"``: [`  
`        ``"Booking not available yet..!"``,`  
`        ``"Store booking script successfully."`  
`    ``]`  
`}`  
   
`-------------------------------------------------------------------------------`  
`SUCCESS RESPONSE : (``if``company exist``with``klara user emailId and``if``obcgenerated status = 1 and booking status = 1)`  
`-------------------------------------------------------------------------------`  
`{`  
`    ``"status"``:``"OK"``,`  
`    ``"status_code"``: 200,`  
`    ``"result"``: [`  
`        ``"Store booking script successfully."``,`  
`        ``"OBC generated successfully."`  
`    ``]`  
`}`

%% ai-graph-start %%

**Related notes:**
- [[How to use Public API to create update KLARA Business Company]]
- [[LUZ-109076 Public API - Create update new tenant (implementation)]]
- [[KLARA Integration (request access token & call API)]]
- [[Login]]
- [[LUZ-110826 Public API - Widget subscription by activation code]]

%% ai-graph-end %%