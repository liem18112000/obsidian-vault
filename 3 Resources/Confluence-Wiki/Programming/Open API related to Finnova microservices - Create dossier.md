---
title: "Open API related to Finnova microservices - Create dossier"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/RT/pages/26446682851/Open+API+related+to+Finnova+microservices+-+Create+dossier
space: "RT"
topic: programming
relevance: 0.738
depth: 2.65
updated: 2021-06-03
attachments: 41
tags:
  - confluence
  - programming
  - space/rt
---

# Open API related to Finnova microservices - Create dossier

> [!info] Imported from Confluence
> Space **RT** · updated 2021-06-03 · [open original](https://axonivy.atlassian.net/wiki/spaces/RT/pages/26446682851/Open+API+related+to+Finnova+microservices+-+Create+dossier)
> Relevance 0.738 · topic `programming`

## **1. Flow:**

<div>

<table style="width: 39.7077%;">
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<tbody>
<tr>
<th style="text-align: center;"><strong>Process</strong></th>
<th style="text-align: center;"><strong>Plantum txt file</strong></th>
<th style="text-align: center;">Image</th>
</tr>
&#10;<tr>
<td style="text-align: center;"><p>Create new dossier new credit business</p></td>
<td style="text-align: center;"><div class="content-wrapper">
<p><a href="../_attachments/26446682851-get-data-create-new-business-dossier.plantuml">get-data-create-new-business-dossier.plantuml</a></p>
</div></td>
<td style="text-align: center;"><div class="content-wrapper">
<div id="expander-265880624" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="de10bb54-54f1-40ef-b559-10a36faec3d6" data-macro-name="expand">
<div id="expander-control-265880624" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">View sequence</span>
</div>
<div id="expander-content-265880624" class="expand-content expand-hidden">

![[26446682851-get-data-create-new-business-dossier.png]]


</div>
</div>
</div></td>
</tr>
<tr>
<td style="text-align: center;">Create new dossier in existing credit business</td>
<td style="text-align: center;"><div class="content-wrapper">
<p><a href="../_attachments/26446682851-get-data-create-existing-business-dossier.plantuml">get-data-create-existing-business-dossier.plantuml</a></p>
</div></td>
<td style="text-align: center;"><div class="content-wrapper">
<div id="expander-372962354" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="52fa08d5-308a-4e12-a887-8070acff4a5b" data-macro-name="expand">
<div id="expander-control-372962354" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">View sequence</span>
</div>
<div id="expander-content-372962354" class="expand-content expand-hidden">

![[26446682851-get-data-create-existing-business-dossier.png]]


</div>
</div>
</div></td>
</tr>
</tbody>
</table>

</div>

  

## **2. OpenAPI:**

## ** FinancialDataReferenceService:**

## <span class="confluence-embedded-file-wrapper conf-macro output-inline" hasbody="false" macro-id="32225641-c2e2-4d40-81b7-eff1bdf47197" macro-name="view-file"><a href="../_attachments/26446682851-ch-financial-data-reference-service-api-spec.yaml" class="confluence-embedded-file" data-nice-type="null" data-file-src="/wiki/download/attachments/26446682851/ch-financial-data-reference-service-api-spec.yaml?version=1&amp;modificationDate=1622718277000&amp;cacheVersion=1&amp;api=v2" data-mime-type="application/octet-stream" data-has-thumbnail="true">

![[26446682851-ch-financial-data-reference-service-api-spec.yaml]]

</a></span>

## ** MortgageFinnovaAdapterService:**

       <span class="confluence-embedded-file-wrapper conf-macro output-inline" hasbody="false" macro-id="c33924a4-0547-423c-b211-a240bb7c1a5e" macro-name="view-file"><a href="../_attachments/26446682851-ch-mortgage-finnova-adapter-service-api-spec.yaml" class="confluence-embedded-file" data-nice-type="null" data-file-src="/wiki/download/attachments/26446682851/ch-mortgage-finnova-adapter-service-api-spec.yaml?version=1&amp;modificationDate=1622718287000&amp;cacheVersion=1&amp;api=v2" data-mime-type="application/octet-stream" data-has-thumbnail="true">

![[26446682851-ch-mortgage-finnova-adapter-service-api-spec.yaml]]

</a></span>

<div>

|  |  |
|----|----|
| Data | API |
| get Order | /banks/{userbank}/orders |
| get Order Detail | /banks/{userbank}/orders/{orderKey} |
| get Finances | /banks/{userbank}/clients/{clientKey}/finances' |
| get Client Main Type | /banks/{userbank}/clients/{clientKey}/clientMainType |
| get Private Individual | /banks/{userbank}/clients/{clientKey}/privateIndividual' |
| get Two Person Houdehold | /banks/{userbank}/clients/{clientKey}/twoPersonHousehold |
| get Companies And Other | /banks/{userbank}/clients/{clientKey}/companiesAndOther |
| get Relationship | /banks/{userbank}/clients/{clientKey}/relationships |
| get Exposure | /banks/{userbank}/clients/{clientKey}/totalExposure |
| get Collaterals | /banks/{userbank}/orders/{orderKey}/collateral |
| get Real Estate Mortgage Certificate | /banks/{userbank}/properties/{propertyKey}/mortgageCertificates |
| get Real Estate | /banks/{userbank}/realEstateData |
| get ApartmentNumberOfRooms | /banks/{userbank}/codes/apartmentNumbersOfRooms |
| get Property By Key | /banks/{userbank}/properties/{propertyKey} |
| get Pricing | /banks/{userbank}/loanAdvisories/pricing |
| get Appraisal | /banks/{userbank}/properties/{propertyKey}/appraisals |
| get Mortgage Certificate | /banks/{userbank}/orders/{orderKey}/mortgagecertificates |
| get Security | /banks/{userbank}/securities/{securityKey} |
| get Encumbrance | /banks/{userbank}/properties/{propertyKey}/encumbrances |
| get Usufructs | /banks/{userbank}/usufruct/{easementKey} |
| get Loan Order Additonal Fields | /banks/{userbank}/loanorder-additionalfields |
| get Amortisation | /banks/{userbank}/orders/{orderKey}/amortisations |
| get Item detail | /banks/{userbank}/orders/{orderKey}/items/{itemKey} |

</div>

## **3. HTTP status code:**

If we get the error status code from finnova, we will keep original code and send it back to system.

If there is an error from our service, we will control and define our code (Follow RFC of HTTP)

## **4. Authentication**

We have an other project to authenticate the communication of internal service (ex: FinancialDataReferenceService and MortgageFinnovaAdapterService)

For the communication to third party, we will use the authenticate of third party. Ex: when MortgageFinnovaAdapterService call to finnova to get Pricing data and receive a response with code 401, MortgageFinnovaAdapterService will send a request to finnova to get the token. After receive the token, MortgageFinnovaAdapterService will be call to finnova to get Pricing data again.
