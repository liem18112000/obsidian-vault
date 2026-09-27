---
title: "Estimate for ivy and cob-unattended-business-dossier-service-api-spec"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/Arrow/pages/48431300739/Estimate+for+ivy+and+cob-unattended-business-dossier-service-api-spec
space: "Arrow"
topic: programming
relevance: 0.738
depth: 2.65
updated: 2025-04-01
attachments: 0
tags:
  - confluence
  - programming
  - space/arrow
---

# Estimate for ivy and cob-unattended-business-dossier-service-api-spec

> [!info] Imported from Confluence
> Space **Arrow** · updated 2025-04-01 · [open original](https://axonivy.atlassian.net/wiki/spaces/Arrow/pages/48431300739/Estimate+for+ivy+and+cob-unattended-business-dossier-service-api-spec)
> Relevance 0.738 · topic `programming`

This document list all the steps and estimate the effort to implement backend APIs in ivy side.

# Total points: 362 pts

# cob-unattended-business-dossier-service-api-spec (41 pts)

- Create module + push into repository (1 pts)

- Create spec for APIs and get approval with Patrick (40 pts)

  - API POST **/business-customer/dossiers** to create new dossier (2 pts)

  - API GET **/business-customer/dossiers/{dossierId}** to get 1 dossier by its id + confirm with Patrick about dossier structure (3 pts)

  - API GET **/business-customer/dossiers?onboardingKey=some-uuid** to get 1 dossier by its onboarding-key (1 pts, since can copy the model response from above)

  - API PATCH **/business-customer/dossiers/{dossierId}/selections** to update legal notice + product + product offers into ivy database + change state to IDENTIFICATION (2 pts)

  - API PATCH **/business-customer/dossiers/{dossierId}/identifications** to update company’s data, account holder persons into ivy database + change state to COLLECTION (2pts)

  - API POST **/business-customer/dossiers/{dossierId}/identification-persons** to call ivy and trigger fidentity url link + send sms to that person (2pts)

  - API POST **/business-customer/dossiers/{dossierId}/authorized-persons** to call ivy to add authorized persons (2pts)

  - API PATCH **/business-customer/dossiers/{dossierId}/collections** to update company’s data in GUI into ivy side and change state to COMPLIANCE_VERIFICATION (2pts)

  - API GET **/business-customer/dossiers/{dossierId}/compliance-verification/identification-status** to get status of person do identification (2pts)

  - API GET **/business-customer/dossiers/{dossierId}/compliance-verification/compliance-check-status** to get status of compliance task (2pts)

  - API PATCH **/business-customer/dossiers/{dossierId}/signs** to update into ivy side to change state to SIGNING (2pts)

  - API GET **/business-customer/dossiers/{dossierId}/signs/signing-status** to get status of signing (2pts)

  - API PATCH **/business-customer/dossiers/{dossierId}/completes** to update into ivy side to change state to COMPLETED (2pts)

  - API GET **/reference/search-company?name=** expose money-house search company (2 pts)

  - API GET **/reference/search-company/{companyId}** expose money-house search company by id (2 pts)

  - API GET **/reference/search-company/{companyId}/persons** expose money-house search company’s perons by id (2 pts)

  - API PATCH **/business-customer/dossiers/{dossierId}/registrations** to update company’s phone in GUI into ivy side and change state to REGISTRATION (2pts)

  - API POST **/business-customer/dossiers/{dossierId}/registrations** to verify the otp code (2pts)

  - API POST **/business-customer/dossiers/re-entries** to send re-entry otp code (2 pts)

  - API POST **/business-customer/dossiers/re-entries/{reEntryId}/verifications** to verify re-entry otp code (2 pts)

# ivy - implement the spec only (313 pts)

- Adapt dossier model to include company structure (3 pts)

- Implement API POST **/business-customer/dossiers** to create new dossier (20 pts)

  - Check/investigate current logic for flow unattended, know which table will be related for this flow. (5 pts)

  - Implement the generate cobId for this flow (1 pts)

  - Implement the service to init the dossier and save the dossier (8 pts)

  - Implement the Rest API (2 pts)

  - Unit testing, cross test, demo (4pts)

- Implement API GET **/business-customer/dossiers/{dossierId}** to get 1 dossier by its id (8 pts)

- Implement API GET **/business-customer/dossiers?onboardingKey=some-uuid** to get 1 dossier by its onboarding-key (8 pts) \*

- Implement API PATCH **/business-customer/dossiers/{dossierId}/selections** to update legal notice + product + product offers into ivy database + change state to IDENTIFICATION (8 pts)

- Implement API PATCH **/business-customer/dossiers/{dossierId}/identifications** to update company’s data, account holder persons into ivy database + change state to COLLECTION (8 pts)

- Implement API POST **/business-customer/dossiers/{dossierId}/identification-persons** to call ivy and trigger fidentity url link + send sms to that person (8 pts)

- Implement API POST **/business-customer/dossiers/{dossierId}/authorized-persons** to call ivy to add authorized persons (8 pts)

- Implement API PATCH **/business-customer/dossiers/{dossierId}/collections** to update company’s data in GUI into ivy side and change state to COMPLIANCE_VERIFICATION (8 pts)

- Implement API GET **/business-customer/dossiers/{dossierId}/compliance-verification/identification-status** to get status of person do identification (8 pts)

- Implement API GET **/business-customer/dossiers/{dossierId}/compliance-verification/compliance-check-status** to get status of compliance task (8 pts)

- Implement PATCH **/business-customer/dossiers/{dossierId}/compliance-verification** to process next (42 pts)

  - If all persons have done identification, we trigger compliance task and show compliance task (33pts)

  - If compliance task is approved, we change state to SIGNING (1pts)

  - If rejected, we process to NOK case and open in Archive role (8 pts)

- Implement API PATCH **/business-customer/dossiers/{dossierId}/signs** to start signing process (Create documents and put to Fidentity, send signing links to account holders) (13 pts)

- Implement API POST **/business-customer/dossiers/{dossierId}/signs/callback** (13 pts)

- Implement API GET **/business-customer/dossiers/{dossierId}/signs/signing-status** to get status of signing (8 pts)

- Implement API PATCH **/business-customer/dossiers/{dossierId}/completes** to update into ivy side to change state to COMPLETED (92 pts)

- Implement API GET **/reference/search-company?name=** expose money-house search company (6 pts)

- Implement API GET **/reference/search-company/{companyId}** expose money-house search company by id (6 pts)

- Implement API GET **/reference/search-company/{companyId}/persons** expose money-house search company’s perons by id (6 pts)

- Implement API PATCH **/business-customer/dossiers/{dossierId}/registrations** to update company’s phone in GUI into ivy side and change state to REGISTRATION (8 pts)

- Implement API POST **/business-customer/dossiers/{dossierId}/registrations** to verify the otp code (8 pts)

- Implement API POST **/business-customer/dossiers/re-entries** to send re-entry otp code (8 pts)

- Implement API POST **/business-customer/dossiers/re-entries/{reEntryId}/verifications** to verify re-entry otp code (8 pts)

# ivy - Update cob-fidentity-adapter-service (8 pts)

Register new callbacks, redirect links

# ivy - compliance task and NOK case (added point to the APIs)

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
<th><p><strong>No.</strong></p></th>
<th><p><strong>Part</strong></p></th>
<th><p><strong>Tasks</strong></p></th>
<th><p><strong>Ivy</strong></p></th>
<th><p><strong>Without Ivy</strong></p></th>
</tr>
&#10;<tr>
<td><p>1</p></td>
<td><p><del>Update cob-fidentity-adapter-service</del></p></td>
<td><p><del>Register new callbacks, redirect links</del></p></td>
<td colspan="2"><p>8pts.</p></td>
</tr>
<tr>
<td><p>2</p></td>
<td><p>Extend API PATCH <strong>/business-customer/dossiers/{dossierId}/identifications</strong></p></td>
<td><p><strong>Create new identification processes</strong></p>
<p><strong>Example:</strong> Register two identified persons.</p>
<ul>
<li><p><strong>Ivy:</strong> Build two request calls to the Fidentity adapter service.</p></li>
<li><p><strong>Create a table</strong> to store <code>extId</code> when registering in Fidentity:</p>
<ul>
<li><p>dossierId | extId | processId | requestId</p></li>
</ul></li>
<li><p><strong>Receive</strong> <code>processId</code>, then build the link and send it to each registered user.</p></li>
</ul></td>
<td><p>8pts</p></td>
<td><p>8pts</p></td>
</tr>
<tr>
<td><p>3</p></td>
<td><p>Compliance task</p></td>
<td><p>Part 1:</p>
<ul>
<li><p>Download necessary documents from Fidentity</p></li>
<li><p>Map Fidentity data to business dossier</p></li>
<li><p>Create compliance task with the business dossier</p></li>
<li><p>Call method to generate protocol</p></li>
</ul>
<p>Part 2:</p>
<ul>
<li><p>Open the business dossier compliance task</p></li>
<li><p>Add a new tab in the business compliance task view to display the business dossier data</p></li>
<li><p>Handle approving/rejecting with new business dossier data</p></li>
</ul></td>
<td><p>Part 1: 13pts</p>
<p>Part 2: 20pts</p>
<p>Total: 33pts</p></td>
<td><p>Is depended on cob-ui</p>
<p>Part 1: 13pts</p>
<p>Part 2: 20pts</p>
<p>Total: 33pts</p></td>
</tr>
<tr>
<td><p>4</p></td>
<td><p>Handle NOK Fraud case</p></td>
<td><ul>
<li><p>Process to archive role</p></li>
<li><p>Handle open the archive task by archive user</p></li>
</ul></td>
<td><p>8pts.</p></td>
<td><p>Is depended on cob-ui</p>
<ul>
<li><p>send email</p></li>
<li><p>change dossier status</p></li>
<li><p>call method to generate protocol</p></li>
</ul>
<p>20pts.</p></td>
</tr>
</tbody>
</table>

</div>

# Ivy - Detail tasks for starting signings and completing dossier (added point to the APIs)

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
<th><p><strong>No.</strong></p></th>
<th><p><strong>Task</strong></p></th>
<th><p><strong>Ivy</strong></p></th>
<th><p><strong>Without Ivy</strong></p></th>
</tr>
&#10;<tr>
<td><p>1</p></td>
<td><p>Implement start signing API (POST /business-customer/dossiers/{dossierId}/signs)</p>
<ul>
<li><p>validateDossierProgress</p></li>
<li><p>Generate a credit card document (from EGW): can be new interface</p></li>
<li><p>Upload documents to Fidentity to sign (for each person)</p></li>
<li><p>Send SMS links to each person (call cob-sms-adapter-service)</p></li>
</ul></td>
<td><p>13pts.</p></td>
<td><p>Is depended on cob-bank-pf-adapter-service.</p>
<p>Addition points:</p>
<ul>
<li><p>Build create credit card request to cob-bank-service</p></li>
</ul>
<p>20pts.</p></td>
</tr>
<tr>
<td><p>2</p></td>
<td><p>Implement signing completed callback API (POST /business-customer/dossiers/{dossierId}/signs/callback)</p>
<ul>
<li><p>Register our callback signing with Fidentity</p></li>
<li><p>Receive callbacks that all person has signed and send an SMS to the account opener.</p></li>
</ul></td>
<td><p>13pts</p></td>
<td><p>13pts</p></td>
</tr>
<tr>
<td><p>3</p></td>
<td><p>Implement signing completes API (POST /business-customer/dossiers/{dossierId}/completes)</p>
<ul>
<li><p>validateDossierProgress</p></li>
<li><p>validateIdentityProcess</p></li>
<li><p>Trigger complete signal and return status code STARTED</p></li>
<li><p>Perform checks (one part)</p></li>
<li><p>Reserve account number</p></li>
<li><p>Download Fidentity document for each person (ONLINE_ID, SIGNATURE, RESIDENCE_PERMIT, CREDIT_CARD)</p></li>
<li><p>Generate document from EGW (one part)</p></li>
<li><p>Generate PROTOCOL (one part)</p></li>
<li><p>Handle case generate document failed.</p></li>
<li><p>Send handover (one part)</p></li>
<li><p>Send TAR file - FDSs (one part)</p></li>
</ul></td>
<td><p>20pts</p></td>
<td><p>Is depended on cob-bank-pf-adapter-service.</p>
<p>Addition points:</p>
<ul>
<li><p>Handle trigger complete signal and return status code STARTED immediately</p></li>
<li><p>Build reserve account number request to cob-bank-service</p></li>
<li><p>Handle download Fidentity documents and store to cob-dossier-content-service</p></li>
<li><p>Handle case generate document failed</p>
<ul>
<li><p>Generate TAR file → cob-bank-pf-adapter-service</p></li>
<li><p>Send email → cob-email-adapter-service</p></li>
</ul></li>
</ul>
<p>20pts.</p></td>
</tr>
<tr>
<td><p>4</p></td>
<td><p>Perform checks</p>
<ul>
<li><p>(Optional) Timebox 90s for the forwarding flow, if longer than this time we show another view inform that we will inform by SMS</p></li>
<li><p>Perform credit check</p></li>
<li><p>Perform PEP check</p></li>
<li><p>Perform Address check</p></li>
<li><p>Handle flow NOK_KYC for each person</p></li>
</ul></td>
<td><p>13pts</p></td>
<td><p>Is depended on cob-credit-rating-adapter-service, cob-kyc-verification-service and cob-address-verification-adapter-service</p>
<p>Additional points:</p>
<ul>
<li><p>Call cob-credit-rating-adapter-service</p></li>
<li><p>Call cob-kyc-verification-service</p></li>
<li><p>Call cob-address-verification-adapter-service</p></li>
<li><p>Handle flow NOK_KYC:</p>
<ul>
<li><p>Send email via cob-email-adapter-service</p></li>
<li><p>Update dossier to archive role</p></li>
</ul></li>
</ul>
<p>20pts.</p></td>
</tr>
<tr>
<td><p>5</p></td>
<td><p>Generate document from EGW</p>
<ul>
<li><p>Build request (expect new data in the request)</p></li>
<li><p>Generate BASIC_CONTRACT, TAX_RESIDENCY_SELF_CERTIFICATION</p></li>
<li><p>Generate opener document</p></li>
</ul></td>
<td><p>13pts</p></td>
<td><p>Is depended on cob-bank-pf-adapter-service.</p>
<p>Additional points:</p>
<ul>
<li><p>Build create document request to cob-bank-service</p>
<ul>
<li><p>BASIC_CONTRACT</p></li>
<li><p>TAX_RESIDENCY_SELF_CERTIFICATION</p></li>
</ul></li>
</ul>
<p>13pts</p></td>
</tr>
<tr>
<td><p>6</p></td>
<td><p>Generate protocol</p>
<ul>
<li><p>Update XML resolver file</p>
<ul>
<li><p>Add new section Business</p></li>
<li><p>Add new table Compliance task (reference to COB Desk)</p></li>
<li><p>Add new section to compliance check</p></li>
</ul></li>
<li><p>Check resolve multiple account holders</p></li>
<li><p>Check resolve opener</p></li>
</ul></td>
<td><p>20pts</p></td>
<td><p>Is depended on cob-dossier-protocol-service, cob-dossier-content-service.</p>
<p>Additional points:</p>
<ul>
<li><p>Modify cob-unattended-business-dossier-service to send protocol event with data</p></li>
<li><p>Implement protocol service for Business dossier section</p></li>
<li><p>Language: DE (one language)</p></li>
</ul>
<p>20pts</p></td>
</tr>
<tr>
<td><p>7</p></td>
<td><p>Send handover</p>
<ul>
<li><p>Import new WSDL</p></li>
<li><p>Convert data to handover request</p></li>
<li><p>Send handover</p></li>
</ul></td>
<td><p>13pts</p></td>
<td><p>Is depended on cob-bank-pf-adapter-service.</p>
<p>Additional points:</p>
<ul>
<li><p>Update adapter according new wsdl.</p></li>
<li><p>Build handover request to cob-bank-service.</p></li>
</ul>
<p>13 pts</p></td>
</tr>
<tr>
<td><p>8</p></td>
<td><p>FDS</p>
<ul>
<li><p>Include more documents in TAR file (Customer1TarFileGeneratorService.generateTarFile()).</p></li>
<li><p>Process receipt file (as current).</p></li>
</ul></td>
<td><p>13pts</p></td>
<td><p>Is depended on cob-bank-pf-adapter-service.</p>
<p>Build TAR file and send to cob-bank-pf-adapter-service.</p>
<p>13pts</p></td>
</tr>
</tbody>
</table>

</div>
