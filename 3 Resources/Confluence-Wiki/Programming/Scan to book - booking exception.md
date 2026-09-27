---
title: "Scan to book - booking exception"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/Helios/pages/47811428613/Scan+to+book+-+booking+exception
space: "Helios"
topic: programming
relevance: 0.792
depth: 3
updated: 2024-05-21
attachments: 4
tags:
  - confluence
  - programming
  - space/helios
---

# Scan to book - booking exception

> [!info] Imported from Confluence
> Space **Helios** · updated 2024-05-21 · [open original](https://axonivy.atlassian.net/wiki/spaces/Helios/pages/47811428613/Scan+to+book+-+booking+exception)
> Relevance 0.792 · topic `programming`

In this page, we list out the exceptions from luz-accounting. Decide to handle which exception in myKlara

------------------------------------------------------------------------

## From luz-accounting

From the API

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="b6e2cca7-9d8e-467f-b1f7-640aba9025e2" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
@POST "../luz-accounting/api/../../companies/{companyId}/business-cases?isBooking=true"
```

</div>

</div>

<div id="expander-869758615" class="expand-container conf-macro output-block" hasbody="true" macro-id="f4dc3e5c-507e-41bd-83af-49dd4fd5a9dc" macro-name="expand">

<div id="expander-control-869758615" class="expand-control">

<span class="expand-control-icon icon"> </span><span class="expand-control-text">1. Accounting validation for fields: exception ValidationException(msg)</span>

</div>

<div id="expander-content-869758615" class="expand-content expand-hidden">

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="3beea8a0-e9d5-4fc3-aa9f-77806ea6d644" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
BusinessCaseService.handleBusinessCase
    businessCaseTemplateService.findEntityById
        NotFoundException
    validateBusinessCaseInput
        ValidatorHelper.validate
            ValidationException
        validateBusinessCase
            validateBusinessCaseOperators
                ValidationException
                    "Business case with OR operator should choose one snippet. There is no selected snippet in list ("
                    "Business case with OR operator should choose only one snippet. There are more than one selected snippet ("
                    "Business case with AND operator should have all snippets selected. There are snippets which is not selected ("
            validateFieldConstraints
                ValidationException
                    Mostly validate fields
    
    Find busines case by businessCaseId, if bookingNumber available
        ValidationException("Cannot change the business case which is booked")
```

</div>

</div>

</div>

</div>

<div id="expander-26402773" class="expand-container conf-macro output-block" hasbody="true" macro-id="8a97fc6e-356e-4ef0-bc27-3df0e236129e" macro-name="expand">

<div id="expander-control-26402773" class="expand-control">

<span class="expand-control-icon icon"> </span><span class="expand-control-text">2. booking will trigger core booking function, potential errors</span>

</div>

<div id="expander-content-26402773" class="expand-content expand-hidden">

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="6b71fc4e-1008-41c7-92da-497bc281ee98" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
ValidatorHelper.validate
    ValidationException 
        ConstraintViolation

BookingValidator.validateAccountingSubscription
    AccountingException:
        BundleMessageConstant.INVALID_SUBSCRIPTION_FOR_ACCOUNTING

BookingCaseService.handleBusinessCase
    doBookingWithDetailErrorShowing -> BookingService.createBookingHeaders() -> validateBookingDate()
        AccountingException
            BundleMessageConstant.FISCAL_YEAR_NOT_FOUND
            "Booking date {0} is out of selected fiscal year {1} - {2}!"    

BookingValidator.validateFullyBookingHeader()
    BookingValidator.validateBookingInFiscalYear
        AccountingException: 
            BundleMessageConstant.FISCAL_YEAR_IS_SEALED
            BundleMessageConstant.FISCAL_YEAR_IS_CLOSING
            BundleMessageConstant.FISCAL_YEAR_NOT_FOUND
    
    validateTransactionLinks
        AccountingException:
            BundleMessageConstant.BOOKING_DETAIL_HAS_BANK_TRANSACTION_ID_NOT_FOUND
            BundleMessageConstant.BOOKING_DETAIL_BANK_ACCOUNT_URI_NOT_EXISTED
            BundleMessageConstant.BOOKING_DETAIL_BANK_ACCOUNT_IBAN_NOT_MATCH_IBAN_OF_BANK_TRANSACTION
    
    BookingValidator.validatePartiallyBookingHeader
        BookingValidator.validateBookingHeaderMandatoryFields
            AccountingException:
                BundleMessageConstant.VALID_BOOKING_HEADER_NOT_NULL
                BundleMessageConstant.BOOKING_DATE_MUST_BE_SPECIFIED
                BundleMessageConstant.DOCUMENT_DATE_MUST_BE_SPECIFIED
                BundleMessageConstant.BOOKING_STATUS_MUST_BE_SPECIFIED
                BundleMessageConstant.WRONG_SAVING_BOOKING_STATUS
        
        BookingDetailValidator.validateBookingDetailForSaving
            validateMinimumNumberOfBookingDetails
                AccountingException:
                    BundleMessageConstant.VALID_BOOKING_DETAIL_SIZE_NOT_LESS_THAN_TWO
                
            validateBookingDetailMandatoryFields
                AccountingException:
                    BundleMessageConstant.VALID_BOOKING_DETAIL_NOT_NULL
                    BundleMessageConstant.BOOKING_TYPE_CODE_MUST_BE_SPECIFIED
                    BundleMessageConstant.VALID_ACCOUNT_CODE_NOT_SUPPORTED
                    BundleMessageConstant.VALID_ACCOUNT_TYPE_MUST_BE_SPECIFIC_IN_BOOKING_DETAIL
                    BundleMessageConstant.VALID_AMOUNT_MUST_BE_SPECIFIC_IN_BOOKING_DETAIL
                    BundleMessageConstant.TAG_MUST_BE_SPECIFIC_IN_BOOKING_DETAIL
                    BundleMessageConstant.DESCRIPTION_MUST_BE_SPECIFIED
            validateCreditDebitMismatched
                AccoutingException:
                    BundleMessageConstant.INVALID_ACCOUNTING_RECORD
                    BundleMessageConstant.BOOKING_MUST_BE_HAS_BOTH_CR_AND_DR
            validateVatBookingDetail
                AccountingException:
                    BundleMessageConstant.BOOKING_DETAIL_VAT_BOOKING_DETAIL_BUT_DONT_SPECIFY_LINK
                    BundleMessageConstant.BOOKING_DETAIL_VAT_BOOKING_DETAIL_MUST_NOT_HAS_VALUE_VAT_AMOUNT_AND_VAT_RATE
                    BundleMessageConstant.BOOKING_DETAIL_NO_VAT_BOOKING_DETAIL_MUST_NOT_HAS_VALUE_VAT_AMOUNT_AND_VAT_RATE
            validatePairVatBookingDetail
                AccountingException:
                    BundleMessageConstant.BOOKING_DETAIL_VAT_BOOKING_DETAIL_MISSING_LINK_TO_OTHER
                    BundleMessageConstant.BOOKING_DETAIL_VAT_BOOKING_DETAIL_VAT_RATE_HAS_TO_BE_NUMBER_AND_POSITIVE
        
    BookingDetailValidator.validateBookingAccount
        AccountingException:
            BundleMessageConstant.VALID_BOOKING_DETAIL_NOT_NULL
            BundleMessageConstant.VALID_ACCOUNT_CODE_NOT_SUPPORTED
            BundleMessageConstant.LINK_MUST_BE_SPECIFIC_IN_BOOKING_DETAIL
            
validateBookingDetailLinks
    ValidationException:
        "Can't find the booking detail with uri: %s"
        "The booking detail with type code %s cannot be link to %s. Account code: %s, booking header id: %s"
```

</div>

</div>

</div>

</div>

------------------------------------------------------------------------

## Luz-mobile and exceptions

<div id="expander-2144528130" class="expand-container conf-macro output-block" hasbody="true" macro-id="05193c89-d76b-461c-9bec-6334b7b86633" macro-name="expand">

<div id="expander-control-2144528130" class="expand-control">

<span class="expand-control-icon icon"> </span><span class="expand-control-text">Define exceptions to be handled</span>

</div>

<div id="expander-content-2144528130" class="expand-content expand-hidden">

<div>

<table>
<colgroup>
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
</colgroup>
<tbody>
<tr>
<th><p><strong>Errors</strong></p></th>
<th><p><strong>luz-accounting exception type</strong></p></th>
<th><p><strong>luz-mobile exception</strong></p></th>
<th><p><strong>Note</strong></p></th>
</tr>
&#10;<tr>
<td><p>Entity not found:</p>
<p>(when business case template id not found)</p>
<p>Validate Input:</p>
<ul>
<li><p>Business case constraint validation</p></li>
<li><p>Business case operators (AND/OR/…)</p></li>
<li><p>Validate field constraint → check json data reference</p></li>
</ul></td>
<td><div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="a6b51a03-09b1-4fbb-9767-77aebe634177" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>NotFoundException</code></pre>
</div>
</div>
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="57872e5f-68ee-405a-821b-e2629f2411c7" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>ValidationException</code></pre>
</div>
</div></td>
<td><p>(common)</p>
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="8fe156a4-06cb-491e-997e-9929552bbabb" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>accounting.booking.create.failed</code></pre>
</div>
</div></td>
<td><p>technical issue</p></td>
</tr>
<tr>
<td><p>validate subscription</p>
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="392ccf61-b799-41b8-a465-49cb8e456e51" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>BundleMessageConstant.INVALID_SUBSCRIPTION_FOR_ACCOUNTING</code></pre>
</div>
</div></td>
<td><div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="9bbd66ee-8a0a-4e47-9ce4-e6d2ec3aa621" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>AccountingException</code></pre>
</div>
</div></td>
<td><p>subscription error code</p>
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="2b5073ad-cb0d-4bae-a817-72e8019e47fa" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>accounting.booking.create.failed</code></pre>
</div>
</div></td>
<td><p><span class="inline-comment-marker" data-ref="170cce23-5b63-4823-8348-85b8cf51d686">accounting subscription error → need error code</span></p></td>
</tr>
<tr>
<td><p>Validate financial year</p>
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="601d607e-af6b-4389-83eb-67ca47690838" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>FISCAL_YEAR_IS_SEALED
FISCAL_YEAR_IS_CLOSING</code></pre>
</div>
</div></td>
<td><div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="a692bd22-7317-46e6-b863-8a6c9a53ac1c" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>AccountingException</code></pre>
</div>
</div></td>
<td><p>fiscal year close error code</p>
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="7e69bc44-4a4b-422c-9311-98fe3d985ed8" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>accounting.booking.create.failed</code></pre>
</div>
</div></td>
<td><p><span class="inline-comment-marker" data-ref="e0d0c2a7-9952-448b-8d59-515baaec3f13">fiscal year closed exception → need error code → do we have this in luz-mobile?</span></p></td>
</tr>
<tr>
<td><div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="85bdc1fa-02e7-4f86-b3b7-c197006e097e" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>&quot;Booking date {0} is out of selected fiscal year {1} - {2}!&quot;</code></pre>
</div>
</div></td>
<td><div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="35c13cae-0630-4a66-a32f-45d1ac6c1597" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>AccountingException</code></pre>
</div>
</div></td>
<td><p>(common)</p>
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="cc1f07a7-a898-4126-ad32-8d99e1b4fe60" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>accounting.booking.create.failed</code></pre>
</div>
</div></td>
<td><p><span class="inline-comment-marker" data-ref="2fe12983-373a-4278-a66f-ae42ef65f4d1">find &amp; compare string → handle as special case</span></p></td>
</tr>
<tr>
<td><p>Validate for transaction links in booking detail: “<code>transaction_links</code>“</p>
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="10fc1e31-a238-4bb5-aa8f-06f10c4d142b" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>BOOKING_DETAIL_HAS_BANK_TRANSACTION_ID_NOT_FOUND
BOOKING_DETAIL_BANK_ACCOUNT_URI_NOT_EXISTED
BOOKING_DETAIL_BANK_ACCOUNT_IBAN_NOT_MATCH_IBAN_OF_BANK_TRANSACTION</code></pre>
</div>
</div></td>
<td><div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="1eafae83-1e70-415a-b3e1-81bdcdbd6608" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>AccountingException</code></pre>
</div>
</div></td>
<td><p>(common)</p>
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="476d254a-da50-4a0a-b3ab-c12453742e75" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>accounting.booking.create.failed</code></pre>
</div>
</div></td>
<td><p><span class="inline-comment-marker" data-ref="08149cef-a27d-4e0e-bc73-3b281b190a56">technical issue, check with team to see if we could show to user some message?</span></p></td>
</tr>
<tr>
<td><p>validate booking header mandatory field</p>
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="1cb680d0-5883-479b-afa8-61ab98e1244b" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>VALID_BOOKING_HEADER_NOT_NULL
BOOKING_DATE_MUST_BE_SPECIFIED
DOCUMENT_DATE_MUST_BE_SPECIFIED
BOOKING_STATUS_MUST_BE_SPECIFIED
WRONG_SAVING_BOOKING_STATUS</code></pre>
</div>
</div></td>
<td><div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="2fb68ed7-7463-413f-9752-38915dfd2013" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>AccountingException</code></pre>
</div>
</div></td>
<td><p>(common)</p>
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="f803df86-0eaa-44ac-9105-696c4edd381a" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>accounting.booking.create.failed</code></pre>
</div>
</div></td>
<td><p>This case shouldn’t be happens, we do validate on client side already.</p>
<p><span class="inline-comment-marker" data-ref="b96fcf89-9e39-468e-b2a8-c4b13a670f22">But support as API level, can have specific error code?</span></p></td>
</tr>
<tr>
<td><p>validate minimum number of booking detail</p>
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="9c332bd5-aafa-4349-acca-6357ef8ad06d" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>VALID_BOOKING_DETAIL_SIZE_NOT_LESS_THAN_TWO</code></pre>
</div>
</div></td>
<td><div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="94ba1b72-23b4-476d-bb2e-2d84690e9272" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>AccountingException</code></pre>
</div>
</div></td>
<td><p>(common)</p>
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="d8592e1c-0106-44f7-9869-c5a48b538b89" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>accounting.booking.create.failed</code></pre>
</div>
</div></td>
<td><p>Not sure how can it happens? ideally it will be an technical error</p></td>
</tr>
<tr>
<td><p>validate booking detail mandatory fields</p>
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="5d4d30db-075c-4f74-a86a-d4353274f890" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>VALID_BOOKING_DETAIL_NOT_NULL
BOOKING_TYPE_CODE_MUST_BE_SPECIFIED
VALID_ACCOUNT_CODE_NOT_SUPPORTED
VALID_ACCOUNT_TYPE_MUST_BE_SPECIFIC_IN_BOOKING_DETAIL
VALID_AMOUNT_MUST_BE_SPECIFIC_IN_BOOKING_DETAIL
TAG_MUST_BE_SPECIFIC_IN_BOOKING_DETAIL
DESCRIPTION_MUST_BE_SPECIFIED</code></pre>
</div>
</div></td>
<td><div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="7cb98f73-6278-4d34-b294-5185420264f3" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>AccountingException</code></pre>
</div>
</div></td>
<td><p>(common)</p>
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="5f242069-7328-4ba1-9378-f758223f9366" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>accounting.booking.create.failed</code></pre>
</div>
</div></td>
<td><p>already validate on client side</p></td>
</tr>
<tr>
<td><p>Validate CR/DR</p>
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="643339c2-b58f-4f05-af63-f60be9239ace" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>INVALID_ACCOUNTING_RECORD
BOOKING_MUST_BE_HAS_BOTH_CR_AND_DR</code></pre>
</div>
</div></td>
<td><div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="e10f311f-1a46-4db5-86a7-fd8d57f60efd" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>AccountingException</code></pre>
</div>
</div></td>
<td><p>CR/DR error code</p>
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="e8f7a1ab-71f7-473f-81af-712f752fec3c" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>&quot;invalid.accounting.record&quot;</code></pre>
</div>
</div></td>
<td><p>need specific error for “INVALID_ACCOUNTING_RECORD” → easier for us to narrow error scope if user face CR/DR exception</p></td>
</tr>
<tr>
<td><p>validate VAT booking detail</p>
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="ef1106c2-2515-4e0f-ac1b-8c4a6af8e874" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>BOOKING_DETAIL_VAT_BOOKING_DETAIL_BUT_DONT_SPECIFY_LINK
BOOKING_DETAIL_VAT_BOOKING_DETAIL_MUST_NOT_HAS_VALUE_VAT_AMOUNT_AND_VAT_RATE
BOOKING_DETAIL_NO_VAT_BOOKING_DETAIL_MUST_NOT_HAS_VALUE_VAT_AMOUNT_AND_VAT_RATE</code></pre>
</div>
</div></td>
<td><div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="1acf5f80-dbbe-484c-a15c-6b44855271d8" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>AccountingException</code></pre>
</div>
</div></td>
<td><p>(common)</p>
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="8f449b26-a050-4a0c-9bf5-5717d7580bd7" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>accounting.booking.create.failed</code></pre>
</div>
</div></td>
<td><p><span class="inline-comment-marker" data-ref="8fb1d6b2-f55d-4194-81a8-edf79dcd6fc3">common exception, VAT or no VAT has been handled on client, data should be corrected</span></p></td>
</tr>
<tr>
<td><p>validate pair vat booking detail</p>
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="71c297af-4843-482b-9e37-bd36d5bbc303" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>BOOKING_DETAIL_VAT_BOOKING_DETAIL_MISSING_LINK_TO_OTHER
BOOKING_DETAIL_VAT_BOOKING_DETAIL_VAT_RATE_HAS_TO_BE_NUMBER_AND_POSITIVE</code></pre>
</div>
</div></td>
<td><div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="f36b6c59-2081-4784-8857-c3127cb42f68" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>AccountingException</code></pre>
</div>
</div></td>
<td><p>(common)</p>
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="22774cfe-df62-4453-b8ce-8160d7aa2aec" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>accounting.booking.create.failed</code></pre>
</div>
</div></td>
<td><p>technical error</p></td>
</tr>
<tr>
<td><p>validate booking detail</p>
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="a35f4ed3-c239-4f3f-a95b-62d3b0c72bc5" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>VALID_BOOKING_DETAIL_NOT_NULL
VALID_ACCOUNT_CODE_NOT_SUPPORTED
LINK_MUST_BE_SPECIFIC_IN_BOOKING_DETAIL</code></pre>
</div>
</div></td>
<td><div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="679b799c-02e6-4311-9073-50faefc7bf3d" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>AccountingException</code></pre>
</div>
</div></td>
<td><p>(common)</p>
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="e3d5b1a8-c51f-46a5-8057-8ebb03217c03" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>accounting.booking.create.failed</code></pre>
</div>
</div></td>
<td><p>technical error</p></td>
</tr>
<tr>
<td><p>validate booking detail links</p>
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="ad6088ef-8166-4026-b5c8-44ff713227ba" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>&quot;Can&#39;t find the booking detail with uri: %s&quot;
&quot;The booking detail with type code %s cannot be link to %s. Account code: %s, booking header id: %s&quot;</code></pre>
</div>
</div></td>
<td><div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="d8e18519-e3d4-4ec8-bb3b-3fa41ad2f488" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>AccountingException</code></pre>
</div>
</div></td>
<td><p>(common)</p>
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="a208f580-d861-44d2-a922-294cb11c789d" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>accounting.booking.create.failed</code></pre>
</div>
</div></td>
<td><p>technical error,</p>
<p>but should log the error</p></td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

Sample response from luz-mobile

<div>

<table>
<colgroup>
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
</colgroup>
<tbody>
<tr>
<th><p><strong>Create booking CR/DR error</strong></p></th>
<th><p><strong>Common business error</strong></p></th>
<th><p><strong>Default error</strong></p></th>
<th><p><strong>Success case</strong></p></th>
</tr>
&#10;<tr>
<td><div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="c8a2e493-7588-44a7-b54a-c40bfbf6f4d8" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>{
    &quot;status&quot;: &quot;400&quot;,
    &quot;message&quot;: &quot;Invalid accounting record: debit/credit mismatch. DR: Optional[108.10], CR: Optional[108.09]&quot;,
    &quot;code&quot;: 400,
    &quot;errorId&quot;: &quot;invalid.accounting.record&quot;,
    &quot;data&quot;: null
}</code></pre>
</div>
</div></td>
<td><div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="ab84acca-5550-47d1-a1c7-d177114417ec" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>{
    &quot;status&quot;: &quot;400&quot;,
    &quot;message&quot;: &quot;Booking date 2020-02-02 is out of selected fiscal year 2024-01-01 - 2024-12-31!&quot;,
    &quot;code&quot;: 400,
    &quot;errorId&quot;: &quot;accounting.booking.create.failed&quot;,
    &quot;data&quot;: null
}</code></pre>
</div>
</div></td>
<td><div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="14ac9b8a-e8ea-4a36-b06a-ce1683dca87e" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>{
    &quot;status&quot;: &quot;500&quot;,
    &quot;message&quot;: &quot;Could not book business case due to RESTEASY004655: Unable to invoke request: org.apache.http.conn.HttpHostConnectException: Connect to host.docker.internal:8085 [host.docker.internal/192.168.127.254] failed: Connection refused&quot;,
    &quot;code&quot;: 500,
    &quot;errorId&quot;: &quot;accounting.booking.create.failed&quot;,
    &quot;data&quot;: null
}</code></pre>
</div>
</div></td>
<td></td>
</tr>
<tr>
<td>

![[47811428613-Untitled-20240521-102643.png]]

</td>
<td>

![[47811428613-Untitled-20240521-102656.png]]

</td>
<td>

![[47811428613-Untitled-20240521-102713.png]]

</td>
<td>

![[47811428613-Untitled-20240521-102734.png]]

</td>
</tr>
</tbody>
</table>

</div>
