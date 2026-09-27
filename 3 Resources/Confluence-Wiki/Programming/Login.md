---
ai_hash: 0d7db3adfe8b335c
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 0
depth: 3
entities: []
relevance: 0.773
source: https://axonivy.atlassian.net/wiki/spaces/Helios/pages/38168508503/Login
space: Helios
status: reference
tags:
- confluence
- programming
- space/helios
title: Login
topic: programming
type: source
updated: 2018-10-01
---

# Login

> [!info] Imported from Confluence
> Space **Helios** · updated 2018-10-01 · [open original](https://axonivy.atlassian.net/wiki/spaces/Helios/pages/38168508503/Login)
> Relevance 0.773 · topic `programming`

First log a user in and then return a user's list of tenants.

### **I. Endpoint **

{protocol}://{ip}:{port}/{applicationName}/api/login

Example: <span class="legacy-color-text-blue4"><span class="legacy-color-text-blue4">http://192.168.72.90:8080/luz_mobile/api/l</span>ogin</span>

### **II. Method**

PUT

### **III. Request**

Header

<div>

| Attribute name | Attribute value | Description |
|----|----|----|
| Authorization | Basic QWxhZGRpbjpvcGVuIHNlc2FtZQ== | <span class="legacy-color-text-default"><a href="https://en.wikipedia.org/wiki/Basic_access_authentication" class="external-link" rel="nofollow"><span class="legacy-color-text-default">HTTP BASIC authorization</span></a></span> is used. User name and password are BASE64 encoded and delimited by a colon (':') |
| App-Version | 0.0.1 | Current version of the myKLARA app |

</div>

###  **IV.Response**

Successful case

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="ee53c2c0-e803-47aa-9c15-4d6cc1dd09a0" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
{
    "status": "success",
    "message": null,
    "tenants": [
        {
            "id": 1665,
            "roles": [
                "tenant-creator"
            ],
            "tenantId": "8815e317-f12f-4d5b-93e1-e0bc27f233ab",
            "username": "hcmc-helios@axonactive.com",
            "type": "COMPANY",
            "companyInfoId": 1319,
            "companyId": 1,
            "companyName": "Helios Aviation AG",
            "address": {
                "validFrom": null,
                "validTo": null,
                "id": "1",
                "addressType": "PRIVATE",
                "addressLines": "Industriestrasse 17",
                "additionalAddress": "",
                "canton": "BE",
                "city": {
                    "cityName18": "Trachselwald",
                    "cityName27": "Trachselwald"
                }
            },
            "person": null
        },
        ...
    ],
    "refreshToken": "eyJhbGciOiJSUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICJPVkMyV29PVjdfMGJkR1lqSFJYb1FZVE0yekhNOHlJQVh2YWY2dEhnQXpZIn0.eyJqdGkiOiI2Njc0ZjhkMC1hNWNhLTRmNTctOGZmOS1lODMyNzY4NDczYTYiLCJleHAiOjAsIm5iZiI6MCwiaWF0IjoxNTMxMTI4NTk2LCJpc3MiOiJodHRwczovL2xvZ2luLmtsYXJhLmNoL2F1dGgvcmVhbG1zL2tsYXJhIiwiYXVkIjoia2xhcmEtbW9iaWxlIiwic3ViIjoiYzEwNDU0MDktMzEzNS00MGRiLWJhZDYtM2M5ZGJiODgzNjJkIiwidHlwIjoiT2ZmbGluZSIsImF6cCI6ImtsYXJhLW1vYmlsZSIsImF1dGhfdGltZSI6MCwic2Vzc2lvbl9zdGF0ZSI6IjljMzRjOWU2LTkwZDAtNGQ2MS1iZDMzLTMxYmY1ZWMyMDIzMCIsImNsaWVudF9zZXNzaW9uIjoiNjk0NjdiODUtYzkyMi00OTZlLTk1NmUtMjNmNGZlYWJjMDA3IiwicmVhbG1fYWNjZXNzIjp7InJvbGVzIjpbIm9mZmxpbmVfYWNjZXNzIiwidW1hX2F1dGhvcml6YXRpb24iXX0sInJlc291cmNlX2FjY2VzcyI6eyJhY2NvdW50Ijp7InJvbGVzIjpbIm1hbmFnZS1hY2NvdW50Iiwidmlldy1wcm9maWxlIl19fX0.hUFBFEOUJxyQi7_WCjlYrTBZcXPR1GItrJtIVXzbqgqJJhtfAVFwmEd6vVksrcbROphDnjpQVwYvZyHS7UDs75VYmsuBC5mzxFyPUHU2YByoizGzD2F-ofDj2TW6tMqImGsRF69ZBWnqPPq91rggT8SGCyySUJr1ccxjMsR1gNfNWv7O9mO1jvRspdgw-jwqcgF_vqFJYp2avB_vwp17-dnnVNLfnJdtu8lb_wCRD1fKnk8qMki-GS_4VhHTwDJm5aS5v-Ikf-gW0oPSgALFsucQIPUIrqVKbbwNTdLQtdkX8UXiOTPqQTBDxmO6YfdDm1zpqUBoxolXcvxRXQ_Qcw"
}
```

</div>

</div>

Error cases

<div>

| Code | What went wrong |
|----|----|
| BAD_REQUEST (400) | User name or password is empty. |
| UNAUTHORIZED (401) | You have to authorize with a uncorrect correct user name and password. |
| FORBIDDEN (403) | your account has temporarily disabled by keycloak or the account is not fully set up |
| INTERNAL SERVER ERROR (500) | An unexpected exception is happen on server |
| GONE (410) | The generated refresh_token is not valid. |

</div>

Example 

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="1096bbe3-e78b-4953-b844-4de57a2f709f" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
{
    "status": "fail",
    "message": "Invalid user credentials"
}
```

</div>

</div>

%% ai-graph-start %%

**Related notes:**
- [[Getting tenant list]]
- [[App Validity]]
- [[Uploading documents]]
- [[How to use Public API to create update KLARA Business Company]]
- [[LUZ-109076 Public API - Create update new tenant (implementation)]]

%% ai-graph-end %%