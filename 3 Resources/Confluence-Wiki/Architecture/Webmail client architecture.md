---
title: "Webmail client architecture"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/Helios/pages/48001417704/Webmail+client+architecture
space: "Helios"
topic: architecture
relevance: 0.769
depth: 2.79
updated: 2024-08-28
attachments: 10
tags:
  - confluence
  - architecture
  - space/helios
---

# Webmail client architecture

> [!info] Imported from Confluence
> Space **Helios** · updated 2024-08-28 · [open original](https://axonivy.atlassian.net/wiki/spaces/Helios/pages/48001417704/Webmail+client+architecture)
> Relevance 0.769 · topic `architecture`

<div class="toc-macro client-side-toc-macro conf-macro output-block" cssliststyle="none" hasbody="false" headerelements="H1,H2,H3,H4,H5,H6,H7" macro-id="d9cfdc5b-7c22-49b5-adfa-ba5278c6de0b" macro-name="toc">

</div>

# File name convention

Start with prefix sm-

Ex: sm-mailbox.model.tsx

# Structure

1.  \_components: all UI component with `'use client';`

2.  \_configs: temporary for user mapper, may deleted later

3.  \_libs:

    1.  \_constants:

        1.  .const: sm-filename.const.tsx

        2.  .enum: sm-file.enum.tsx

    2.  \_types: view model: sm-mailbox.model.tsx

    3.  \_utils: common function

4.  \_service:

    1.  \_actions: called from client, await data from server ( imap-core ) then return data to client.

    2.  \_dtos: model and data type for \_actions call and return

    3.  \_imap-core: handle imap flow and related

    4.  \_mappers: convert data type from \_dtos to \_lib/types

5.  \_stores: store, reducer, redux, context

6.  1st layout.tsx: will handle/include SessionProvider, DataProvider, mail box list

    1.  \[mailboxId\] layout: page for mail box, handle mail list

        1.  \[mailId\]: page for mail detail

# Data flow

- Client call function from action to request data

- action use mapper to convert data from view model to dto then send request to server

- Server return response with dto type, the mapper will convert dto type to view model type, then response data to client.


![[48001417704-SM- Data Flow.png]]



# Multi user flow


![[48001417704-SM Multi User Flow.png]]
