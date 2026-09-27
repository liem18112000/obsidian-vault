---
ai_hash: 314b53d4305fe120
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 0
depth: 3
entities: []
relevance: 0.771
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20473126686/KLARA+Integration+request+access+token+call+API
space: LUZ
status: reference
tags:
- confluence
- programming
- space/luz
title: KLARA Integration (request access token & call API)
topic: programming
type: source
updated: 2018-09-17
---

# KLARA Integration (request access token & call API)

> [!info] Imported from Confluence
> Space **LUZ** · updated 2018-09-17 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20473126686/KLARA+Integration+request+access+token+call+API)
> Relevance 0.771 · topic `programming`

<div class="toc-macro client-side-toc-macro conf-macro output-block" hasbody="false" headerelements="H1,H2,H3,H4,H5,H6,H7" macro-id="e232268f-e2fd-486b-9753-cfb22ac6913e" macro-name="toc">

</div>

## 1. Configuration KLARA application on API developer portal

<span class="fontstyle0">On the API developer portal (</span><span class="fontstyle0 legacy-color-text-blue1"><a href="https://apistoretest.suva.ch:443" class="external-link" rel="nofollow">https://apistoretest.suva.ch:443</a></span><span class="fontstyle0">) the following configurations have to be set for the KLARA application:</span>

- <span class="fontstyle0">Select checkbox «SAML2» for "Federated Authentication"</span>
- <span class="fontstyle0">Configure the callback URL (</span><span class="fontstyle0 legacy-color-text-blue1"><a href="https://www.klara.ch/" class="external-link" rel="nofollow">https://www.klara.ch/</a></span><span class="fontstyle0">...) and select the checkbox "code"</span>

## <span class="fontstyle0">2. Request authorization code</span>

<span class="fontstyle2">For the request of an authorization code the user agent (browser) has to perform the following HTTP GET:</span>

    GET /authorize?response_type=code&client_id=<Consumer
    Key>&scope=default&redirect_uri=<Callback URL>
    Host: apitest.suva.ch
    Content-Type: application/x-www-form-urlencoded

The consumer key can be found in the <span class="fontstyle2">API developer portal on the created KLARA application.  
</span>

## <span class="fontstyle0">3. Suva login with the following credentials</span>

<span class="fontstyle2">User: <a href="mailto:adrian.nigg@suva.ch" class="external-link" rel="nofollow">adrian.nigg@suva.ch</a>  
Password: 100%Klara  
After that the acces to Suva data has to be confirmed by KLARA.  
As the response the authorization code is delivered as a query parameter in the callback URL:</span>

    https://www.klara.ch/...?code=<Authorization Code>

## <span class="fontstyle0">4. Request access token</span>

<span class="fontstyle2">With the following HTTP POST the user agent (browser) can request an access token:</span>

    POST /token?grant_type=authorization_code&code=<Authorization
    Code>&redirect_uri=<Callback URL>
    Host: apitest.suva.ch
    Authorization: Basic Base64Encode(<Consumer Key>:<Consumer Secret>)
    Content-Type: application/x-www-form-urlencoded

<span class="fontstyle2">The consumer secret can be found on the API developer Portal in the KLARA application.  
The response delivered by the API gateway delivers the access token, the usage area and the remaining validity of the token in JSON format.  
Below a sample response:  
</span>

{

    "access_token":"467353b0-82ed-3aa2-b9f1-cecc44664f38",
    "refresh_token":"15e6815f-f994-375d-8dc8-775883cb4f52",
    "scope":"default",
    "token_type":"Bearer",
    "expires_in":3600
    }

<span class="fontstyle1">After the token became invalid a new token can be requested using the refresh token:</span>

    POST /token?grant_type=refresh_token&refresh_token=<Refresh Token>
    Host: apitest.suva.ch
    Authorization: Basic Base64Encode(<Consumer Key>:<Consumer Secret>)
    Content-Type: application/x-www-form-urlencoded

## <span class="fontstyle3">5. Call UserContextInfo API</span>

<span class="fontstyle3">For the call of the <span class="fontstyle1">DocumentStatusInfo API, the partner number has to be passed as a query parameter.  
With the following call the partner number can be queried:</span></span>

    GET /usermanagement/UserContextInfo/1.0.0/userContextInfo
    Accept-Encoding: gzip,deflate
    Accept: application/json
    Authorization : Bearer <Access Token>
    Host: apitest.suva.ch

<span class="fontstyle1">Below a sample response:</span>

    {
    "email":
    "adrian.nigg@suva.ch",
    "firstName":"Hans",
    "gender":"MALE",
    "language":"de",
    "lastName":"Muster",
    "organizations":[{
    "name":"Betriebsname unbekannt",
    "partnerNo":8794672,
    "roles":["B_ADM"]
    }],
    "userId": "EE467313"
    }

## <span class="fontstyle3">6. Call DocumentStatusInfo API</span>

<span class="fontstyle0">With the partner number (see above) the <span class="fontstyle1">DocumentStatusInfo API can be called.  
Below a sample request:</span>  
</span>

    GET
    /documentmanagement/DocumentStatusInfo/1.0.0/documentStatusInfo?partnerNr=
    <Partner Nummer>
    Accept-Encoding: gzip,deflate
    Suva-User-Language: de
    Accept: application/json
    Authorization : Bearer <Access Token>
    Host: apitest.suva.ch

<span class="fontstyle0"><span class="fontstyle0"><span class="fontstyle1">Below a sample response:</span></span>  
</span>

    [{"documentStatistics": [
    {
    "codeName": "versicherungsgrundlagen-personen",
    "description": "Versicherungsgrundlagen Personen",
    "availableDocuments": 0
    },
    {
    "codeName": "lohndeklaration-praemienrechnung",
    "description": "Lohndeklaration und Prämienrechnung",
    "availableDocuments": 1
    },
    {
    "codeName": "taggeldabrechnungen",
    "description": "Taggeldabrechnungen",
    "availableDocuments": 11
    },
    {
    "codeName": "unfallkorrespondenz",
    "description": "Unfallkorrespondenz",
    "availableDocuments": 60
    },
    {
    "codeName": "auswertungen-unfallversicherung",
    "description": "Auswertungen zur Unfallversicherung",
    "availableDocuments": 3
    },
    {
    "codeName": "alle",
    "description": "Alle",
    "availableDocuments": 95
    },
    {
    "codeName": "arbeitssicherheit-gesundheitsschutz",
    "description": "Arbeitssicherheit und Gesundheitsschutz",
    "availableDocuments": 11
    },
    {
    "codeName": "versicherungsgrundlagen-betrieb",
    "description": "Versicherungsgrundlagen Betrieb",
    "availableDocuments": 1
    },
    {
    "codeName": "allgemeine-korrespondenz",
    "description": "Allgemeine Korrespondenz",
    "availableDocuments": 8
    }
    ]}]

%% ai-graph-start %%

**Related notes:**
- [[Use KLARA Swagger UI for REST API]]
- [[Understanding Keycloak Authorization Code flow]]
- [[Implement Corporate API Access (LUZ-17959)]]
- [[How to use Public API to create update KLARA Business Company]]
- [[KLARA Booking - KLARA OBC API]]

%% ai-graph-end %%