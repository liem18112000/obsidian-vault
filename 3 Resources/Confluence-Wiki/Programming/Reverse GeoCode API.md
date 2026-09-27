---
ai_hash: dc89d43edde1f7b0
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 0
depth: 2.73
entities: []
relevance: 0.731
source: https://axonivy.atlassian.net/wiki/spaces/Helios/pages/38162415316/Reverse+GeoCode+API
space: Helios
status: reference
tags:
- confluence
- programming
- space/helios
title: Reverse GeoCode API
topic: programming
type: source
updated: 2017-10-26
---

# Reverse GeoCode API

> [!info] Imported from Confluence
> Space **Helios** · updated 2017-10-26 · [open original](https://axonivy.atlassian.net/wiki/spaces/Helios/pages/38162415316/Reverse+GeoCode+API)
> Relevance 0.731 · topic `programming`

**Description**: Reverse lat-long coordinate into address

**Request** **Path**: {Base URL}/address/geocode

**Method**: POST

**Authentication:** Basic authentication with username and password

**Request Body:**

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="ea48d376-c1d3-4a98-a56f-844d2bb32c2e" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
{
    "lat":"47.378824",
    "lon":"8.526764"
}
```

</div>

</div>

**<span class="legacy-color-text-default">Response Object</span>:**

<div>

|             |         |                              |
|-------------|---------|------------------------------|
| Field name  | Type    | Description                  |
| level       | Integer | value: always -1             |
| countryCode | String  | Country code                 |
| zip         | String  | zipCode                      |
| street      | String  | Street name and house number |
| locationId  | Integer | always 0                     |
| lat         | double  | latitude                     |
| lon         | double  | longitude                    |
| ortId       | Integer | always 0                     |

</div>

**Response Example:**

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="807ea9b6-5942-45df-ad44-7fafe93d41fc" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
{
    "level": -1,
    "countryCode": "CH",
    "zip": "8004",
    "town": "Zürich",
    "street": "Rolandstrasse 3",
    "locationId": 0,
    "lat": 47.378834,
    "lon": 8.526869699999999,
    "ortId": 0
}
```

</div>

</div>

**Error**

**Response code**

<div>

|                 |
|-----------------|
| 400 Bad request |

</div>

%% ai-graph-start %%

**Related notes:**
- [[Login]]
- [[Getting tenant list]]
- [[API to generate authentication letter for inividual]]
- [[Luz_google Api Document]]
- [[Hexonet API Document]]

%% ai-graph-end %%