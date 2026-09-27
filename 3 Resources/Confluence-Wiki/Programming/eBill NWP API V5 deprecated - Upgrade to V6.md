---
title: "eBill NWP API V5 deprecated -> Upgrade to V6"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/HACKA/pages/49011982458/eBill+NWP+API+V5+deprecated+-+Upgrade+to+V6
space: "HACKA"
topic: programming
relevance: 0.818
depth: 3
updated: 2026-01-12
attachments: 2
tags:
  - confluence
  - programming
  - space/hacka
---

# eBill NWP API V5 deprecated -> Upgrade to V6

> [!info] Imported from Confluence
> Space **HACKA** · updated 2026-01-12 · [open original](https://axonivy.atlassian.net/wiki/spaces/HACKA/pages/49011982458/eBill+NWP+API+V5+deprecated+-+Upgrade+to+V6)
> Relevance 0.818 · topic `programming`

<span class="confluence-jim-macro jira-issue conf-macro output-block" client-id="SINGLE_d3f195c5-8684-3b17-b4f6-e9ee3a0b0fe2_49011982458_712020:87b0f7f1-aaab-4406-a25d-fa0fc075c4d4" hasbody="false" jira-key="LUZ-145524" macro-id="c8b3f572-4686-423e-807e-04e285df8d28" macro-name="jira"> <a href="https://axonivy.atlassian.net/browse/LUZ-145524" class="jira-issue-key"><span class="aui-icon aui-icon-wait issue-placeholder"> </span>LUZ-145524</a> - <span class="summary">Getting issue details...</span> <span class="aui-lozenge aui-lozenge-subtle aui-lozenge-default issue-placeholder">STATUS</span> </span>

<div class="toc-macro client-side-toc-macro conf-macro output-block" cssliststyle="none" hasbody="false" headerelements="H1,H2,H3,H4,H5,H6,H7" macro-id="b1737d70-74c0-47b9-9f1d-cc4cf71b4879" macro-name="toc">

</div>

# Overview

This document outlines all changes required to upgrade from eBill Network Partner API **v5.1.1** to **v6.4.3**.

**Migration Type:** Major Version Upgrade (Breaking Changes)  
**API Specification:** OpenAPI 2.0 (Swagger) → OpenAPI 3.0.1 (OAS3)  
**Estimated Effort:** Medium to High  
**Backward Compatibility:** None - v5 and v6 are separate endpoints  

eBill Network Partner API Endpoints  
Base URL: <a href="https://api-preprod.np.six-group.com/api/pns/xe/networkpartner/v5" class="external-link" rel="nofollow"><u>https://api-preprod.np.six-group.com/api/pns/xe/networkpartner/v5</u></a>

==\> We need to change the base URL:  


![[49011982458-image-20260102-030852.png]]

![[49011982458-image-20260102-030921.png]]



**luz-public-api-adapter will call luz-ebill-networkpartner ==\> We need to change the code on luz-ebill-networkpartner**

# APIs

🔹 Biller Management  
POST /billers - Create a new biller  
GET /billers/{billerId} - Get biller details  
PUT /billers/{billerId} - Update biller information  
POST /billers/search - Search for billers with filters  
🔹 Bill Recipients  
POST /billers/{billerId}/bill-recipients/search - Search bill recipients for a specific biller  
🔹 Business Cases  
POST /billers/{billerId}/business-cases - Create business case (PDF upload) for primary network partner  
POST /billers/{billerId}/business-cases - Create business case for secondary network partner (with anomaly detection skipped)  
🔹 Assets Management  
PUT /billers/{billerId}/assets/{assetId} - Update asset (supports PNG, JPEG, GIF, PDF formats)  
GET /billers/{billerId}/assets/{assetId} - Get asset content  
DELETE /billers/{billerId}/assets/{assetId} - Delete asset  
🔹 Events  
GET /events/business-case-status-changed - Get business case status change events  
GET /events/bill-recipient-email-address-changed - Get email address change events  
GET /events/bill-recipient-subscription-status-changed - Get recipient subscription status change events  
🔹 Sectors  
GET /sectors - Get list of available sectors  
🔹 Health Check  
GET /healthcheck - Perform health check on the API  
📋 Headers Required  
x-networkpartner-id: NWID0000006003  
x-correlation-id: Auto-generated UUID for each request  
x-filename: Required for business case creation  
x-anomaly-detection: Optional (value: "SKIP" for secondary network partner)

------------------------------------------------------------------------

## 🎯 Quick Reference

<div>

|  |  |  |  |
|----|----|----|----|
| Category | v5 | v6 | Status |
| **API Base Path** | `/api/pns/networkpartner/v5` | `/api/pns/networkpartner/v6` | 🔴 Changed |
| **OpenAPI Spec** | Swagger 2.0 | OAS 3.0.1 | 🔴 Changed |
| **XML Schema** | `eBill-SIX_v5.xsd` | `eBill-SIX_V6.xsd` | 🔴 Changed |
| **XML Namespace** | `.../v5/ebill/xml` | `.../v6/ebill/xml` | 🔴 Changed |
| **Generic URLs** | Not supported | ✅ Supported | 🟢 New Feature |

</div>

------------------------------------------------------------------------

## 🟢 NEW FEATURES

### Generic Subscription URLs

v6 introduces the ability to create reusable subscription URLs for marketing campaigns and general distribution.

#### New Endpoints

<div>

|  |  |  |
|----|----|----|
| Method | Endpoint | Description |
| **POST** | `/billers/{billerId}/bill-recipient-subscription-initiations-generic-url` | Create generic subscription URL |
| **PUT** | `/billers/{billerId}/bill-recipient-subscription-initiations-generic-url/{tokenId}` | Update generic subscription URL |

</div>

## 📐 VALIDATION CHANGES

### Pattern Updates

<div>

|                                |                |                |               |
|--------------------------------|----------------|----------------|---------------|
| Field                          | v5 Pattern     | v6 Pattern     | Change        |
| `BillerAddress.postalCode`     | XML 1.0 subset | `[\\w -]*`     | ✅ Simplified |
| `BillerAddress.buildingNumber` | N/A            | `[\\w/ -]*`    | 🟢 New field  |
| `RecipientAddress.postalCode`  | XML 1.0 subset | XML 1.0 subset | ⚪ No change  |

</div>

### V6 vs V5 DTO Validation Requirements Comparison

#### 1. Biller Management

<div>

<table style="width:100%;">
<colgroup>
<col style="width: 16%" />
<col style="width: 16%" />
<col style="width: 16%" />
<col style="width: 16%" />
<col style="width: 16%" />
<col style="width: 16%" />
</colgroup>
<tbody>
<tr>
<th><p>DTO Class</p></th>
<th><p>Field Name</p></th>
<th><p>V6 Validations</p></th>
<th><p>V5 Validations</p></th>
<th><p>Changes in V6</p></th>
<th><p>V6 Example</p></th>
</tr>
&#10;<tr>
<td><p><strong>Biller</strong></p></td>
<td><p><strong>ebillDirectDebitSupport</strong></p></td>
<td><p><strong>@NotNull (RENAMED)</strong></p></td>
<td><p><strong>ebillDebitSupport @NotNull</strong></p></td>
<td><p><strong>⚠️ RENAMED from ebillDebitSupport</strong></p></td>
<td><p>ENABLED, DISABLED</p></td>
</tr>
<tr>
<td></td>
<td><p><strong>billRecipientSubscriptionStatus</strong></p></td>
<td><p>@NotNull</p></td>
<td><p>@NotNull</p></td>
<td><p>✓ Same</p></td>
<td><p>ALLOWED, NOT_ALLOWED</p></td>
</tr>
<tr>
<td></td>
<td><p>legalName</p></td>
<td><p>@NotBlank, @Size(min=1, max=70), @Pattern</p></td>
<td><p>@NotBlank, @Size(min=1, max=70), @Pattern</p></td>
<td><p>✓ Same</p></td>
<td><p>Verlag Neuer</p></td>
</tr>
<tr>
<td></td>
<td><p>localizedData</p></td>
<td><p>@NotNull, @Valid</p></td>
<td><p>@NotNull, @Valid</p></td>
<td><p><strong>⚠️ Structure changed - removed address from Biller</strong></p></td>
<td><ul>
<li></li>
</ul></td>
</tr>
<tr>
<td></td>
<td><p>sectorIds</p></td>
<td><p>@NotNull, @Size(min=1, max=100)</p></td>
<td><p>@NotNull, @Size(min=1, max=100)</p></td>
<td><p>✓ Same</p></td>
<td><ul>
<li></li>
</ul></td>
</tr>
<tr>
<td></td>
<td><p>allowedToSubmitDonationInquiries</p></td>
<td><p>@NotNull</p></td>
<td><p>@NotNull</p></td>
<td><p>✓ Same</p></td>
<td><p>true/false</p></td>
</tr>
<tr>
<td></td>
<td><p>billerDirectSubscriptionSupport</p></td>
<td><p>@NotNull</p></td>
<td><p>@NotNull</p></td>
<td><p>✓ Same</p></td>
<td><p>ENABLED, DISABLED</p></td>
</tr>
<tr>
<td></td>
<td><p>certificationIds</p></td>
<td><p>@Size(min=0, max=100)</p></td>
<td><p>@Size(min=0, max=100)</p></td>
<td><p>✓ Same</p></td>
<td><ul>
<li></li>
</ul></td>
</tr>
<tr>
<td><p><strong>LocalizedBillerData</strong></p></td>
<td><p>address</p></td>
<td><p>@NotNull, @Valid</p></td>
<td><p><strong>@NotNull, @Valid (was in Biller)</strong></p></td>
<td><p><strong>⚠️ MOVED from Biller to LocalizedBillerData</strong></p></td>
<td><ul>
<li></li>
</ul></td>
</tr>
<tr>
<td></td>
<td><p>displayName</p></td>
<td><p>@NotBlank, @Size(min=1, max=100), @Pattern</p></td>
<td><p>@NotBlank, @Size(min=1, max=100), @Pattern</p></td>
<td><p>✓ Same</p></td>
<td><p>Neuers Neuste Nachrichten</p></td>
</tr>
<tr>
<td></td>
<td><p>emailAddress</p></td>
<td><p>@Size(min=1, max=256), @Email</p></td>
<td><p>@Size(min=1, max=256), @Email</p></td>
<td><p>✓ Same</p></td>
<td><p><a href="mailto:nnn@nnn-verlag.info" class="external-link" rel="nofollow">nnn@nnn-verlag.info</a></p></td>
</tr>
<tr>
<td></td>
<td><p>website</p></td>
<td><p>@Size(min=1, max=1000), @Pattern</p></td>
<td><p>@Size(min=1, max=1000), @Pattern</p></td>
<td><p>✓ Same</p></td>
<td><p><a href="http://www.nnn-verlag.info" class="external-link" rel="nofollow">http://www.nnn-verlag.info</a></p></td>
</tr>
<tr>
<td><p><strong>BillerAccount</strong></p></td>
<td><p><strong>ebillDirectDebitSupport</strong></p></td>
<td><p><strong>readOnly (RENAMED)</strong></p></td>
<td><p><strong>ebillDebitSupport readOnly</strong></p></td>
<td><p><strong>⚠️ RENAMED from ebillDebitSupport</strong></p></td>
<td><p>ENABLED, DISABLED</p></td>
</tr>
<tr>
<td></td>
<td><p>accountNumber</p></td>
<td><p>@NotNull, @Valid</p></td>
<td><p>@NotNull, @Valid</p></td>
<td><p>✓ Same</p></td>
<td><ul>
<li></li>
</ul></td>
</tr>
<tr>
<td></td>
<td><p>iid</p></td>
<td><p>@NotBlank, @Size(5,5), @Pattern</p></td>
<td><p>@NotBlank, @Size(5,5), @Pattern</p></td>
<td><p>✓ Same</p></td>
<td><p>00100</p></td>
</tr>
<tr>
<td></td>
<td><p>currencyCode</p></td>
<td><p>@NotBlank, @Size(max=3), @Pattern</p></td>
<td><p>@NotBlank, @Size(max=3), @Pattern</p></td>
<td><p>✓ Same</p></td>
<td><p>CHF</p></td>
</tr>
</tbody>
</table>

</div>

#### 2. Bill Recipients & Identification

<div>

<table style="width:100%;">
<colgroup>
<col style="width: 16%" />
<col style="width: 16%" />
<col style="width: 16%" />
<col style="width: 16%" />
<col style="width: 16%" />
<col style="width: 16%" />
</colgroup>
<tbody>
<tr>
<th><p>DTO Class</p></th>
<th><p>Field Name</p></th>
<th><p>V6 Validations</p></th>
<th><p>V5 Validations</p></th>
<th><p>Changes in V6</p></th>
<th><p>V6 Example</p></th>
</tr>
&#10;<tr>
<td><p><strong>BillRecipient</strong></p></td>
<td><p>emailAddress</p></td>
<td><p>@Size(min=1, max=256), @Email</p></td>
<td><p>@Size(min=1, max=256), @Email</p></td>
<td><p>✓ Same</p></td>
<td><p><a href="mailto:peter@muster.ch" class="external-link" rel="nofollow">peter@muster.ch</a></p></td>
</tr>
<tr>
<td></td>
<td><p>billRecipientId</p></td>
<td><p>@NotNull</p></td>
<td><p>@NotNull</p></td>
<td><p>✓ Same</p></td>
<td><p>41010560425610173</p></td>
</tr>
<tr>
<td></td>
<td><p>type</p></td>
<td><p>@NotNull</p></td>
<td><p>@NotNull</p></td>
<td><p>✓ Same</p></td>
<td><p>PRIVATE, COMPANY</p></td>
</tr>
<tr>
<td></td>
<td><p>name</p></td>
<td><p>@NotBlank, @Size(min=1, max=70), @Pattern</p></td>
<td><p>@NotBlank, @Size(min=1, max=70), @Pattern</p></td>
<td><p>✓ Same</p></td>
<td><p>Muster / Muster AG</p></td>
</tr>
<tr>
<td></td>
<td><p>firstName</p></td>
<td><p>@Size(max=35), @Pattern</p></td>
<td><p>@Size(max=35), @Pattern</p></td>
<td><p>✓ Same</p></td>
<td><p>Peter</p></td>
</tr>
<tr>
<td></td>
<td><p>correspondenceLanguage</p></td>
<td><p>@NotBlank, @Size(min=1, max=3)</p></td>
<td><p>@NotBlank, @Size(min=1, max=3)</p></td>
<td><p>✓ Same</p></td>
<td><p>ger</p></td>
</tr>
<tr>
<td></td>
<td><p>address</p></td>
<td><p>@NotNull, @Valid</p></td>
<td><p>@NotNull, @Valid</p></td>
<td><p>✓ Same</p></td>
<td><ul>
<li></li>
</ul></td>
</tr>
<tr>
<td><p><strong>BillRecipientsForBillerSearchResponseItem</strong></p></td>
<td><p><strong>ebillDirectDebitProposalStatus</strong></p></td>
<td><p><strong>@NotNull (RENAMED)</strong></p></td>
<td><p><strong>ebillDebitProposalStatus @NotNull</strong></p></td>
<td><p><strong>⚠️ RENAMED from ebillDebitProposalStatus</strong></p></td>
<td><p>ALLOWED, NOT_ALLOWED</p></td>
</tr>
<tr>
<td></td>
<td><p>ebillSubmissionStatus</p></td>
<td><p>@NotNull</p></td>
<td><p>@NotNull</p></td>
<td><p>✓ Same</p></td>
<td><p>ALLOWED, NOT_ALLOWED</p></td>
</tr>
<tr>
<td></td>
<td><p><strong>allowedEbillDirectDebitSubmissions</strong></p></td>
<td><p><strong>@Valid (RENAMED)</strong></p></td>
<td><p><strong>allowedEbillDebitSubmissions @Valid</strong></p></td>
<td><p><strong>⚠️ RENAMED from allowedEbillDebitSubmissions</strong></p></td>
<td><ul>
<li></li>
</ul></td>
</tr>
</tbody>
</table>

</div>

#### 3. Address Changes

<div>

<table style="width:100%;">
<colgroup>
<col style="width: 16%" />
<col style="width: 16%" />
<col style="width: 16%" />
<col style="width: 16%" />
<col style="width: 16%" />
<col style="width: 16%" />
</colgroup>
<tbody>
<tr>
<th><p>DTO Class</p></th>
<th><p>Field Name</p></th>
<th><p>V6 Validations</p></th>
<th><p>V5 Validations</p></th>
<th><p>Changes in V6</p></th>
<th><p>V6 Example</p></th>
</tr>
&#10;<tr>
<td><p><strong>BillerAddress</strong></p></td>
<td><p>streetName</p></td>
<td><p>@NotBlank, @Size(min=1, max=70), @Pattern</p></td>
<td><p>@NotBlank, @Size(min=1, max=70), @Pattern</p></td>
<td><p>✓ Same</p></td>
<td><p>Neustadtstrasse</p></td>
</tr>
<tr>
<td></td>
<td><p><strong>buildingNumber</strong></p></td>
<td><p><strong>@Size(min=1, max=16), @Pattern</strong></p></td>
<td><p><strong>@Size(min=1, max=16), @Pattern</strong></p></td>
<td><p>✓ Same</p></td>
<td><p>20a</p></td>
</tr>
<tr>
<td></td>
<td><p>postalCode</p></td>
<td><p>@NotBlank, @Size(min=1, max=9), @Pattern</p></td>
<td><p>@NotBlank, @Size(min=1, max=9), @Pattern</p></td>
<td><p>✓ Same</p></td>
<td><p>6025</p></td>
</tr>
<tr>
<td></td>
<td><p>city</p></td>
<td><p>@NotBlank, @Size(min=1, max=35), @Pattern</p></td>
<td><p>@NotBlank, @Size(min=1, max=35), @Pattern</p></td>
<td><p>✓ Same</p></td>
<td><p>Neudorf</p></td>
</tr>
<tr>
<td></td>
<td><p>countryCode</p></td>
<td><p>@NotBlank, @Size(max=2), @Pattern</p></td>
<td><p>@NotBlank, @Size(max=2), @Pattern</p></td>
<td><p>✓ Same</p></td>
<td><p>CH</p></td>
</tr>
<tr>
<td><p><strong>RecipientAddress</strong></p></td>
<td><p><strong>buildingNumber</strong></p></td>
<td><p><strong>N/A (REMOVED)</strong></p></td>
<td><p><strong>@Size(min=1, max=16), @Pattern</strong></p></td>
<td><p><strong>❌ REMOVED in V6</strong></p></td>
<td><ul>
<li></li>
</ul></td>
</tr>
</tbody>
</table>

</div>

#### 4. Currency & Amount Changes

<div>

|  |  |  |  |  |  |  |
|----|----|----|----|----|----|----|
| DTO Class | Field Name | V6 Validations | V5 Validations | Changes in V6 | V6 Example |  |
| **EbillDirectDebitCurrencyCode** | **(Type name)** | \*\*@Size(max=3), @Pattern ("CHF\\ | EUR")\*\* | **EbillDebitCurrenyCode** | **⚠️ RENAMED type (fixed typo: Curreny→Currency) + DirectDebit** | CHF, EUR |
| **OptionalAmountWithCurrency** | value | @DecimalMin("0.00"), @DecimalMax("99999999.99"), @Digits | @DecimalMin, @DecimalMax | ✓ Same | 99.99 |  |
|  | currencyCode | @NotBlank, @Size(max=3), @Pattern | @NotBlank, @Size(max=3), @Pattern | ✓ Same | CHF |  |
| **ApprovalAmountWithCurrency** | value | @NotNull, @DecimalMin("0.01"), @DecimalMax | @NotNull, @DecimalMin, @DecimalMax | ✓ Same | 99.99 |  |
|  | currencyCode | @NotBlank, @Size(max=3), @Pattern | @NotBlank, @Size(max=3), @Pattern | ✓ Same | CHF |  |
| **AmountValue** | maximum | 99999999.99 | 99999999.99 | ✓ Same | 99.99 |  |

</div>

------------------------------------------------------------------------

## ❌ NEW ERROR CODES

### ProblemType Enum Additions

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="9d528d44-1350-48e6-a307-0dba802a2a6d" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
public enum ProblemType {
    // ... existing error codes ...
    
    // ✅ NEW Generic URL Subscription Errors
    VALID_TO_DATE_FOR_GENERIC_BILL_RECIPIENT_SUBSCRIPTION_URL_OUT_OF_RANGE,
    BILLER_MUST_NOT_HAVE_SUBSCRIPTION_FORM_FIELDS,
    
    // ✅ NEW eBill Direct Debit Errors (more specific)
    EBILL_DIRECT_DEBIT_SUPPORT_READ_ONLY_FOR_BILLER_ACCOUNT,
    EBILL_DIRECT_DEBIT_CHARGEBACK_MODE_UNSUPPORTED,
}
```

</div>

</div>

**Handle These in Error Processing:**

- Generic URL validation failures

- Account support read-only violations

- Chargeback mode incompatibilities

------------------------------------------------------------------------

## Migration Analysis: XML eBill-SIX v3 to v6

### **1. Namespace and Version Changes**

- **v3**: `<http://six-group.com/pns/networkpartner/v3/ebill/xml`,\> version="3.3"

- **v6**: `<http://six-group.com/pns/networkpartner/v6/ebill/xml`,\> version="6.4.3"

### **2. Removed: ESR Payment Format**

**v3** had ESR (orange payment slip) support with:

- `accountESRAndReference` complex type

- `esrReferenceStructuredType`

- `esr` element in `accountAndReference` choice

**v6** completely removes ESR support - only IBAN/QR-IBAN remains

### **3. accountAndReference Structure Change**

**v3**: Choice between `generic` (IBAN) or `esr`

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="99eccdc2-f505-4e56-85dd-eafa81f2ab3d" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
<xsd:choice>
  <xsd:element name="generic" type="tns:accountIBANAndReference" minOccurs="1"/>
  <xsd:element name="esr" type="tns:accountESRAndReference" minOccurs="1"/>
</xsd:choice>
```

</div>

</div>

**v6**: Only `generic` element (no choice)

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="af9e8d22-c166-4fd0-9cb8-2968a162cd2f" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
<xsd:element name="generic" type="tns:accountIBANAndReference" minOccurs="1"/>
```

</div>

</div>

### **4. accountHolderType Address Structure**

**v3**: Only structured address (optional)

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="3e37b188-a941-41fb-b9b8-aa9b402a9587" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
<xsd:element name="structuredAddress" type="tns:postalAddress" minOccurs="0"/>
```

</div>

</div>

**v6**: Choice between structured or unstructured address

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="c44f1437-aaef-4b62-9691-9587d7229322" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
<xsd:choice>
  <xsd:element name="structuredAddress" type="tns:postalAddress" minOccurs="1"/>
  <xsd:element name="unstructuredAddress" type="tns:unstructuredAddressType" minOccurs="1"/>
</xsd:choice>
```

</div>

</div>

### **5. referenceType IPI Removed**

**v3** documentation mentioned IPI references (deprecated):

- "IPI - IPI Reference (Note: the IPI receipt was eliminated by 31.03.2020)"

**v6**: IPI references completely removed from documentation

### **6. billRecipient address Field**

**v3**: Address is optional (`minOccurs="0"`)

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="a265dfad-9d30-43f8-a0fd-92017f5adc20" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
<xsd:element name="address" type="tns:billRecipientAddress" minOccurs="0"/>
```

</div>

</div>

**v6**: Address is mandatory (`minOccurs="1"`) with additional validation

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="acd9da30-df1e-4cbc-a8d6-1bb9349529e4" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
<xsd:element name="address" type="tns:billRecipientAddress" minOccurs="1"/>
```

</div>

</div>

Plus, added pain.001 mapping documentation.

### **7. billRecipientAddress minOccurs Changes**

**v3**: Both structured and unstructured addresses are optional (`minOccurs="0"`)

**v6**: Both are mandatory (`minOccurs="1"`)

### **8. unstructuredAddressType Field Changes**

**v3**: All fields optional

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="b87cf020-894b-4996-8041-e6e2d0df419f" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
<xsd:element name="addressLine1" type="tns:optionalStringWithMaxLength70" minOccurs="0"/>
<xsd:element name="addressLine2" type="tns:optionalStringWithMaxLength70" minOccurs="0"/>
<xsd:element name="countryCode" type="tns:countryCodeType" minOccurs="0"/>
```

</div>

</div>

**v6**: addressLine1, addressLine2, countryCode all mandatory (`minOccurs="1"`)

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="905a8ab7-31c8-4e03-946c-c836ce2c1d11" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
<xsd:element name="addressLine1" type="tns:optionalStringWithMaxLength70" minOccurs="1"/>
<xsd:element name="addressLine2" type="tns:optionalStringWithMaxLength70" minOccurs="1"/>
<xsd:element name="countryCode" type="tns:countryCodeType" minOccurs="1"/>
```

</div>

</div>

Plus warning annotation about October 31, 2025 deadline.

### **9. currencyCode Documentation Updates**

**v3**: "When using QR-IBAN or ESR-participant number, only CHF and EUR are allowed."

**v6**: Removed ESR references; added EUR deprecation notices:

- "QR-IBAN submissions for EUR accounts with a due date after 29.10.2027 will be rejected."

- "QR-IBAN submissions for EUR accounts will be rejected starting from 01.10.2027, regardless of the due date."

### **10. paymentInformation dueDate Constraints**

**v3**: Single constraint for all payment types

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="7bc4218a-1dde-4a48-8536-7ff74978c6f5" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
At time of submission, cannot be set to more than 3 years in the future (1095 days).
```

</div>

</div>

**v6**: Different constraints per payment mode

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="13780710-2b13-454b-9977-3e78962f69d1" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
cannot be set to more than 3 years in the future (1095 days) for payment mode ebill.
cannot be set to more than 30 days in the future for payment mode ebill direct debit.
```

</div>

</div>

### **11. singlePayment New Fields**

**v6** adds two new optional fields:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="862e730f-da3e-4472-8bfb-240bf9368340" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
<xsd:element name="paymentMode" type="tns:paymentModeType" minOccurs="1"/>
<xsd:element name="salesChannel" type="tns:salesChannelType" minOccurs="0"/>
```

</div>

</div>

With new types:

- `paymentModeType`: EBILL or EBILL_DIRECT_DEBIT

- `salesChannelType`: ECOMMERCE or OTHER

**v3** doesn't have these fields.

### **12. paymentInformation Documentation**

**v3**: "Information about the account and reference of the invoice issuer. Information about the debit account can be provided in the IBAN format **or the ESR format**, always with the according references."

**v6**: "Information about the account and reference of the invoice issuer. Information about the debit account can be provided in the IBAN format, always with the according references."

------------------------------------------------------------------------

## Summary of Migration Impact

**Breaking Changes:**

1.  ✅ Remove all ESR-related elements and types

2.  ✅ Make billRecipient address mandatory

3.  ✅ Make unstructuredAddress fields mandatory

4.  ✅ Add paymentMode to singlePayment (mandatory)

5.  ✅ Add choice structure to accountHolderType address

6.  ✅ Update namespace from v3 to v6

**Non-Breaking Additions:**

- Add optional salesChannel to singlePayment

- Updated documentation and validation rules

------------------------------------------------------------------------

## 📋 MIGRATION CHECKLIST

### Phase 1: Code Changes

- \[ \] **Update Base URL Configuration**

  - \[ \] Environment variables

  - \[ \] Configuration files

  - \[ \] REST client beans

  - \[ \] Test configurations

- \[ \] **Rename Fields**

  - \[ \] `Biller.ebillDebitSupport` → `ebillDirectDebitSupport`

  - \[ \] `BillerAccount.ebillDebitSupport` → `ebillDirectDebitSupport`

  - \[ \] Update all getters/setters

  - \[ \] Update JSON property annotations

- \[ \] **Update XML Schema**

  - \[ \] Change namespace in `package-info.java`

  - \[ \] Update attachment name in `MetadataAdder.java`

  - \[ \] Update XSD reference from v5 to v6

- \[ \] **Create New Models**

  - \[ \] `BillRecipientSubscriptionInitiationGenericURL`

  - \[ \] `BillRecipientGenericUrlSubscriptionResponse`

  - \[ \] `BillRecipientPersonalizedUrlSubscriptionResponse`

  - \[ \] `SubscriptionAtEbillInitiationTokenId`

  - \[ \] `BillerAccountsAnomalyDetection`

  - \[ \] `BillerLocalizedData`

  - \[ \] `SectorLocalizedData`

  - \[ \] `BillRecipientEmailAddressChangedEventAllOfTriggeredBy`

- \[ \] **Update Existing Models**

  - \[ \] Add `buildingNumber` to `BillerAddress`

  - \[ \] Add `subscriptionSource` to `BillRecipientSubscriptionStatusChangedEvent`

  - \[ \] Add `subscriptionInfo` to `BillRecipientSubscriptionStatusChangedEvent`

  - \[ \] Update `postalCode` pattern in `BillerAddress`

- \[ \] **Update Enums**

  - \[ \] Add `EBILL_CONNECT_PERSONALIZED` to `SubscriptionSource`

  - \[ \] Add `EBILL_CONNECT_GENERIC` to `SubscriptionSource`

  - \[ \] Rename `EbillDebitCurrenyCode` → `EbillDirectDebitCurrencyCode`

  - \[ \] Add new `ProblemType` values

- \[ \] **Update Converters**

  - \[ \] `BillerConverter.java` field mappings

  - \[ \] Any DTOs ↔ Entity mappings

### Phase 2: Service Layer

- \[ \] **Implement Generic URL Subscription**

  - \[ \] Service method for creating generic URLs

  - \[ \] Service method for updating generic URLs

  - \[ \] Validation for `subscriptionInfo` field

  - \[ \] UUID generation/handling for `subscriptionAtEbillInitiationTokenId`

- \[ \] **Update REST Clients**

  - \[ \] Add new endpoints

  - \[ \] Update request/response handling

  - \[ \] Test API integration

### Phase 3: Database (If Applicable)

- \[ \] **Schema Migration**

  - \[ \] Rename column: `ebill_debit_support` → `ebill_direct_debit_support`

  - \[ \] Add column: `building_number` (VARCHAR 16)

  - \[ \] Add column: `subscription_source` (VARCHAR)

  - \[ \] Add column: `subscription_info` (VARCHAR 150)

- \[ \] **Update JPA Mappings**

  - \[ \] `@Column` annotations

  - \[ \] Entity field names

### Phase 4: Testing

- \[ \] **Unit Tests**

  - \[ \] Test field renames

  - \[ \] Test new models

  - \[ \] Test enum additions

  - \[ \] Test validation patterns

- \[ \] **Integration Tests**

  - \[ \] Test v6 API endpoints

  - \[ \] Test XML schema validation

  - \[ \] Test generic URL creation

  - \[ \] Test error handling

- \[ \] **Regression Tests**

  - \[ \] Verify existing functionality

  - \[ \] Test subscription flows

  - \[ \] Test business case submissions
