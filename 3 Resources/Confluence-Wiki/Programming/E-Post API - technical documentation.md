---
title: "E-Post API - technical documentation"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20525652813/E-Post+API+-+technical+documentation
space: "LUZ"
topic: programming
relevance: 0.755
depth: 2.81
updated: 2026-01-20
attachments: 22
tags:
  - confluence
  - programming
  - space/luz
---

# E-Post API - technical documentation

> [!info] Imported from Confluence
> Space **LUZ** · updated 2026-01-20 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20525652813/E-Post+API+-+technical+documentation)
> Relevance 0.755 · topic `programming`

<div class="contentLayout2">

<div class="columnLayout single">

<div class="cell normal" data-type="normal">

<div class="innerCell">

<div class="toc-macro client-side-toc-macro conf-macro output-block" hasbody="false" headerelements="H1,H2,H3,H4,H5,H6,H7" macro-id="2a1d660b-74d6-4af5-9fc4-3c713471ba10" macro-name="toc">

</div>

# ePost basic flow introduction

<span class="confluence-embedded-file-wrapper conf-macro output-inline" hasbody="false" macro-id="44be468e-c8a9-4fd7-8601-98540f279ffa" macro-name="view-file"><a href="../_attachments/20525652813-epost_basic_flow.pptx" class="confluence-embedded-file" data-nice-type="Microsoft PowerPoint Presentation" data-file-src="/wiki/download/attachments/20525652813/epost_basic_flow.pptx?version=1&amp;modificationDate=1648719841042&amp;cacheVersion=1&amp;api=v2" data-mime-type="application/vnd.openxmlformats-officedocument.presentationml.presentation" data-has-thumbnail="true">

![[20525652813-epost_basic_flow.pptx]]

</a></span>

# Public API technical specs

API Rate Limit:

- 10000 / Minute per Sender

V2 delivery Limits:

- Size per document (file + metadata) \< 200MB.

- Whole request \< 2GB

Identity matching before delivery limits:

- 50'000 recipients / request

# Identity Matching

## Levels of Trust

The Levels of Trust (LoT) define how strong a user is verified in the KLARA ecosystem. We defined three possible level of trust:

- **BRONZE**

  - The user has verified his E-Mail address by entering a code / link sent by E-Mail

- **SILVER**

  - The user has verified his postal address by entering a code sent by physical letter

- **GOLD**

  - The user is verified as a person by an online identification process where he has to upload a picture of an official identity document and a live-taken self-picture

## Credentials Private Customers


![[20525652813-star_yellow.png]]

 = Primary Credentials needs to be delivered with other non-unique credentials

- E-Mail address 

![[20525652813-star_yellow.png]]

 

- Mobile phone number 

![[20525652813-star_yellow.png]]



- Postal address 

![[20525652813-star_yellow.png]]

 

  - First name(s)

  - Last name

  - <span class="inline-comment-marker" ref="d84b72ae-969c-4ad2-b9fd-d66fbc43300e">Street</span>

  - <span class="inline-comment-marker" ref="d84b72ae-969c-4ad2-b9fd-d66fbc43300e">Street number</span>

  - ZIP Code

  - City

- <span class="inline-comment-marker" ref="c8844d90-9880-45b3-8e06-46e7fec90126">Participant ID (</span>

![[20525652813-star_yellow.png]]

<span class="inline-comment-marker" ref="c8844d90-9880-45b3-8e06-46e7fec90126">)</span>

- Key Address Hash 

![[20525652813-star_yellow.png]]



- <span class="inline-comment-marker" ref="a97121cb-6c6b-4681-b089-b2caa0e62d0d">Legacy ID (EPOF-Unique Key) </span>

![[20525652813-star_yellow.png]]



- Birth Date

- Social security number

- Tax ID

## Credentials Business Customers


![[20525652813-star_yellow.png]]

 = Primary Credentials that are unique and needs to be delivered with other non-unique credentials

- Postal address 

![[20525652813-star_yellow.png]]

 

  - CompanyName

  - Street

  - Street number

  - ZIP Code

  - City

- Participant ID (

![[20525652813-star_yellow.png]]

)

- Key Address Hash 

![[20525652813-star_yellow.png]]



- UID 

![[20525652813-star_yellow.png]]

 (no more primary credential on beginning of July 22)

## Process

The identity matching process defines how the senders can check if a user exists on the KLARA ecosystem by providing some credentials of them.   
There are two ways an identity matching process; before a delivery or with the delivery of a document.

### **<span class="inline-comment-marker" ref="7a271223-c309-4978-a624-9f7cf5ad116b">Business View</span>**


![[20525652813-IdentityMatching_beforeDelivery.png]]




![[20525652813-IM_ondelivery.png]]



### Technical View (DRAFT)

#### Possible Matchings

For a matching there is **<u>always</u>** the need of a unique credential. That means one of the following credentials are necessary:

- Postal address (with first- and last name or with company name)

- E-Mail address

- Mobile phone number

- Participant ID

- Key Address Hash

- UID from a company

<span class="inline-comment-marker" ref="b0ef102c-3978-440a-8c6a-8b08cbd8e1d0">If there isn't any of these credentials in the request we give an error back to the sender.</span>  
Beside that it is possible that every combination of credentials can be delivered by the sender.

Please notice: This list needs to be adaptable (if once there is another "unique identifier").

##### Priority of Matching

For a faster matching it makes sense to first match the unique credentials (from above).  
When a unique credential matches we can look at the other provided credentials if they match and don't need to look for matches on the whole directory with every credential.

As the credentials could be hashed I would recommend the following priority:

1.  PID → When the sender sends a PID we need to check if the PID exists and if yes, we can check other provided credentials on correctness.

2.  Key Address Hash → When the sender sends a Key Address Hash, we check if it exists. If it does, we can verify the other provided credentials (except Postal Address). The Key Address Hash represents the Postal Address; if it matches, we ignore the Postal Address and continue verifying other credentials.

3.  UID → When the sender sends a UID we know that he wants to match a business tenant.

4.  Legacy-ID → When the sender provides a Legacy-ID we know that the sender wants to verify if this user exists on the new platform. If this ID exists we need to check the other provided credentials. 

5.  E-Mail Address hashed → As the E-Mail Address is standardized we have a high probability that we find the hash value when we have the user on our platform. 

6.  Postal Address hashed → The hashed postal address allows us to check for the address without normalizing the address.

7.  Mobile Phone Number hashed

8.  E-Mail Address → Same as the hashed value, the E-Mail address is standardized and therefore we don't need to normalize it to find a match.

9.  Mobile Phone Number → As we specify the way a mobile phone number needs to be delivered we can find matches fast. 

10. Postal Address → As the postal address is sent to the Swiss Post to normalize I set this credential to the end (of the unique credentials) for performance reasons.

11. Other (non-unique) credentials → As we absolutely need one of the unique credentials above we don't need to look for the other credentials before a match is made with a unique <span class="inline-comment-marker" ref="fa93fea9-f792-48f2-aab5-d6df643c4222">credentials.</span> 

##### <span class="inline-comment-marker" ref="a571a25b-0bdb-4ac9-8f88-58a450977554">Cases of Matching</span>

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
<th><p><strong>Case</strong></p>
<p><strong>Nr</strong></p></th>
<th><p><strong>Provided credentials</strong><br />

![[20525652813-check.png]]

<strong> = matched</strong><br />

![[20525652813-error.png]]

<strong> = not matched</strong></p></th>
<th><p><strong>LoT corresponds (yes / no)</strong></p></th>
<th><p><strong>Recipient matched (yes / no)</strong></p></th>
<th><p><strong>Response put in...</strong></p></th>
<th><p><strong>Error Message for Sender</strong></p></th>
</tr>
&#10;<tr>
<td><p><strong>1</strong></p></td>
<td><div>
<table>
<colgroup>
<col style="width: 12%" />
<col style="width: 12%" />
<col style="width: 12%" />
<col style="width: 12%" />
<col style="width: 12%" />
<col style="width: 12%" />
<col style="width: 12%" />
<col style="width: 12%" />
</colgroup>
<tbody>
<tr>
<th><p><strong>E-Mail</strong></p></th>
<th><p><strong>Mobile</strong></p></th>
<th><p><strong>Postal Address (complete)</strong></p></th>
<th><p><strong>Participant ID</strong></p></th>
<th><p><strong>LegacyID</strong></p></th>
<th><p><strong>UID</strong></p></th>
<th><p><strong>BirthDate</strong></p></th>
<th><p><strong>Future unique credential</strong></p></th>
</tr>
&#10;<tr>
<td><p>

![[20525652813-check.png]]

</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
</tr>
</tbody>
</table>
</div></td>
<td><p>Yes</p></td>
<td><p><strong>Yes</strong></p></td>
<td><p>matchedRecipients</p></td>
<td><p>null</p></td>
</tr>
<tr>
<td><p><strong>2</strong></p></td>
<td><div>
<table>
<colgroup>
<col style="width: 12%" />
<col style="width: 12%" />
<col style="width: 12%" />
<col style="width: 12%" />
<col style="width: 12%" />
<col style="width: 12%" />
<col style="width: 12%" />
<col style="width: 12%" />
</colgroup>
<tbody>
<tr>
<th><p><strong>E-Mail</strong></p></th>
<th><p><strong>Mobile</strong></p></th>
<th><p><strong>Postal Address (complete)</strong></p></th>
<th><p><strong>Participant ID</strong></p></th>
<th><p><strong>LegacyID</strong></p></th>
<th><p><strong>UID</strong></p></th>
<th><p><strong>BirthDate</strong></p></th>
<th><p><strong>Future unique credential</strong></p></th>
</tr>
&#10;<tr>
<td><p>(

![[20525652813-check.png]]

)</p></td>
<td><p>(

![[20525652813-check.png]]

)</p></td>
<td><p>

![[20525652813-check.png]]

</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
<td><p>

![[20525652813-error.png]]

</p></td>
<td><p><br />
</p></td>
</tr>
</tbody>
</table>
</div></td>
<td><p>Yes</p></td>
<td><p><strong>Yes</strong></p></td>
<td><p>matchedRecipients</p></td>
<td><p>Credential not matched: Birth Date</p></td>
</tr>
<tr>
<td><p><strong><span class="inline-comment-marker" data-ref="5ca22925-8984-4e81-ad7a-a3b631041dd1">3</span></strong></p></td>
<td><div>
<table>
<colgroup>
<col style="width: 12%" />
<col style="width: 12%" />
<col style="width: 12%" />
<col style="width: 12%" />
<col style="width: 12%" />
<col style="width: 12%" />
<col style="width: 12%" />
<col style="width: 12%" />
</colgroup>
<tbody>
<tr>
<th><p><strong>E-Mail</strong></p></th>
<th><p><strong>Mobile</strong></p></th>
<th><p><strong>Postal Address (complete)</strong></p></th>
<th><p><strong>Participant ID</strong></p></th>
<th><p><strong>LegacyID</strong></p></th>
<th><p><strong>UID</strong></p></th>
<th><p><strong>BirthDate</strong></p></th>
<th><p><strong>Future unique credential</strong></p></th>
</tr>
&#10;<tr>
<td><p>

![[20525652813-check.png]]

</p></td>
<td><p>

![[20525652813-error.png]]

</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
</tr>
</tbody>
</table>
</div></td>
<td><p>Yes</p></td>
<td><p><strong>Yes</strong></p></td>
<td><p>matchedRecipients</p></td>
<td><p>Credential not matched: Mobile Phone Number</p></td>
</tr>
<tr>
<td><p><strong>4</strong></p></td>
<td><div>
<table>
<colgroup>
<col style="width: 12%" />
<col style="width: 12%" />
<col style="width: 12%" />
<col style="width: 12%" />
<col style="width: 12%" />
<col style="width: 12%" />
<col style="width: 12%" />
<col style="width: 12%" />
</colgroup>
<tbody>
<tr>
<th><p><strong>E-mail</strong></p></th>
<th><p><strong>Mobile</strong></p></th>
<th><p><strong>Postal Address (complete)</strong></p></th>
<th><p><strong>Participant ID</strong></p></th>
<th><p><strong>LegacyID</strong></p></th>
<th><p><strong>UID</strong></p></th>
<th><p><strong>BirthDate</strong></p></th>
<th><p><strong>Future unique credential</strong></p></th>
</tr>
&#10;<tr>
<td><p>

![[20525652813-error.png]]

</p></td>
<td><p>

![[20525652813-check.png]]

</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
</tr>
</tbody>
</table>
</div></td>
<td><p>Yes</p></td>
<td><p><strong>Yes</strong></p></td>
<td><p>matchedRecipients</p></td>
<td><p>Credential not matched: E-Mail</p></td>
</tr>
<tr>
<td><p><strong>5</strong></p></td>
<td><div>
<table>
<colgroup>
<col style="width: 12%" />
<col style="width: 12%" />
<col style="width: 12%" />
<col style="width: 12%" />
<col style="width: 12%" />
<col style="width: 12%" />
<col style="width: 12%" />
<col style="width: 12%" />
</colgroup>
<tbody>
<tr>
<th><p><strong>E-Mail</strong></p></th>
<th><p><strong>Mobile</strong></p></th>
<th><p><strong>Postal Address (complete)</strong></p></th>
<th><p><strong>Participant ID</strong></p></th>
<th><p><strong>LegacyID</strong></p></th>
<th><p><strong>UID</strong></p></th>
<th><p><strong>BirthDate</strong></p></th>
<th><p><strong>Future unique credential</strong></p></th>
</tr>
&#10;<tr>
<td><p>

![[20525652813-check.png]]

</p></td>
<td><p>

![[20525652813-check.png]]

</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
</tr>
</tbody>
</table>
</div></td>
<td><p>No</p></td>
<td><p><strong>No</strong></p></td>
<td><p>nonMatchedRecipients</p></td>
<td><p>Level of Trust did not match</p></td>
</tr>
<tr>
<td><p><strong>6</strong></p></td>
<td><div>
<table>
<colgroup>
<col style="width: 12%" />
<col style="width: 12%" />
<col style="width: 12%" />
<col style="width: 12%" />
<col style="width: 12%" />
<col style="width: 12%" />
<col style="width: 12%" />
<col style="width: 12%" />
</colgroup>
<tbody>
<tr>
<th><p><strong>E-Mail</strong></p></th>
<th><p><strong>Mobile</strong></p></th>
<th><p><strong>Postal Address (complete)</strong></p></th>
<th><p><strong>Participant ID</strong></p></th>
<th><p><strong>LegacyID</strong></p></th>
<th><p><strong>UID</strong></p></th>
<th><p><strong>BirthDate</strong></p></th>
<th><p><strong>Future unique credential</strong></p></th>
</tr>
&#10;<tr>
<td><p>

![[20525652813-error.png]]

</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
<td><p>

![[20525652813-check.png]]

</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
</tr>
</tbody>
</table>
</div></td>
<td><p>Yes</p></td>
<td><p><strong>Yes</strong></p></td>
<td><p>matchedRecipients</p></td>
<td><p>Credential not matched: E-Mail Address</p></td>
</tr>
<tr>
<td><p><strong>7</strong></p></td>
<td><div>
<table>
<colgroup>
<col style="width: 12%" />
<col style="width: 12%" />
<col style="width: 12%" />
<col style="width: 12%" />
<col style="width: 12%" />
<col style="width: 12%" />
<col style="width: 12%" />
<col style="width: 12%" />
</colgroup>
<tbody>
<tr>
<th><p><strong>E-Mail</strong></p></th>
<th><p><strong>Mobile</strong></p></th>
<th><p><strong>Postal Address (complete)</strong></p></th>
<th><p><strong>Participant ID</strong></p></th>
<th><p><strong>LegacyID</strong></p></th>
<th><p><strong>UID</strong></p></th>
<th><p><strong>BirthDate</strong></p></th>
<th><p><strong>Future unique credential</strong></p></th>
</tr>
&#10;<tr>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
<td><p>

![[20525652813-check.png]]

</p></td>
<td><p><br />
</p></td>
</tr>
</tbody>
</table>
</div></td>
<td><p>?</p></td>
<td><p><strong>No</strong></p></td>
<td><p>nonMatchedRecipients</p></td>
<td><p>No unique credential provided</p></td>
</tr>
<tr>
<td><p><strong>8</strong></p></td>
<td><div>
<table>
<colgroup>
<col style="width: 12%" />
<col style="width: 12%" />
<col style="width: 12%" />
<col style="width: 12%" />
<col style="width: 12%" />
<col style="width: 12%" />
<col style="width: 12%" />
<col style="width: 12%" />
</colgroup>
<tbody>
<tr>
<th><p><strong>E-Mail</strong></p></th>
<th><p><strong>Mobile</strong></p></th>
<th><p><strong>Postal Address (complete)</strong></p></th>
<th><p><strong>Participant ID</strong></p></th>
<th><p><strong>LegacyID</strong></p></th>
<th><p><strong>UID</strong></p></th>
<th><p><strong>BirthDate</strong></p></th>
<th><p><strong>Future unique credential</strong></p></th>
</tr>
&#10;<tr>
<td><p>

![[20525652813-error.png]]

</p></td>
<td><p>

![[20525652813-error.png]]

</p></td>
<td><p>

![[20525652813-error.png]]

</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
</tr>
</tbody>
</table>
</div></td>
<td><p>?</p></td>
<td><p><strong>No</strong></p></td>
<td><p>nonMatchedRecipients</p></td>
<td><p><span class="inline-comment-marker" data-ref="76c24015-4e41-4e3c-980d-bc7f80f75639">Resource / recipient not found</span></p></td>
</tr>
<tr>
<td><p><strong>9</strong></p></td>
<td><div>
<table>
<colgroup>
<col style="width: 12%" />
<col style="width: 12%" />
<col style="width: 12%" />
<col style="width: 12%" />
<col style="width: 12%" />
<col style="width: 12%" />
<col style="width: 12%" />
<col style="width: 12%" />
</colgroup>
<tbody>
<tr>
<th><p><strong>E-Mail</strong></p></th>
<th><p><strong>Mobile</strong></p></th>
<th><p><strong>Postal Address (complete)</strong></p></th>
<th><p><strong>Participant ID</strong></p></th>
<th><p><strong>LegacyID</strong></p></th>
<th><p><strong>UID</strong></p></th>
<th><p><strong>BirthDate</strong></p></th>
<th><p><strong>Future unique credential</strong></p></th>
</tr>
&#10;<tr>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
<td><p>

![[20525652813-check.png]]

</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
</tr>
</tbody>
</table>
</div></td>
<td><p>Yes</p></td>
<td><p><strong>Yes</strong></p></td>
<td><p>matchedRecipients</p></td>
<td><p>null</p></td>
</tr>
<tr>
<td><p><strong>10</strong></p></td>
<td><div>
<table>
<colgroup>
<col style="width: 12%" />
<col style="width: 12%" />
<col style="width: 12%" />
<col style="width: 12%" />
<col style="width: 12%" />
<col style="width: 12%" />
<col style="width: 12%" />
<col style="width: 12%" />
</colgroup>
<tbody>
<tr>
<th><p><strong>E-Mail</strong></p></th>
<th><p><strong>Mobile</strong></p></th>
<th><p><strong>Postal Address (complete)</strong></p></th>
<th><p><strong>Participant ID</strong></p></th>
<th><p><strong>LegacyID</strong></p></th>
<th><p><strong>UID</strong></p></th>
<th><p><strong>BirthDate</strong></p></th>
<th><p><strong>Future unique credential</strong></p></th>
</tr>
&#10;<tr>
<td><p><br />
</p></td>
<td><p>

![[20525652813-error.png]]

</p></td>
<td><p>

![[20525652813-check.png]]

</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
</tr>
</tbody>
</table>
</div></td>
<td><p>Yes</p></td>
<td><p><strong>Yes</strong></p></td>
<td><p>matchedRecipients</p></td>
<td><p>Credential not matched: Mobile Phone Number</p></td>
</tr>
<tr>
<td><p><strong>11</strong></p></td>
<td><div>
<table>
<colgroup>
<col style="width: 12%" />
<col style="width: 12%" />
<col style="width: 12%" />
<col style="width: 12%" />
<col style="width: 12%" />
<col style="width: 12%" />
<col style="width: 12%" />
<col style="width: 12%" />
</colgroup>
<tbody>
<tr>
<th><p><strong>E-Mail</strong></p></th>
<th><p><strong>Mobile</strong></p></th>
<th><p><strong>Postal Address (complete)</strong></p></th>
<th><p><strong>Participant ID</strong></p></th>
<th><p><strong>LegacyID</strong></p></th>
<th><p><strong>UID</strong></p></th>
<th><p><strong>BirthDate</strong></p></th>
<th><p><strong>Future unique credential</strong></p></th>
</tr>
&#10;<tr>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
<td><p>

![[20525652813-check.png]]

</p></td>
</tr>
</tbody>
</table>
</div></td>
<td><p>Yes</p></td>
<td><p><strong>Yes</strong></p></td>
<td><p>matchedRecipients</p></td>
<td><p>null</p></td>
</tr>
<tr>
<td><p><strong>12</strong></p></td>
<td><div>
<table>
<colgroup>
<col style="width: 12%" />
<col style="width: 12%" />
<col style="width: 12%" />
<col style="width: 12%" />
<col style="width: 12%" />
<col style="width: 12%" />
<col style="width: 12%" />
<col style="width: 12%" />
</colgroup>
<tbody>
<tr>
<th><p><strong>E-Mail</strong></p></th>
<th><p><strong>Mobile</strong></p></th>
<th><p><strong>Postal Address (complete)</strong></p></th>
<th><p><strong>Participant ID</strong></p></th>
<th><p><strong>LegacyID</strong></p></th>
<th><p><strong>UID</strong></p></th>
<th><p><strong>BirthDate</strong></p></th>
<th><p><strong>Future unique credential</strong></p></th>
</tr>
&#10;<tr>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
<td><p>

![[20525652813-check.png]]

</p>
<p>First name, Last name,</p>
<p>Street + Nr.</p>
<p>Zip, city</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
</tr>
</tbody>
</table>
</div></td>
<td><p>Yes</p></td>
<td><p><strong>Yes</strong></p></td>
<td><p>matchedRecipients</p></td>
<td><p>null</p></td>
</tr>
<tr>
<td><p><strong>13</strong></p></td>
<td><div>
<table>
<colgroup>
<col style="width: 12%" />
<col style="width: 12%" />
<col style="width: 12%" />
<col style="width: 12%" />
<col style="width: 12%" />
<col style="width: 12%" />
<col style="width: 12%" />
<col style="width: 12%" />
</colgroup>
<tbody>
<tr>
<th><p><strong>E-Mail</strong></p></th>
<th><p><strong>Mobile</strong></p></th>
<th><p><strong>Postal Address (complete)</strong></p></th>
<th><p><strong>Participant ID</strong></p></th>
<th><p><strong>LegacyID</strong></p></th>
<th><p><strong>UID</strong></p></th>
<th><p><strong>BirthDate</strong></p></th>
<th><p><strong>Future unique credential</strong></p></th>
</tr>
&#10;<tr>
<td><p>

![[20525652813-error.png]]

</p></td>
<td><p>

![[20525652813-error.png]]

</p></td>
<td><p>

![[20525652813-check.png]]

</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
</tr>
</tbody>
</table>
</div></td>
<td><p>Yes</p></td>
<td><p><strong>Yes</strong></p></td>
<td><p>matchedRecipients</p></td>
<td><p>Credential not matched: Mobile Phone Number</p></td>
</tr>
<tr>
<td><p><strong>14</strong></p></td>
<td><div>
<table>
<colgroup>
<col style="width: 12%" />
<col style="width: 12%" />
<col style="width: 12%" />
<col style="width: 12%" />
<col style="width: 12%" />
<col style="width: 12%" />
<col style="width: 12%" />
<col style="width: 12%" />
</colgroup>
<tbody>
<tr>
<th><p><strong>E-Mail</strong></p></th>
<th><p><strong>Mobile</strong></p></th>
<th><p><strong>Postal Address (complete)</strong></p></th>
<th><p><strong>Participant ID</strong></p></th>
<th><p><strong>LegacyID</strong></p></th>
<th><p><strong>UID</strong></p></th>
<th><p><strong>BirthDate</strong></p></th>
<th><p><strong>Future unique credential</strong></p></th>
</tr>
&#10;<tr>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
<td><p>Company Name</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
<td><p>

![[20525652813-check.png]]

</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
</tr>
</tbody>
</table>
</div></td>
<td><p>Yes</p></td>
<td><p><strong>Yes</strong></p></td>
<td><p>matchedRecipients</p></td>
<td><p>null</p></td>
</tr>
<tr>
<td><p><strong>15</strong></p></td>
<td><div>
<table>
<colgroup>
<col style="width: 12%" />
<col style="width: 12%" />
<col style="width: 12%" />
<col style="width: 12%" />
<col style="width: 12%" />
<col style="width: 12%" />
<col style="width: 12%" />
<col style="width: 12%" />
</colgroup>
<tbody>
<tr>
<th><p><strong>E-Mail</strong></p></th>
<th><p><strong>Mobile</strong></p></th>
<th><p><strong>Postal Address (complete)</strong></p></th>
<th><p><strong>Participant ID</strong></p></th>
<th><p><strong>LegacyID</strong></p></th>
<th><p><strong>UID</strong></p></th>
<th><p><strong>BirthDate</strong></p></th>
<th><p><strong>Future unique credential</strong></p></th>
</tr>
&#10;<tr>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
<td><p>

![[20525652813-check.png]]

</p>
<p>Company name</p>
<p>Street + Nr</p>
<p>zip, city</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
</tr>
</tbody>
</table>
</div></td>
<td><p>Yes</p></td>
<td><p><strong>Yes</strong></p></td>
<td><p>matchedRecipients</p></td>
<td><p>null</p></td>
</tr>
<tr>
<td><p><strong>16 </strong></p></td>
<td><div>
<table>
<colgroup>
<col style="width: 11%" />
<col style="width: 11%" />
<col style="width: 11%" />
<col style="width: 11%" />
<col style="width: 11%" />
<col style="width: 11%" />
<col style="width: 11%" />
<col style="width: 11%" />
<col style="width: 11%" />
</colgroup>
<tbody>
<tr>
<th><p><strong>E-Mail</strong></p></th>
<th><p><strong>Mobile</strong></p></th>
<th><p><strong>Postal Address (complete)</strong></p></th>
<th><p><strong>Participant ID</strong></p></th>
<th><p><strong>LegacyID</strong></p></th>
<th><p><strong>UID</strong></p></th>
<th><p><strong>BirthDate</strong></p></th>
<th><p><strong>Future unique credential</strong></p></th>
<th><p><strong>Tax ID</strong></p></th>
</tr>
&#10;<tr>
<td><p>

![[20525652813-check.png]]

</p></td>
<td><p>or (

![[20525652813-check.png]]

)</p></td>
<td><p>or (

![[20525652813-check.png]]

)</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
<td><p>

![[20525652813-check.png]]

</p></td>
</tr>
</tbody>
</table>
</div></td>
<td><p>Yes</p>
<p>(Bronze, silver, gold)</p></td>
<td><p><strong>Yes</strong></p></td>
<td><p>matchedRecipients</p></td>
<td><p>null</p></td>
</tr>
<tr>
<td><p><strong>17 </strong></p></td>
<td><div>
<table>
<colgroup>
<col style="width: 11%" />
<col style="width: 11%" />
<col style="width: 11%" />
<col style="width: 11%" />
<col style="width: 11%" />
<col style="width: 11%" />
<col style="width: 11%" />
<col style="width: 11%" />
<col style="width: 11%" />
</colgroup>
<tbody>
<tr>
<th><p><strong>E-Mail</strong></p></th>
<th><p><strong>Mobile</strong></p></th>
<th><p><strong>Postal Address (complete)</strong></p></th>
<th><p><strong>Participant ID</strong></p></th>
<th><p><strong>LegacyID</strong></p></th>
<th><p><strong>UID</strong></p></th>
<th><p><strong>BirthDate</strong></p></th>
<th><p><strong>Future unique credential</strong></p></th>
<th><p><strong>Tax ID</strong></p></th>
</tr>
&#10;<tr>
<td><p>

![[20525652813-check.png]]

</p></td>
<td><p>or (

![[20525652813-check.png]]

)</p></td>
<td><p>or (

![[20525652813-check.png]]

)</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
<td><p>

![[20525652813-error.png]]

</p></td>
</tr>
</tbody>
</table>
</div></td>
<td><p>Yes</p>
<p>(Bronze, silver, gold)</p></td>
<td><p><strong>Yes </strong></p></td>
<td><p>MatchedRecipients</p></td>
<td><p>Tax ID did not match</p></td>
</tr>
<tr>
<td><p><strong>18 </strong></p></td>
<td><div>
<table>
<colgroup>
<col style="width: 11%" />
<col style="width: 11%" />
<col style="width: 11%" />
<col style="width: 11%" />
<col style="width: 11%" />
<col style="width: 11%" />
<col style="width: 11%" />
<col style="width: 11%" />
<col style="width: 11%" />
</colgroup>
<tbody>
<tr>
<th><p><strong>E-Mail</strong></p></th>
<th><p><strong>Mobile</strong></p></th>
<th><p><strong>Postal Address (complete)</strong></p></th>
<th><p><strong>Participant ID</strong></p></th>
<th><p><strong>LegacyID</strong></p></th>
<th><p><strong>UID</strong></p></th>
<th><p><strong>BirthDate</strong></p></th>
<th><p><strong>Future unique credential</strong></p></th>
<th><p><strong>Tax ID</strong></p></th>
</tr>
&#10;<tr>
<td><p>

![[20525652813-check.png]]

</p></td>
<td><p>or (

![[20525652813-check.png]]

)</p></td>
<td><p>

![[20525652813-error.png]]

</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
</tr>
</tbody>
</table>
</div></td>
<td><p>Yes</p>
<p>silver</p></td>
<td><p><strong>Yes </strong></p></td>
<td><p>MatchedRecipients</p></td>
<td><p>Postal address did not match</p></td>
</tr>
<tr>
<td colspan="6"><p><strong>Matching cases when primary credentials are not unique anymore</strong></p></td>
</tr>
<tr>
<td><p><strong>19</strong></p></td>
<td><div>
<table style="width:100%;">
<colgroup>
<col style="width: 10%" />
<col style="width: 10%" />
<col style="width: 10%" />
<col style="width: 10%" />
<col style="width: 10%" />
<col style="width: 10%" />
<col style="width: 10%" />
<col style="width: 10%" />
<col style="width: 10%" />
<col style="width: 10%" />
</colgroup>
<tbody>
<tr>
<th><p><strong>UID</strong></p></th>
<th><p><strong>E-Mail</strong></p></th>
<th><p><strong>Mobile</strong></p></th>
<th><p><strong>Postal Address (complete)</strong></p></th>
<th><p><strong>Participant ID</strong></p></th>
<th><p><strong>LegacyID</strong></p></th>
<th><p><strong>UID</strong></p></th>
<th><p><strong>BirthDate</strong></p></th>
<th><p><strong>Future unique credential</strong></p></th>
<th><p><strong>Tax ID</strong></p></th>
</tr>
&#10;<tr>
<td><p>1</p></td>
<td><p>

![[20525652813-check.png]]

</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
</tr>
<tr>
<td><p>2</p></td>
<td><p>

![[20525652813-check.png]]

</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
</tr>
</tbody>
</table>
</div></td>
<td><p>Yes</p></td>
<td><p><strong>Yes, both</strong></p></td>
<td><p>MatchedRecipients</p></td>
<td></td>
</tr>
<tr>
<td><p><strong>20</strong></p></td>
<td><div>
<table style="width:100%;">
<colgroup>
<col style="width: 10%" />
<col style="width: 10%" />
<col style="width: 10%" />
<col style="width: 10%" />
<col style="width: 10%" />
<col style="width: 10%" />
<col style="width: 10%" />
<col style="width: 10%" />
<col style="width: 10%" />
<col style="width: 10%" />
</colgroup>
<tbody>
<tr>
<th><p><strong>UID</strong></p></th>
<th><p><strong>E-Mail</strong></p></th>
<th><p><strong>Mobile</strong></p></th>
<th><p><strong>Postal Address (complete)</strong></p></th>
<th><p><strong>Participant ID</strong></p></th>
<th><p><strong>LegacyID</strong></p></th>
<th><p><strong>UID</strong></p></th>
<th><p><strong>BirthDate</strong></p></th>
<th><p><strong>Future unique credential</strong></p></th>
<th><p><strong>Tax ID</strong></p></th>
</tr>
&#10;<tr>
<td><p>1</p></td>
<td><p>

![[20525652813-check.png]]

</p></td>
<td><p><br />
</p></td>
<td><p>

![[20525652813-check.png]]

</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
</tr>
<tr>
<td><p>2</p></td>
<td><p>

![[20525652813-check.png]]

</p></td>
<td><p><br />
</p></td>
<td><p>

![[20525652813-error.png]]

</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
</tr>
</tbody>
</table>
</div></td>
<td><p>Yes</p></td>
<td><p><strong>Yes, both</strong><br />
<br />
"Allcredentials" boolean would help → Only 1</p></td>
<td></td>
<td></td>
</tr>
<tr>
<td><p><strong>21</strong></p></td>
<td><div>
<table style="width:100%;">
<colgroup>
<col style="width: 10%" />
<col style="width: 10%" />
<col style="width: 10%" />
<col style="width: 10%" />
<col style="width: 10%" />
<col style="width: 10%" />
<col style="width: 10%" />
<col style="width: 10%" />
<col style="width: 10%" />
<col style="width: 10%" />
</colgroup>
<tbody>
<tr>
<th><p><strong>UID</strong></p></th>
<th><p><strong>E-Mail</strong></p></th>
<th><p><strong>Mobile</strong></p></th>
<th><p><strong>Postal Address (complete)</strong></p></th>
<th><p><strong>Participant ID</strong></p></th>
<th><p><strong>LegacyID</strong></p></th>
<th><p><strong>UID</strong></p></th>
<th><p><strong>BirthDate</strong></p></th>
<th><p><strong>Future unique credential</strong></p></th>
<th><p><strong>Tax ID</strong></p></th>
</tr>
&#10;<tr>
<td><p>1</p></td>
<td><p>

![[20525652813-check.png]]

</p></td>
<td><p><br />
</p></td>
<td><p>

![[20525652813-check.png]]

</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
</tr>
<tr>
<td><p>2</p></td>
<td><p>

![[20525652813-check.png]]

</p></td>
<td><p><br />
</p></td>
<td><p>

![[20525652813-error.png]]

</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
</tr>
</tbody>
</table>
</div></td>
<td><p>Only 1 of them</p></td>
<td><p>No, not both</p></td>
<td></td>
<td></td>
</tr>
<tr>
<td><p><strong>22</strong></p></td>
<td><div>
<table style="width:100%;">
<colgroup>
<col style="width: 10%" />
<col style="width: 10%" />
<col style="width: 10%" />
<col style="width: 10%" />
<col style="width: 10%" />
<col style="width: 10%" />
<col style="width: 10%" />
<col style="width: 10%" />
<col style="width: 10%" />
<col style="width: 10%" />
</colgroup>
<tbody>
<tr>
<th><p><strong>UID</strong></p></th>
<th><p><strong>E-Mail</strong></p></th>
<th><p><strong>Mobile</strong></p></th>
<th><p><strong>Postal Address (complete)</strong></p></th>
<th><p><strong>Participant ID</strong></p></th>
<th><p><strong>LegacyID</strong></p></th>
<th><p><strong>UID</strong></p></th>
<th><p><strong>BirthDate</strong></p></th>
<th><p><strong>Future unique credential</strong></p></th>
<th><p><strong>Tax ID</strong></p></th>
</tr>
&#10;<tr>
<td><p>1</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
</tr>
<tr>
<td><p>2</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
</tr>
</tbody>
</table>
</div></td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td><p><strong>23</strong></p></td>
<td><div>
<table style="width:100%;">
<colgroup>
<col style="width: 10%" />
<col style="width: 10%" />
<col style="width: 10%" />
<col style="width: 10%" />
<col style="width: 10%" />
<col style="width: 10%" />
<col style="width: 10%" />
<col style="width: 10%" />
<col style="width: 10%" />
<col style="width: 10%" />
</colgroup>
<tbody>
<tr>
<th><p><strong>UID</strong></p></th>
<th><p><strong>E-Mail</strong></p></th>
<th><p><strong>Mobile</strong></p></th>
<th><p><strong>Postal Address (complete)</strong></p></th>
<th><p><strong>Participant ID</strong></p></th>
<th><p><strong>LegacyID</strong></p></th>
<th><p><strong>UID</strong></p></th>
<th><p><strong>BirthDate</strong></p></th>
<th><p><strong>Future unique credential</strong></p></th>
<th><p><strong>Tax ID</strong></p></th>
</tr>
&#10;<tr>
<td><p>1</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
</tr>
<tr>
<td><p>2</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
</tr>
</tbody>
</table>
</div></td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
</tbody>
</table>

</div>

### QUESTION: is it really enough to have only 1 primary credential as a match? Or always all provided credentials should be a match?

Discussed with Daniel and Anis on 17.May.2021.

We keep the logic as it is for 1.7.21 Afterwards, Arrow will add and extra flag. if the sender wants to match all the credentials, then KLARA will only match if all credentials are matched.


![[20525652813-Matching_Priority.png]]



<div id="ap-com.mxgraph.confluence.plugins.plantuml__plantumlcloud5112709506154888445" class="ap-container conf-macro output-block" hasbody="false" macro-id="fab57e43-4a5c-4475-bd6c-a9d0cf0285a2" macro-name="plantumlcloud">

<div id="embedded-com.mxgraph.confluence.plugins.plantuml__plantumlcloud5112709506154888445" class="ap-content">

</div>

</div>

#### Clear Text Postal address Matching (to be defined)

To get the right address we use the <a href="https://developer.post.ch/en/address-web-services-rest" class="external-link" rel="nofollow">address-service provided by Swiss Post </a>to normalize the address (without names). 

Let's make an example: Sender A wants to deliver a document to a person with the following credentials:

FirstName: Peter  
LastName: Muster  
<span class="inline-comment-marker" ref="3f6d3245-ef4a-4544-8221-84a9f55736c1">StreetName: Bahnhofsstr.</span>  
<span class="inline-comment-marker" ref="3f6d3245-ef4a-4544-8221-84a9f55736c1">StreetNumber: 20</span>  
<span class="inline-comment-marker" ref="3f6d3245-ef4a-4544-8221-84a9f55736c1">ZIPCode: 3000</span>  
<span class="inline-comment-marker" ref="3f6d3245-ef4a-4544-8221-84a9f55736c1">City: Bern</span>

We send this data to <span class="inline-comment-marker" ref="2f07830a-0c15-4c16-83ff-e6ff5f98b7e5">SwissPost to normalize it and get the correct and normalized address</span>:

StreetName: Bahnhofsstrasse  
StreetNumber: 20  
ZIPCode: 3000  
City: Bern

## Matching Lookup

The "matching lookup" has the goal to provide the senders with metadata of our customers.  
The idea behind this is that the sender customers can already do a "pre-matching" and thus do not have to provide us with all customer data.

It is important that we only transmit unique credentials in **hashed form.** The possible credentials are currently:  
- Email addresses  
- Postal addresses (complete)  
- Cell phone number

In the request the sender should have the ability to provide a timestamp.  
With this timestamp he only gets the Delta between the provided timestamp and the newest data (edited or new customer metadata).

## <span class="inline-comment-marker" ref="8f23091c-56eb-482d-9f99-f92875240062">Metadata</span> for Delivery

Please look at this Requestbody of this call to see latest description of DocumentMetadata:

<a href="https://api-dev.klara.tech/docs#/ePost+communication+-+V1/post_epost_v1_deliveries__delivery_id__documents" class="external-link" rel="nofollow">https://api-dev.klara.tech/docs#/ePost+communication+-+V1/post_epost_v1_deliveries__delivery_id__documents</a>

<a href="https://axonivy.atlassian.net/wiki/people/5b8371e4fe42212a79621549?ref=confluence" class="confluence-userlink user-mention" data-account-id="5b8371e4fe42212a79621549" target="_blank" data-base-url="https://axonivy.atlassian.net/wiki">[Arrow] Tran The Phi (Unlicensed)</a> → Hope this helps.

OUTDATED DESCRIPTION:

~~{~~  
~~"minimumLevelOfTrust": "string", ~~<span class="legacy-color-text-red2">~~→ this could be either BRONZE, SILVER or GOLD~~</span>  
~~"deliveryChannelPreferences": \[~~  
~~"digital" ~~<span class="legacy-color-text-red2">~~→ This is a preference. There could be following inputs: "digital", "physical" (when we can't deliver digitally we delivery physically) OR "digital" (when we can't deliver digitally we just send an error message back to the sender) OR "phyiscal" (we just send it physically)~~</span>  
<span class="legacy-color-text-default">~~\],~~</span>  
~~"senderName": "string",~~  
~~"senderEndToEndId": "string", ~~<span class="legacy-color-text-red2">~~→ ~~</span>~~This id provided by the sender. It helps the sender to reference the document during the whole process.~~  
~~"senderCaseId": "string", ~~<span class="legacy-color-text-red2">~~→ ~~</span>~~This id provided by the sender. If multiple Documents are related to one case (e.g. insurance) it helps the receiver to group received documents~~  
~~"documentReferenceDate": "2021-03-03", ~~<span class="legacy-color-text-red2">~~→ ~~</span>~~This date is relevant for a document from a business point of view, it differs from the date at which the document is entered in the "Klara Documents" system.~~  
~~"documentTitle": "string", ~~<span class="legacy-color-text-red2">~~→ This is the title we show on the App to the customers (without file-extension)~~</span>  
~~"documentTypes": \[~~  
~~"invoice" ~~<span class="legacy-color-text-red2">~~→ There could be more than one possible value. At the moment we just accept "message", "invoice" and "contract" or multiple ones at the same time but we should extend it by more document types in future (depends on the document enricher)~~</span>  
~~\],~~  
~~"recipients": \[~~  
~~{~~  
~~"<span class="inline-comment-marker" ref="58aa2927-1a02-488f-b663-ec06238d6a68">senderUserId</span>": "string",~~<span class="legacy-color-text-red2">~~ → This ID is for the sender to recognize his recipient. This ID we don't save it, we just give it back in the response of a match, so the sender knows which recipient has matched.~~</span>  
~~"hashedPrimaryCredential": "email", ~~<span class="legacy-color-text-red2">~~→ This could be either "email", "postaladdress" or "mobilenumber"~~</span>  
~~"hashedPrimaryValue": "string", ~~<span class="legacy-color-text-red2">~~→ This is the hashed value of the defined credential above.~~</span>  
~~"derivedHashedCredentials": \[~~<span class="legacy-color-text-red2">~~→ These are credentials that are hashed but aren't "Primary Credentials". There could be more than one credential.~~</span>  
~~"birthDate"~~  
~~ \]~~  
~~"derivedHashedValue": "string", ~~<span class="legacy-color-text-red2">~~→ This hash value is derived from the other value "hashedPrimaryValue". How exactly we derive this hash-value we have to define while implementing. We shouldn't be able to know what value is behind this hash, that's why this is a derived hash (for example a birthDate isn't unique and we can't just hash the value and send it like this).~~</span>  
~~"unhashedCredentials": {~~  
~~"firstNames": "Peter",~~  
~~"lastName": "Muster",~~  
~~"companyName": "Example Company AG",~~  
~~"street": "string",~~  
~~"streetNumber": "string",~~  
~~"zipCode": "string",~~  
~~"city": "string",~~  
~~"email": "string",~~  
~~"mobileNumber": "string",~~  
~~"birthDate": "string",~~  
~~"participantId": "string",~~  
~~"uid": "string", ~~<span class="legacy-color-text-red2">~~→ UID from a company~~</span>  
~~"clientId.legacyId": "string", ~~<span class="legacy-color-text-red2">~~→ From our old API from Swiss Post we get the unique keys from the recipients. And on luz-profile we should have these legacy-id's to get the "old" recipients from a sender.~~</span>  
~~"minimumLevelOfTrust": "string"~~  
~~}~~  
~~}~~  
~~\]~~  
~~}~~

### <span class="inline-comment-marker" ref="98441943-95af-4172-adee-60e5fb086d5a">Client legacy ID</span>

<span class="legacy-color-text-blue1">From our old API from Swiss Post we get the unique keys from the recipients. And on luz-profile we should have these legacy-id's to get the "old" recipients from a sender.</span>

<span class="legacy-color-text-blue1">The unique keys are sender-specific, this means a recipient can have multiple legacy id's on KLARA. That we can be sure that the Legacy ID is unique in our database we will add the "clientID" of the sender in front of it. The senders need to send this format to make a match.</span>

<span class="legacy-color-text-blue1">These values will be <span class="inline-comment-marker" ref="100f2ca2-efba-4e3c-8091-e2fce847309b">exported in an exact given time by Anis</span>, and after that they won't be added or changed. No new sender will be added to this list.</span>

<span class="legacy-color-text-blue1">Important: The values can only be used for the matching when the user has logged in once with his Swiss Post Account on Mylife (Trigger needed)</span>

<span class="legacy-color-text-blue1"><u>Example</u></span>

<span class="legacy-color-text-blue1">Sender: Canton Bern</span>  
<span class="legacy-color-text-blue1">ClientID: canton-bern</span>

<span class="legacy-color-text-blue1">Old Unique Keys: </span>  
<span class="legacy-color-text-blue1">KLP: 1092049 Unique Key: 283920</span>  
<span class="legacy-color-text-blue1">KLP: 1092050 Unique Key: 283921</span>

<span class="legacy-color-text-blue1"><span class="inline-comment-marker" ref="65af4d33-e9d5-47e8-ad85-21de705f8454">The unique keys will be stored</span> like that: "canton-bern.283920" and "canton-bern.283921" for the matching on KLARA.</span>

## <span class="legacy-color-text-blue1">Hashing</span>

The hashing is made using the **<u>SHA-256</u>** algorithm as the SHA-512 is slower on small input text sizes. 

### Primary credential hashing

#### emailAddress: 

The E-Mail address contains a local-part and a domain divided by an "@" → local-part@domain. Every character is lower case. 

example: anis.sebai@klara.ch

SHA256: hash(anis.sebai@klara.ch) → d02435254757c1904f0560f1f70ac0959b9d4ffa7c802017b084d47bf638c222

#### mobilePhoneNumber: 

A telephone number is formed of 9 digits, plus two digits corresponding to the regional code and preceded by the “+” symbol, totalling 12 symbols (E.164 standard).

example: +41788508018 

SHA256: hash(+41788508018) → d57c9ea280cf690b5ccaeb225d844e0b0936d6e08c2f8bb13e08abfec406fee4

#### postaladdress: 

The postal address contains following fields:

- First Name

- Last Name

- Street Name

- Street Number

- ZIP Code

- City

The additional address information and multiple names are ignored.

The First Name and Last Name are normalized before getting hashed (special characters are ignored and every character is in lower case).

example: André Peter Muster, Weltpoststrasse 3E, 3015 Bern

SHA256: hash(<span class="inline-comment-marker" ref="7d5f81c8-6837-4896-aeb5-9590fd4136d1"><span class="inline-comment-marker" ref="f29ccc8a-0a8b-47a4-90cf-776d161979b9">andremusterweltpoststrasse3e3015bern</span></span>): e3c33c2e3a8e84e907f40eea7867c625ff8428e5d835d7bbe033640c094c0edd

### Derived credential hashing

The derived credential hashing is following the structure as explained below: 

To get a <span class="inline-comment-marker" ref="b07a54e2-7e6b-49ba-9dbd-347fe3066b61">derived credential hash first a primary credential hash is necessary</span> (see above).

Then the derived credential is hashed and the hash is concatenated after the primary credential hash. Both are then hashed together.

Example: 

primary Credential = emailAddress: <span class="legacy-color-text-green5">d02435254757c1904f0560f1f70ac0959b9d4ffa7c802017b084d47bf638c222</span>

derived credential = birthDate: <span class="legacy-color-text-teal3">5dd57dd3045a0f81e2835a2c34e0e422abea36a667f3ecdf6673c1f2933d30fa</span>

<span class="legacy-color-text-default">SHA256: hash(</span><span class="legacy-color-text-green5">d02435254757c1904f0560f1f70ac0959b9d4ffa7c802017b084d47bf638c222</span><span class="legacy-color-text-teal3">5dd57dd3045a0f81e2835a2c34e0e422abea36a667f3ecdf6673c1f2933d30fa</span><span class="legacy-color-text-default">)</span>

<span class="legacy-color-text-default">→ Derived Hash Value: 0e7398b5921dc5cf417e52f2175cba4d419e063259948b94e540f0f804d393ba</span>

------------------------------------------------------------------------

------------------------------------------------------------------------

------------------------------------------------------------------------

# Progress

**Identity Matching**

- <span class="placeholder-inline-tasks">Matching on E-Mail address</span>
- <span class="placeholder-inline-tasks">Matching on Postal address (individual and business tenant)</span>
  - <span class="placeholder-inline-tasks">Address normalizer (incl. fail-safe)</span>
- <span class="placeholder-inline-tasks">Matching on mobile phone number</span>
- <span class="placeholder-inline-tasks">Matching on social security number</span>
- <span class="placeholder-inline-tasks">Matching on UID</span>
- <span class="placeholder-inline-tasks">Matching on legacy-ID</span>
- <span class="placeholder-inline-tasks">Matching on PID</span>
- <span class="placeholder-inline-tasks">Matching on birthdate</span>
- <span class="placeholder-inline-tasks">Hash credentials and store them</span>
- <span class="placeholder-inline-tasks">Matching on E-Mail hashed</span>
- <span class="placeholder-inline-tasks">Matching on Postal address hashed</span>
- <span class="placeholder-inline-tasks">Matching on mobile phone number hashed</span>
- <span class="placeholder-inline-tasks">Matching on derived hash</span>

**Matching Lookup**

- <span class="placeholder-inline-tasks">Get recipients (hashed)</span>
- <span class="placeholder-inline-tasks">Allow providing a timestamp on request for getting delta</span>
- <span class="placeholder-inline-tasks">Implement "next link" for getting all recipients</span>

**Document Delivery**

- <span class="placeholder-inline-tasks">Deliver document on v1  
  </span>
- <span class="placeholder-inline-tasks">Deliver document on v2  
  </span>
- <span class="placeholder-inline-tasks">Store files correctly</span>
- <span class="placeholder-inline-tasks">Store metadata correctly</span>

--------------

</div>

</div>

</div>

<div class="columnLayout single">

<div class="cell normal" data-type="normal">

<div class="innerCell">

<a href="https://axonivy.atlassian.net/wiki/people/557058:077937e9-545a-4d59-abd2-435d99193914?ref=confluence" class="confluence-userlink user-mention" data-account-id="557058:077937e9-545a-4d59-abd2-435d99193914" target="_blank" data-base-url="https://axonivy.atlassian.net/wiki">Daniel Gauch</a> <a href="https://axonivy.atlassian.net/wiki/people/557058:07dd902c-5aad-44a7-a4f0-c9d8b48b4505?ref=confluence" class="confluence-userlink user-mention" data-account-id="557058:07dd902c-5aad-44a7-a4f0-c9d8b48b4505" target="_blank" data-base-url="https://axonivy.atlassian.net/wiki">Negin (Unlicensed)</a> Bitte Review und Ergänzung

# <span class="inline-comment-marker" ref="f135a19d-60e3-4699-b848-e6466cc77022">Adress-Matching</span>

Ausgangslage:

Die heutige Implementierung basiert auf einer Einzelabfrage bei EIRENE, d.h. pro Request zu EIRENE wird eine Adresse abgefragt. Dies dauert gemäss EIRENE ca. 1000ms pro Request.  
Diese Lösung ist für grosse Mengen von Adressen nicht denkbar. Ebenfalls ist der heutige Prozess synchron aufgebaut, was bedeutet, dass die Connection bis zur Response offen bleiben muss.

Aus diesem Grund muss nun möglichst schnell eine Alternative implementiert werden, um den Absendern eine performantere Lösung anbieten zu können. 

Es sind zwei Varianten möglich, die nachfolgend aufgeführt sind: 

</div>

</div>

</div>

<div class="columnLayout two-equal">

<div class="cell normal" data-type="normal">

<div class="innerCell">

## Adress-Matching with EIRENE (Batch)

EIRENE bietet eine Batch-Verarbeitung an, die es erlaubt mehrere Adressen in einem .txt Dokument abzufragen.

Gemäss EIRENE dauert die Abfrage bei 1Mio Adressen ca. 5 Std.

Der Ablauf würde in etwa so funktionieren:

1.  Sender schickt uns mehrere Adressen über einen Call zur Überprüfung

2.  KLARA nimmt die Adressen entgegen und schreibt diese in eine .txt Datei

3.  KLARA schickt die Datei zu EIRENE und erhält eine eindeutige ID des Requests

4.  KLARA überprüft regelmässig den Stand der Bearbeitung durch die Abfrage zu EIRENE mit der ID

5.  KLARA holt die normalisierten Adressen ab und matcht diese mit den eigenen Adressen (mit Normalisierung von Namen)

6.  Der Sender kann die Resultate über eine weitere Abfrage abholen

7.  Nach dem 01.07.: KLARA cacht die abgefragten Adressen mit den erhaltenen Resultaten für eine höhere Performance bei der nächsten Abfrage mit der gleichen Adresse.

Vorteil: EIRENE ist spezialisiert auf die Erkennung und Normalisierung von Adressen und bietet daher eine hohe Qualität von Treffern

Nachteil: Abhängig von der Performance von EIRENE

## Address matching with Eirene Parallel Query

Usecase identity matching before delivery asynchron API.

<div>

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<tbody>
<tr>
<th><p><strong>Case</strong></p></th>
<th><p><strong>Before</strong></p></th>
<th><p><strong>After</strong></p></th>
</tr>
&#10;<tr>
<td><p>Case 1: Leftover recipients<br />
with addresses from matching</p></td>
<td><p>100 leftover addresses<br />
are normalized with single<br />
query API</p>

![[20525652813-image_2022_03_15T12_37_58_363Z.png]]

</td>
<td><p>100 leftover addresses are normalized<br />
with parallel query API</p>

![[20525652813-image_2022_03_15T12_39_55_217Z.png]]

</td>
</tr>
<tr>
<td><p>Case 2: Leftover recipients<br />
with addresses sending batch<br />
after getting from cache</p></td>
<td><p>100 leftover addresses<br />
are normalized with single<br />
query API</p>

![[20525652813-image_2022_03_15T12_42_05_065Z.png]]

</td>
<td><p>100 leftover addresses are normalized<br />
with parallel query API</p>

![[20525652813-image_2022_03_15T12_42_21_323Z.png]]

</td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div class="cell normal" data-type="normal">

<div class="innerCell">

## Adress-Matching mit self-made Algorithmus

Durch einen selbst entwickelten Algorithmus könnte auf EIRENE verzichtet werden, da bei der Registrierung bereits die normalisierten Adressen des Empfängers abgespeichert wurden. Die Normalisierung der Adressen eines Absenders würden ohne EIRENE durchgeführt werden.

Der Ablauf würde in etwa so funktionieren: 

1.  Sender schickt uns mehrere Adressen über einen Call zur Übeprüfung

2.  KLARA normalisiert Adressen nach eigenem Algorithmus

3.  KLARA matcht Adressen mit bereits normalisierten Adressen von EIRENE (mit Normalisierung von Namen)

4.  Der Sender kann die Resultate über eine weitere Abfrage abholen

5.  Nach dem 01.07.: KLARA cacht die abgefragten Adressen mit den normalisierten Resultaten für eine höhere Performance bei der nächsten Abfrage mit der gleichen Adresse.

Vorteil: Keine Abhängigkeit zur Performance von EIRENE

Nachteil: Komplexes Thema mit vielen Spezialfällen (multilingual, ausländische Namen, etc.), stetige Weiterentwicklung notwendig

Möglicher Algorithmus (nicht vollständig):

- Abkürzungen ergänzen (str. → Strasse)

- Kleinschreibung

- Sonderzeichen entfernen

- Akzente entfernen

</div>

</div>

</div>

</div>
