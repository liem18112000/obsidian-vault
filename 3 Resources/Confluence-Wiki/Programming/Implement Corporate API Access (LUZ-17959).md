---
title: "Implement Corporate API Access (LUZ-17959)"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20481508537/Implement+Corporate+API+Access+LUZ-17959
space: "LUZ"
topic: programming
relevance: 0.847
depth: 3
updated: 2019-11-07
attachments: 28
tags:
  - confluence
  - programming
  - space/luz
---

# Implement Corporate API Access (LUZ-17959)

> [!info] Imported from Confluence
> Space **LUZ** · updated 2019-11-07 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20481508537/Implement+Corporate+API+Access+LUZ-17959)
> Relevance 0.847 · topic `programming`

The goal of the Swiss Corporate API platform is to provide an unified Swiss infrastructure by SIX for any interactions between third party providers (TPPs) and financial institutions.


![[20481508537-image2019-3-5_17-5-53.png]]



  


![[20481508537-Cor API flow.png]]



  

# 1. OAuth

A major part of the system is OAuth for each financial institution. The flow is illustrated as follows :


![[20481508537-image2019-3-1_11-3-40.png]]



  

OAuth Step 1 : Set consent

The user, usually in eBanking of the specific bank, configures which IBAN are available for CorAPI (=set consent). In the SIX FI Simulator it looks as follows:


![[20481508537-image2019-4-10_13-37-37.png]]



Only the selected accounts can be accessed from CorAPI.

If the user clicks on "Authorize Client" then the browser is redirected to (Test) :

<a href="https://api-test.klara.ch/consent/six?bank_id=IIDX99999" class="external-link" rel="nofollow"><strong>https://api-test.klara.ch/consent/six</strong>?bank_id=CIDX9999999999</a>

bank_id : Financial Institution, in this case it is the Six FI Simulator (CIDX9999999999).

(Dev: to test please replace URI with the entry point URI of Ivy process in Dev ... <a href="http://localhost:8081/ivy/pro/designer/luz_cor_api/1690A16FD92AFABA/start.ivp?bank_id=IIDX99999" class="external-link" rel="nofollow">http://localhost:8081/ivy/pro/designer/luz_xyz/1690A16FD92AFABA/start.ivp?bank_id=CIDX9999999999</a>)

  

(New)

Step 1b : Check if the user is logged in, if not then redirect to Klara login. After successful login, <a href="https://api-test.klara.ch/consent/six?bank_id=IIDX99999" class="external-link" rel="nofollow"><strong>https://api-test.klara.ch/consent/six</strong>?bank_id=CIDX9999999999</a> should be called again.

  

Step 2 : Auth Code for Oauth

CorAPI provides a login to obtain an auth code. This is comparable to the Valiant login.

URL : <a href="https://swiss-corporate-api-etu.six-group.com/fi/api/oauth/authorize/?login&amp;response_type=code&amp;client_id=CIDX0000000002&amp;redirect_uri=https://api-test.klara.ch/consent/six&amp;state=eyJiYW5rSWQiOiJJSURYOTk5OTkifQ%3D%3D" class="external-link" rel="nofollow">https://swiss-corporate-api-etu.six-group.com/fi/api/oauth/authorize/?login&amp;response_type=code&amp;client_id=CIDX0000000002&amp;redirect_uri=https://api-test.klara.ch/consent/six&amp;state=eyJiYW5rSWQiOiJJSURYOTk5OTkifQ%3D%3D</a>


![[20481508537-image2019-4-10_13-52-46.png]]



After the login, the redirect_uri is applied:

<a href="https://api-test.klara.ch/consent/six?code=ILvhtM&amp;state=eyJiYW5rSWQiOiJJSURYOTk5OTkifQ%3D%3D" class="external-link" rel="nofollow"><strong>https://api-test.klara.ch/consent/six</strong>?code=ILvhtM&amp;state=eyJiYW5rSWQiOiJJSURYOTk5OTkifQ%3D%3D</a>

where in this case the auth code would be Kw5G7S.

(Dev: To test please replace URI with Ivy URI in Dev)

  

Step 3: After the above call, the token is requested by calling "/api/bankingservices/corporate/v1/oauth/token". The response will provide the token which is saved in luz_key_value_store


![[20481508537-image2019-4-10_13-58-19.png]]



## Implementation

The OAuth flow in Six Cor API is triggered by the eBanking, where the client (= KLARA) is authorized to access account and/or payment information. As mentioned above Klara will called by the URL <a href="https://api-test.klara.ch/consent/six?bank_id=IIDX99999" class="external-link" rel="nofollow"><strong>https://api-test.klara.ch/consent/six</strong>?bank_id=IIDX99999.</a> In Dev we can only simulate this call by invoking the process start. A link such as <a href="http://localhost:8081/ivy/pro/designer/luz_cor_api/1690A16FD92AFABA/start.ivp?bank_id=IIDX99999" class="external-link" rel="nofollow">http://localhost:8081/ivy/pro/designer/luz_cor_api/1690A16FD92AFABA/start.ivp?bank_id=IIDX99999</a> should be sufficient to test the process below.


![[20481508537-image2019-4-23_14-14-23.png]]



Currently in the view of OAuth xhtml, there is the following statement \<p:remoteCommand name="onload" action="#{logic.oauth()}" /\> which triggers the sub process. The OAuth flow is in fact called twice :

  

OAuth Pass 1: In case there is no token or token has expired, we will have this flow, which ends with a redirect to the authentication page of the financial institutions (= e.g. UBS). After a successful login the user is redirected again to this process but as "Pass 2".


![[20481508537-image2019-4-23_14-21-1.png]]



OAuth Pass 2: In contrary to above we have a authentication token to retrieve an access token. The retrieval is done in the service "Refresh Token".


![[20481508537-image2019-4-23_14-21-59.png]]



Important: The above flow is only valid if the user is redirected from eBanking ("Authorize Client"). For the "service" methods such as GetAccounts, GetTransactions, SendPayments etc, the access token is refreshed on a 24h basis each time these services call the Cor API rest services.

  

# 2. GetAccounts

As soon as the consent has been set and token has been retrieved, the available accounts can be loaded. Important : only those accounts which have been **selected for the consent are available**:

REST GET : luz_cor_api - /{company-tenant-id}/companies/1/ais/accounts

<span class="sBracket structure-1">\[ </span>  
<span class="sBrace structure-2">{ </span>  
**<span class="sObjectK">"id"</span><span class="sColon">:</span><span class="sObjectV">"1"</span><span class="sComma">,</span>**  
<span class="sObjectK">"account"</span><span class="sColon">:</span><span class="sBrace structure-3">{ </span>  
<span class="sObjectK">"type"</span><span class="sColon">:</span><span class="sObjectV">"IBAN"</span><span class="sComma">,</span>  
**<span class="sObjectK">"identification"</span><span class="sColon">:</span><span class="sObjectV">"CH4299999000000000001"</span>**  
<span class="sBrace structure-3">}</span><span class="sComma">,</span>  
<span class="sObjectK">"currency"</span><span class="sColon">:</span><span class="sObjectV">"EUR"</span><span class="sComma">,</span>  
<span class="sObjectK">"designation"</span><span class="sColon">:</span><span class="sObjectV">"Bank Account 1 - no bookings"</span><span class="sComma">,</span>  
<span class="sObjectK">"links"</span><span class="sColon">:</span><span class="sBrace structure-3">{ </span>  
<span class="sObjectK">"self"</span><span class="sColon">:</span><span class="sObjectV">"/accounts/1"</span><span class="sComma">,</span>  
<span class="sObjectK">"balance"</span><span class="sColon">:</span><span class="sObjectV">"/accounts/1/balance"</span><span class="sComma">,</span>  
<span class="sObjectK">"transactions"</span><span class="sColon">:</span><span class="sObjectV">"/accounts/1/transactions"</span>  
<span class="sBrace structure-3">}</span>  
<span class="sBrace structure-2">}</span><span class="sComma">,</span>  
<span class="sBrace structure-2">{ </span>  
**<span class="sObjectK">"id"</span><span class="sColon">:</span><span class="sObjectV">"4"</span><span class="sComma">,</span>**  
<span class="sObjectK">"account"</span><span class="sColon">:</span><span class="sBrace structure-3">{ </span>  
<span class="sObjectK">"type"</span><span class="sColon">:</span><span class="sObjectV">"IBAN"</span><span class="sComma">,</span>  
**<span class="sObjectK">"identification"</span><span class="sColon">:</span><span class="sObjectV">"CH5899999000000000004"</span>**  
<span class="sBrace structure-3">}</span><span class="sComma">,</span>  
<span class="sObjectK">"currency"</span><span class="sColon">:</span><span class="sObjectV">"CHF"</span><span class="sComma">,</span>  
<span class="sObjectK">"designation"</span><span class="sColon">:</span><span class="sObjectV">"Bank Account 4 - with bookings for today\u0027s date"</span><span class="sComma">,</span>  
<span class="sObjectK">"links"</span><span class="sColon">:</span><span class="sBrace structure-3">{ </span>  
<span class="sObjectK">"self"</span><span class="sColon">:</span><span class="sObjectV">"/accounts/4"</span><span class="sComma">,</span>  
<span class="sObjectK">"balance"</span><span class="sColon">:</span><span class="sObjectV">"/accounts/4/balance"</span><span class="sComma">,</span>  
<span class="sObjectK">"transactions"</span><span class="sColon">:</span><span class="sObjectV">"/accounts/4/transactions"</span>  
<span class="sBrace structure-3">}</span>  
<span class="sBrace structure-2">}</span>  
<span class="sBracket structure-1">\]</span>

# 3. GetTransactions

For a given IBAN (resp. **id** of an account), the transactions can be loaded :

REST GET : luz_cor_api - /{company-tenant-id}/companies/1/ais/transaction/{iban\]

{

  "iban": "CH4299999000000000001",

  "designation": "Bank Account 1 - no bookings",

  "entries": \[

    {

      "transactionType": "DBIT",

      "bookingDate": "2019-04-11",

      "valueDate": "2019-04-11",

      "bankTransactionCode": {

        "domainCode": "PMNT",

        "familyCode": "ICDT",

        "subFamilyCode": "OTHR"

      },

      "amount": {

        "currency": "CHF",

        "amount": "100.00"

      },

      "transactions": \[

        {

          "transactionId": "TX000021",

          "transactionType": "DBIT",

          "endToEndId": "E2E-20190411-VZNRIE-0005",

          "amount": {

            "currency": "CHF",

            "amount": "100.00"

          },

          "counterparty": {

            "name": "Horst Schlonz",

            "postalAddress": {

              "structured": {

                "streetName": "Mustergasse",

                "buildingNumber": "11",

                "postCode": "9000",

                "townName": "St. Gallen",

                "country": "CH"

              }

            },

            "account": {

              "type": "IBAN",

              "identification": "CH7509000000800077785"

            },

            "agent": {}

          },

          "remittanceInformation": "bla bla bla"

        }

      \]

    }

  \],

  "links": {

    "self": "/accounts/1/transactions",

    "account": "/accounts/1",

    "balance": "/accounts/1/balance"

  }

}

  

# 4. SendPayments

REST POST : luz_cor_api - /{company-tenant-id}/companies/1/pis/payments

Sample data structure (ch.klara.bank.corapi.model.payment.Payment)

{

  "messageId": "eb6305c91f7f49deaed016487c27b42d",

  "initiatingPartyId": "TPP01746",

  "requestedExecutionDate": "2018-04-07",

  "debtorAccount": {

    "type": "IBAN",

    "identification": "CH350023923910402740Q"

  },

  "bookingInstruction": "BATCHBOOKING_CWD",

  "transactions": \[

    {

      "instructionId": "DNCS-20180407-IXN0-TXN0",

      "endToEndId": "ENDTOENDID-001",

      "instructedAmount": {

        "currency": "CHF",

        "amount": "8479.25"

      },

      "ibanDetails": {

        "creditorAccount": {

          "type": "IBAN",

          "identification": "CH9300862011623852339"

        },

        "creditor": {

          "name": "Peter Haller",

          "postalAddress": {

            "structured" : {

              "streetName" : "Hardturmstrasse",

              "buildingNumber" : "201",

              "postCode" : "8021",

              "townName" : "Zuerich",

              "country" : "CH"

            }

          }

        },

        "remittanceInformation": "Rechnung Nr. 408"

      }

    },

    {

      "instructionId": "DNCS-20180407-IXN0-TXN1",

      "endToEndId": "ENDTOENDID-002",

      "instructedAmount": {

        "currency": "CHF",

        "amount": "6400.25"

      },

      "isrDetails": {

        "creditorAccount": {

          "type": "OTHER",

          "identification": "01-39139-1"

        },

        "creditor": {

          "name": "Robert Schneider",

          "postalAddress": {

            "structured" : {

              "streetName" : "Rue de la gare",

              "buildingNumber" : "25",

              "postCode" : "2501",

              "townName" : "Biel",

              "country" : "CH"

            }

          }

        },

        "remittanceReference": {

          "type": "ISR",

          "reference": "210000000003139471430009017"

        }

      }

    },

    {

      "instructionId": "DNCS-20180407-IXN0-TXN2",

      "endToEndId": "ENDTOENDID-001",

      "instructedAmount": {

        "currency": "CHF",

        "amount": "10.1"

      },

      "ibanDetails": {

        "creditorAccount": {

          "type": "IBAN",

          "identification": "CH85002582584X1234560"

        },

        "creditorAgent": {

          "clearingSystemMemberIdentification": {

            "code": "CHBCC",

            "memberId": "00258"

          }

        },

        "creditor": {

          "name": "Max Muster",

          "postalAddress": {

            "unstructured" : {

              "addressLines" : \[

                "Zentralstrasse 55",

                "5610 Wohlen AG"

              \],

              "country" : "CH"

            }

          }

        },

        "remittanceInformation": "Rechnungsnummer 18C527-005"

      }

    }

  \]

}

# 5. Token Refresh

Access token can be refreshed by using the refresh_token :


![[20481508537-image2019-3-1_11-2-57.png]]



One should always check the token before calling a Six Cor API service. Either the token is refreshed using the refresh_token or a login has to be performed in order to get the authentication code to retrieve the access_code.

  

UI + Backend Specification (who does what) :


![[20481508537-image2019-4-10_16-3-20.png]]



  

# Error Handling

  

<div>

<table>
<colgroup>
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
</colgroup>
<tbody>
<tr>
<th>Exception</th>
<th>REST Status</th>
<th>Description</th>
<th>Cause</th>
<th><br />
</th>
</tr>
&#10;<tr>
<td>NoTokenException</td>
<td>Response.Status.SERVICE_UNAVAILABLE</td>
<td>luz_cor_api was not able to retrieve an access token by using the refresh token.</td>
<td><ol>
<li>Refresh token has expired and the user needs to go to the eBanking and "Authorize the Client" (=causes to start the OAuth flow)</li>
</ol></td>
<td><p>GetAccounts</p>
<p>GetTransactions</p>
<p>SendPayments</p></td>
</tr>
<tr>
<td><br />
</td>
<td>Status.NOT_FOUND</td>
<td>For a given IBAN no value for bank_id can be found in KeyValueStore</td>
<td><ol>
<li>The user has not yet authorized Klara in eBanking for this IBAN</li>
</ol></td>
<td><br />
</td>
</tr>
<tr>
<td><br />
</td>
<td>Status.BAD_REQUEST</td>
<td>Technical error happened in backend.</td>
<td><br />
</td>
<td>Backend</td>
</tr>
</tbody>
</table>

</div>

  

  

# Appendix

  


![[20481508537-image2019-10-10_13-28-6.png]]



\(c\) by Hau-Tran
