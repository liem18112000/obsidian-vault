---
title: "One API - Investigation"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/TS/pages/47125988369/One+API+-+Investigation
space: "TS"
topic: programming
relevance: 0.769
depth: 2.68
updated: 2022-08-02
attachments: 1
tags:
  - confluence
  - programming
  - space/ts
---

# One API - Investigation

> [!info] Imported from Confluence
> Space **TS** · updated 2022-08-02 · [open original](https://axonivy.atlassian.net/wiki/spaces/TS/pages/47125988369/One+API+-+Investigation)
> Relevance 0.769 · topic `programming`

### What is One API? (Link: [OneAPI - Concept](https://axonivy.atlassian.net/wiki/spaces/KPM/pages/47080276496/OneAPI+-+Concept) )

One API takes the responsibility for delivering a letter/simple message to the recipient

The flow of One API:


![[47125988369-IMG_7593.jpeg]]



### The list of delivery channels (Link: <a href="https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47121007316/One+API+-+Delivery+channels+dispatching#Supported-media-types.3" data-card-appearance="inline" rel="nofollow">https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47121007316/One+API+-+Delivery+channels+dispatching#Supported-media-types.3</a>)

- Epost

- eBill

- Print and Send

- IncaMail

- **Whatsapp**

  - Document types: not defined

  - Media types: txt, png, jpeg, mp4, Short text message

- **SMS**

  - Document types: none - since documents are not in short text message format

  - Media types: Short text message

- **Normal Email**

  - Document types: invoice, contract, hr, advertisement, short text message, empty

  - Media types: pdf, txt, png, jpeg, mp4, html

### How to use (Link: <a href="https://api-dev.klara.tech/docs" class="external-link" data-card-appearance="inline" rel="nofollow">https://api-dev.klara.tech/docs</a> → DeliveryMetadataV2). Module: luz-eletter

allCredentialShouldMatch: false

allRecipientsRequired: false

deliveryChannelPreferences: \[DIGITAL, WHATSAPP, SMS\]

displayDeliveredDocument: true

documentReferenceDate: time to send message

documentTitle: subject of notification content

documentMessage: message send to WHATSAPP and SMS

documentTypes: null

fileName: booking.html

mediaType: application/vnd.ch.klara.epost.v1+simple_short_message

recipients:

senderUserId: tenant id

unhashedCredentials: → email and phone number

senderCaseId: booking id

senderEndToEndId: customer id

senderName: company name

### Pull request of Avatar team: <a href="https://bitbucket.org/axonivy-prod/%7Bc066bf60-6984-4fa5-8840-b281f4c172d9%7D/pull-requests/1431" class="external-link" data-card-appearance="inline" rel="nofollow">https://bitbucket.org/axonivy-prod/%7Bc066bf60-6984-4fa5-8840-b281f4c172d9%7D/pull-requests/1431</a>
