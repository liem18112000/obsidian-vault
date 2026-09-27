---
ai_hash: 27ffbd02ab6093d2
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 6
depth: 2.76
entities: []
relevance: 0.721
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47153317019/ONE+API+-+Delivery+V2+-+Error+code+checklist
space: LUZ
status: reference
tags:
- confluence
- programming
- space/luz
title: ONE API - Delivery V2 - Error code checklist
topic: programming
type: source
updated: 2023-08-28
---

# ONE API - Delivery V2 - Error code checklist

> [!info] Imported from Confluence
> Space **LUZ** · updated 2023-08-28 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47153317019/ONE+API+-+Delivery+V2+-+Error+code+checklist)
> Relevance 0.721 · topic `programming`

<div>

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<tbody>
<tr>
<th><p><strong>Reponse from One API</strong></p></th>
<th><p><strong>Document error code</strong></p></th>
<th><p><strong>Error message</strong></p></th>
</tr>
&#10;<tr>
<td rowspan="16">

![[47153317019-image-20230824-102304.png]]

</td>
<td><p><code>INVALID_FILE_FORMAT_FOR_EML_LETTER</code></p></td>
<td rowspan="16"><p><span class="confluence-embedded-file-wrapper conf-macro output-inline" data-hasbody="false" data-macro-id="1ef10c1a-4ee7-410f-a85f-147f6d646fc3" data-macro-name="view-file"><a href="../_attachments/47153317019-error_message_en.properties" class="confluence-embedded-file" data-nice-type="null" data-file-src="/wiki/download/attachments/47153317019/error_message_en.properties?version=2&amp;modificationDate=1692871883005&amp;cacheVersion=1&amp;api=v2" data-mime-type="binary/octet-stream" data-has-thumbnail="true">

![[47153317019-error_message_en.properties]]

</a></span></p>
<p>Search the correspondent error message from this file.</p></td>
</tr>
<tr>
<td><p><code>INVALID_FILE_FORMAT_FOR_SMART_LETTER</code></p></td>
</tr>
<tr>
<td><p><code>INVALID_FILE_FORMAT_FOR_SIMPLE_SHORT_MESSAGE</code></p></td>
</tr>
<tr>
<td><p><code>INVALID_FILE_FORMAT_FOR_CLASSIC_LETTER</code></p></td>
</tr>
<tr>
<td><p><code>INVALID_FILE_FORMAT_FOR_PHYSICAL_LETTER</code></p></td>
</tr>
<tr>
<td><p><code>INVALID_SHIPPING_COUNTRY_CODE</code></p></td>
</tr>
<tr>
<td><p><code>INVALID_FILE_FORMAT_FOR_EBILL_LETTER</code></p></td>
</tr>
<tr>
<td><p><code>INVALID_RECIPIENT_FOR_EBILL_LETTER</code></p></td>
</tr>
<tr>
<td><p><code>INVALID_MEDIA_TYPE_FOR_EBILL_LETTER</code></p></td>
</tr>
<tr>
<td><p><code>MISSING_BINARY_FILE_FOR_METADATA</code></p></td>
</tr>
<tr>
<td><p><code>MISSING_METADATA_FOR_BINARY_FILE</code></p></td>
</tr>
<tr>
<td><p><code>METADATA_AND_BINARY_MISMATCH</code></p></td>
</tr>
<tr>
<td><p><code>EMPTY_METADATA_OR_BINARY_FILE_LIST</code></p></td>
</tr>
<tr>
<td><p><code>DOCUMENT_ENRICHMENT_FAILED</code></p></td>
</tr>
<tr>
<td><p><code>INVALID_MEDIA_TYPE</code></p></td>
</tr>
<tr>
<td><p><code>INVALID_MEDIA_TYPE_FOR_EMAIL</code></p></td>
</tr>
<tr>
<td></td>
<td><p><code>NOT_MATCH_ALL_RECIPIENTS</code>( incase allRecipientRequired is true)</p></td>
<td></td>
</tr>
</tbody>
</table>

</div>

<div>

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<tbody>
<tr>
<th><p><strong>Reponse from One API</strong></p></th>
<th><p><strong>Recipient error code</strong></p></th>
<th><p><strong>Error message</strong></p></th>
</tr>
&#10;<tr>
<td rowspan="16">

![[47153317019-image-20230824-104051.png]]

</td>
<td><p><code>PHYSICAL_DELIVERY_FAILED</code></p></td>
<td rowspan="16"><p><span class="confluence-embedded-file-wrapper conf-macro output-inline" data-hasbody="false" data-macro-id="04a8b300-8b86-45ec-b7d5-cd5afe152ca3" data-macro-name="view-file"><a href="../_attachments/47153317019-error_message_en.properties" class="confluence-embedded-file" data-nice-type="null" data-file-src="/wiki/download/attachments/47153317019/error_message_en.properties?version=2&amp;modificationDate=1692871883005&amp;cacheVersion=1&amp;api=v2" data-mime-type="binary/octet-stream" data-has-thumbnail="true">

![[47153317019-error_message_en.properties]]

</a></span></p>
<p>Search the correspondent error message from this file.</p></td>
</tr>
<tr>
<td><p><code>SMS_DELIVERY_FAILED</code></p></td>
</tr>
<tr>
<td><p><code>ADDRESS_INCOMPLETE</code></p></td>
</tr>
<tr>
<td><p><code>NON_MATCHED_RECIPIENT</code> (incase allRecipientRequired is false)</p></td>
</tr>
<tr>
<td><p><code>NOT_MATCH_ALL_RECIPIENTS</code>( incase allRecipientRequired is true )</p></td>
</tr>
<tr>
<td><p><code>SENDER_IN_BLACKLIST</code></p></td>
</tr>
<tr>
<td><p><code>CANNOT_COUNT_NUMBER_OF_PAGE</code></p></td>
</tr>
<tr>
<td><p><code>MISSING_DOCUMENT_MESSAGE</code></p></td>
</tr>
<tr>
<td><p><code>MISSING_MOBILE_NUMBER</code></p></td>
</tr>
<tr>
<td><p><code>MISSING_RECIPIENT_INFO</code></p></td>
</tr>
<tr>
<td><p><code>INVALID_RECIPIENT_FOR_EMAIL_LETTER</code></p></td>
</tr>
<tr>
<td><p><code>MISSING_EMAIL_SUBJECT</code></p></td>
</tr>
<tr>
<td><p><code>MISSING_EMAIL_BODY</code></p></td>
</tr>
<tr>
<td><p><code>RECIPIENT_NAME_REQUIRED</code></p></td>
</tr>
<tr>
<td></td>
</tr>
<tr>
<td></td>
</tr>
</tbody>
</table>

</div>

**Testcases**

<div>

<table>
<colgroup>
<col style="width: 8%" />
<col style="width: 8%" />
<col style="width: 8%" />
<col style="width: 8%" />
<col style="width: 8%" />
<col style="width: 8%" />
<col style="width: 8%" />
<col style="width: 8%" />
<col style="width: 8%" />
<col style="width: 8%" />
<col style="width: 8%" />
<col style="width: 8%" />
</colgroup>
<tbody>
<tr>
<th><p><strong>CHANNEL</strong></p>
<p><strong>(s)</strong></p></th>
<th><p><strong>Document metadata</strong></p></th>
<th><p><strong>Recipient info</strong></p></th>
<th><p><strong>ALL RECIPEINT REQUIRE</strong></p></th>
<th><p><strong>ALL CREDENTIAL SHOULD MATCH</strong></p></th>
<th><p><strong>SENDING STATUS FROM THIRD PARTY</strong></p></th>
<th><p><strong>last delivery channel must try</strong></p></th>
<th><p><strong>Document status</strong></p></th>
<th><p><strong>Document error code</strong></p></th>
<th><p><strong>Recipient error code</strong></p></th>
<th><p><strong>Error message</strong></p></th>
<th><p><strong>Should continue next channel</strong></p></th>
</tr>
&#10;<tr>
<td><p>DIGITAL, SMS</p></td>
<td></td>
<td><p>mobileNumber (invalid)</p></td>
<td></td>
<td></td>
<td></td>
<td><p>SMS</p></td>
<td><p><code>FAILED</code> (<span class="inline-comment-marker" data-ref="c1ef4307-aca9-4dce-9ea1-e4b1b7d31588">new</span>)</p>
<p><del>DELIVERED_WITH_ERROR</del> (old but still using to get the status of delivery)</p></td>
<td></td>
<td><p><code>NON_MATCHED_RECIPIENT</code></p></td>
<td><p><code>Could not find the recipient</code></p></td>
<td><p>x</p></td>
</tr>
<tr>
<td><p>DIGITAL</p></td>
<td></td>
<td><p>mobileNumber (matched)</p></td>
<td></td>
<td></td>
<td><p>succeed</p></td>
<td><p>DIGITAL</p></td>
<td><p><code>DELIVERED</code></p></td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td><p>DIGITAL</p></td>
<td></td>
<td><p>mobileNumber (matched)</p></td>
<td></td>
<td></td>
<td><p>fail to send to recipient</p></td>
<td><p><span class="inline-comment-marker" data-ref="bc036126-97cb-43a0-95d0-029ba56bfa0f">DIGITAL</span></p></td>
<td><p><code>FAILED</code> (new)</p>
<p><del>DELIVERED_WITH_ERROR</del> (old but still using to get the status of delivery)</p></td>
<td></td>
<td><p><code>DIGITAL_DELIVERY_FAILED</code></p></td>
<td><p><code>Could not deliver digitally, internal server error</code></p></td>
<td></td>
</tr>
<tr>
<td><p>SMS</p></td>
<td></td>
<td><p>mobileNumber (invalid)</p></td>
<td></td>
<td></td>
<td><p>fail (invalid mobile phone)</p></td>
<td><p>SMS</p></td>
<td><p><code>FAILED</code> (new)</p>
<p><del>DELIVERED_WITH_ERROR</del> (old but still using to get the status of delivery)</p></td>
<td></td>
<td><p><code>SMS_DELIVERY_FAILED</code></p></td>
<td><p><code>No correct phone numbers</code></p></td>
<td></td>
</tr>
<tr>
<td><p>SMS</p></td>
<td></td>
<td><p>mobileNumber (valid)</p></td>
<td></td>
<td></td>
<td><p>fail to send</p></td>
<td><p>SMS</p></td>
<td><p><code>FAILED</code> (new)</p>
<p><del>DELIVERED_WITH_ERROR</del> (old but still using to get the status of delivery)</p></td>
<td></td>
<td><p><code>SMS_DELIVERY_FAILED</code></p></td>
<td><p><code>Could not deliver SMS, internal server error</code></p></td>
<td></td>
</tr>
<tr>
<td><p>SMS</p></td>
<td></td>
<td><p>mobileNumber (valid)</p></td>
<td></td>
<td></td>
<td><p>succeed</p></td>
<td><p>SMS</p></td>
<td><p><code>DELIVERED</code></p></td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td><p>DIGITAL, EBILL</p></td>
<td></td>
<td><p>email (non match)</p></td>
<td></td>
<td></td>
<td></td>
<td><p>EBILL</p></td>
<td><p><code>FAILED</code> (new)</p>
<p><del>DELIVERED_WITH_ERROR</del> (old but still using to get the status of delivery)</p></td>
<td></td>
<td><p><code>EBILL_DELIVERY_FAILED</code></p></td>
<td><p><code>EBILL_SERVER_UNAVAILABLE</code></p></td>
<td><p>x</p></td>
</tr>
<tr>
<td><p>PHYSICAL</p></td>
<td></td>
<td><p>address</p></td>
<td></td>
<td></td>
<td><p>fail</p></td>
<td><p>PHYSICAL</p></td>
<td><p><code>FAILED</code> (new)</p>
<p><del>DELIVERED_WITH_ERROR</del> (old but still using to get the status of delivery)</p></td>
<td></td>
<td><p><code>PHYSICAL_DELIVERY_FAILED</code></p></td>
<td><p><code>Could not deliver physically, internal server error</code></p></td>
<td></td>
</tr>
<tr>
<td><p>PHYSICAL</p></td>
<td></td>
<td><p>address</p></td>
<td></td>
<td></td>
<td><p>succeed</p></td>
<td><p>PHYSICAL</p></td>
<td><p><code>DELIVERED</code></p></td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td><p>PHYSICAL</p></td>
<td></td>
<td><p>email</p></td>
<td></td>
<td></td>
<td></td>
<td><p>PHYSICAL</p></td>
<td><p><code>FAILED</code> (new)</p>
<p><del>DELIVERED_WITH_ERROR</del> (old but still using to get the status of delivery)</p></td>
<td></td>
<td><p><code>ADDRESS_INCOMPLETE</code></p></td>
<td><p><code>Please provide a complete address for the recipient of this letter</code></p></td>
<td></td>
</tr>
<tr>
<td><p>DIGITAL, PHYSICAL</p></td>
<td></td>
<td><p>participantId (matched)<br />
address (nonmatched)</p></td>
<td></td>
<td></td>
<td><p>succeed</p></td>
<td><p>DIGITAL</p></td>
<td><p><code>DELIVERED</code></p></td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td><p>DIGITAL, PHYSICAL</p></td>
<td></td>
<td><p>participantId (nonmatched)</p>
<p>email (nonmatched)<br />
address (nonmatched)</p></td>
<td></td>
<td></td>
<td><p>succeed</p></td>
<td><p>PHYSICAL</p></td>
<td><p><code>DELIVERED</code></p></td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td><p>DIGITAL, PHYSICAL</p></td>
<td></td>
<td><p>address(nonmatched)</p></td>
<td></td>
<td></td>
<td></td>
<td><p>PHYSICAL</p></td>
<td><p><code>FAILED</code> (new)</p>
<p><del>DELIVERED_WITH_ERROR</del> (old but still using to get the status of delivery)</p></td>
<td></td>
<td><p><code>NON_MATCHED_RECIPIENT</code></p></td>
<td><p><code>Could not find the recipient</code></p></td>
<td><p>x</p></td>
</tr>
<tr>
<td><p>DIGITAL, SMS</p></td>
<td></td>
<td><p>mobileNumber (matched)</p></td>
<td></td>
<td></td>
<td><p>succeed</p></td>
<td><p>DIGITAL</p></td>
<td><p><code>DELIVERED</code></p></td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td><p>DIGITAL, SMS</p></td>
<td></td>
<td><p>mobileNumber (nonmatch but valid)</p></td>
<td></td>
<td></td>
<td><p>succeed</p></td>
<td><p>SMS</p></td>
<td><p><code>DELIVERED</code></p></td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td><p>AUTO</p></td>
<td></td>
<td><p>participantId (nonmatched)</p>
<p>email (nonmatched)<br />
address (nonmatched)</p></td>
<td></td>
<td></td>
<td><p>succeed</p></td>
<td><p>PHYSICAL</p></td>
<td><p><code>DELIVERED</code></p></td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td><p>AUTO</p></td>
<td></td>
<td><p>participantId (macthed)</p>
<p>email (nonmatched)<br />
address (nonmatched)</p></td>
<td></td>
<td></td>
<td><p>succeed</p></td>
<td><p>DIGITAL</p></td>
<td><p><code>DELIVERED</code></p></td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td><p>DIGITAL</p></td>
<td></td>
<td><p>Recipient 1: mobileNumber (matched)</p>
<p>Recipient 2: email (nonmatched)</p></td>
<td><p>x</p></td>
<td></td>
<td></td>
<td><p>DIGITAL</p></td>
<td><p><code>NOT_MATCH_ALL_RECIPIENTS</code></p></td>
<td></td>
<td><p><code>NOT_MATCH_ALL_RECIPIENTS</code></p></td>
<td></td>
<td></td>
</tr>
<tr>
<td><p>DIGITAL, SMS</p></td>
<td></td>
<td><p>Recipient 1: mobileNumber (matched)</p>
<p>Recipient 2: email (nonmatched)</p></td>
<td></td>
<td><p>x</p></td>
<td><p>Recipient 1: succeed</p></td>
<td><p>DIGITAL</p></td>
<td><p><code>FAIL</code>(new)</p>
<p><del>DELIVERED_WITH_ERROR</del> (old but still using to get the status of delivery)</p></td>
<td></td>
<td><p>Recipient 2:</p>
<p><code>NON_MATCHED_RECIPIENT</code></p></td>
<td><p><code>Could not find the recipient</code></p></td>
<td></td>
</tr>
<tr>
<td><p>DIGITAL, SMS</p></td>
<td></td>
<td><p>Recipient 1:</p>
<p>mobileNumber (matched)</p>
<p>email (nonmatched)</p>
<p>Recipient 2:</p>
<p>email (nonmatched)</p></td>
<td><p>x</p></td>
<td><p>x</p></td>
<td><p>succeed</p></td>
<td><p>SMS</p></td>
<td><p><code>DELIVERED</code></p></td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td colspan="12"></td>
</tr>
<tr>
<td><p>any</p></td>
<td><p>format is incorrect</p></td>
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
<td><p><code>FAIL</code></p></td>
<td><p><code>FILE_CONTAINS_MALWARE</code></p></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td><p>any</p></td>
<td><p>upload large file</p></td>
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
<td><p><code>FAIL</code></p></td>
<td><p><code>DOCUMENT_SIZE_EXCEEDED</code></p></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td><p>DIGITAL</p></td>
<td></td>
<td></td>
<td></td>
<td></td>
<td><p>fail to store file to luz-docs</p></td>
<td></td>
<td><p><code>FAILED</code> (<span class="inline-comment-marker" data-ref="c1ef4307-aca9-4dce-9ea1-e4b1b7d31588">new</span>)</p>
<p><del>DELIVERED_WITH_ERROR</del> (old but still using to get the status of delivery)</p></td>
<td><p><code>DOCUMENT_IMPORT_UNKNOWN_ERROR</code></p></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td><p>AUTO</p></td>
<td><p>missing upload file</p></td>
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
<td><p><code>FAILED</code> (new)</p>
<p><del>DELIVERED_WITH_ERROR</del> (old but still using to get the status of delivery)</p></td>
<td><p><code>MISSING_BINARY_FILE_FOR_METADATA</code></p></td>
<td></td>
<td><p><code>Missing binary file for metadata with fileName: {0}</code></p></td>
<td></td>
</tr>
<tr>
<td><p>AUTO</p></td>
<td><p>missing defining fileName</p></td>
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
<td><p><code>FAILED</code> (new)</p>
<p><del>DELIVERED_WITH_ERROR</del> (old but still using to get the status of delivery)</p></td>
<td><p><code>MISSING_FILE_NAME_VALUE</code></p></td>
<td></td>
<td><p><code>Missing metadata for binary file with fileName: {0}</code></p></td>
<td></td>
</tr>
<tr>
<td><p>AUTO</p></td>
<td><p>missing document message</p></td>
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
<td><p><code>FAILED</code> (new)</p>
<p><del>DELIVERED_WITH_ERROR</del> (old but still using to get the status of delivery)</p></td>
<td><p><code>MISSING_DOCUMENT_MESSAGE</code></p></td>
<td></td>
<td><p><code>Missing Document Message field</code></p></td>
<td></td>
</tr>
<tr>
<td><p>AUTO</p></td>
<td><p>invalid mediaType</p></td>
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
<td><p><code>FAILED</code> (new)</p>
<p><del>DELIVERED_WITH_ERROR</del> (old but still using to get the status of delivery)</p></td>
<td><p><code>INVALID_MEDIA_TYPE</code></p></td>
<td></td>
<td><p><code>Not support to deliver this media type {0}</code></p></td>
<td></td>
</tr>
<tr>
<td><p>AUTO</p></td>
<td><p>invalid file format</p></td>
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
<td><p><code>FAILED</code> (new)</p>
<p><del>DELIVERED_WITH_ERROR</del> (old but still using to get the status of delivery)</p></td>
<td><p><code>INVALID_FILE_FORMAT_FOR_PHYSICAL_LETTER</code> or<br />
<code>INVALID_FILE_FORMAT_FOR_EML_LETTER</code> or</p>
<p><code>INVALID_FILE_FORMAT_FOR_CLASSIC_LETTER</code> or</p>
<p><code>INVALID_FILE_FORMAT_FOR_SMART_LETTER</code> or</p>
<p><code>INVALID_FILE_FORMAT_FOR_SIMPLE_SHORT_MESSAGE</code></p></td>
<td></td>
<td><p><code>Physical letter only accept PDF file</code> or</p>
<p><code>IncaMail letter only accept EML file</code> or</p>
<p><code>Classic letter only accept PDF or TXT file</code> or</p>
<p><code>Smart letter only accept JSON file</code> or</p>
<p><code>Simple short message only accept PDF or TXT or HTML file</code></p></td>
<td></td>
</tr>
</tbody>
</table>

</div>

%% ai-graph-start %%

**Related notes:**
- [[Preview Delivery-Prices API - ForcedOnboading]]
- [[5. How to extend modify ONE API delivery API Research]]
- [[Copy 5. How to extend modify ONE API delivery API Research]]
- [[Invoice API Reference]]
- [[One API - Investigation]]

%% ai-graph-end %%