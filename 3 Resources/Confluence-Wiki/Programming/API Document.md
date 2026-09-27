---
title: "API Document"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20508019771/API+Document
space: "LUZ"
topic: programming
relevance: 0.86
depth: 3
updated: 2020-05-28
attachments: 0
tags:
  - confluence
  - programming
  - space/luz
---

# API Document

> [!info] Imported from Confluence
> Space **LUZ** · updated 2020-05-28 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20508019771/API+Document)
> Relevance 0.86 · topic `programming`

## Configure

API in luz_hubspot to sync Subscription information from Klara to Hubspot when have some actions and make some change affect to Subscription information

1.  ### Create a new subscription

    1.  #### Method type & Endpoint

        <div>

        |  |  |
        |----|----|
        | Method | PUT, POST |
        | Endpoint | <span class="legacy-color-text-default">http://\<host\>:8080/luz_hubspot/api/subscription/create/{subscription_id}</span> |

        </div>

    2.  #### Response

        <div>

        <table>
        <colgroup>
        <col style="width: 50%" />
        <col style="width: 50%" />
        </colgroup>
        <tbody>
        <tr>
        <td colspan="2"><p>{</p>
        <p>          "dealId": "2006714900",</p>
        <p>          "productId": "29",</p>
        <p>          "companyId": "3871444763",</p>
        <p>          "vid": "11301"</p>
        <p>}</p></td>
        </tr>
        </tbody>
        </table>

        </div>

2.  ### Update a subscription( subscribe, unsubscribe, purchases, update)

    1.  #### Method type & Endpoint

        <div>

        |  |  |
        |----|----|
        | Method | PUT, POST |
        | Endpoint | <span class="legacy-color-text-default">http://\<host\>:8080/luz_hubspot/api/subscription/update/{subscription_id}</span> |

        </div>

    2.  Body  
        <div>

        <table>
        <colgroup>
        <col style="width: 50%" />
        <col style="width: 50%" />
        </colgroup>
        <tbody>
        <tr>
        <th>SUBSCRIBE</th>
        <td><p>{</p>
        <p>    "action": "SUBSCRIBE"</p>
        <p>}</p></td>
        </tr>
        <tr>
        <th>UNSUBSCRIBE</th>
        <td><p>{</p>
        <p>   "email": "test@<a href="http://example.com" class="external-link" rel="nofollow">example.com</a>"</p>
        <p>    "action": "UNSUBSCRIBE"</p>
        <p>}</p></td>
        </tr>
        <tr>
        <th>INCREASE</th>
        <td><p>{</p>
        <p>    "volume": "100"</p>
        <p>    "action": "INCREASE"</p>
        <p>}</p></td>
        </tr>
        <tr>
        <th>UPDATE</th>
        <td><p>{</p>
        <p>    "action": "UPDATE"</p>
        <p>}</p></td>
        </tr>
        </tbody>
        </table>

        </div>

    3.  #### Response

        <div>

        <table>
        <colgroup>
        <col style="width: 50%" />
        <col style="width: 50%" />
        </colgroup>
        <tbody>
        <tr>
        <td colspan="2"><p>{</p>
        <p>          "dealId": "2006714900",</p>
        <p>          "productId": "29",</p>
        <p>          "companyId": "3871444763",</p>
        <p>          "vid": "11301"</p>
        <p>}</p></td>
        </tr>
        </tbody>
        </table>

        </div>

3.  ### Delete Subscription

    1.  #### Method type & Endpoint

        <div>

        <table style="font-weight: 400;letter-spacing: 0.0px;">
        <colgroup>
        <col style="width: 50%" />
        <col style="width: 50%" />
        </colgroup>
        <tbody>
        <tr>
        <th>Method</th>
        <td>PUT, POST</td>
        </tr>
        <tr>
        <th>Endpoint</th>
        <td><span class="legacy-color-text-default">http://&lt;host&gt;:8080/luz_hubspot/api/<span>subscription</span>/delete/{<span>subscription_id</span>}</span></td>
        </tr>
        <tr>
        <th>Body</th>
        <td><p>{</p>
        <p>   "email": "<a href="mailto:test@example.com" class="external-link" rel="nofollow">test@example.com</a>"</p>
        <p>}</p></td>
        </tr>
        </tbody>
        </table>

        </div>

    2.  #### Response

        <div>

        <table>
        <colgroup>
        <col style="width: 50%" />
        <col style="width: 50%" />
        </colgroup>
        <tbody>
        <tr>
        <td colspan="2"><p>{</p>
        <p>          "dealId": "2006714900",</p>
        <p>          "productId": "29",</p>
        <p>          "companyId": "3871444763",</p>
        <p>          "vid": "11301"</p>
        <p>}</p></td>
        </tr>
        </tbody>
        </table>

        </div>
