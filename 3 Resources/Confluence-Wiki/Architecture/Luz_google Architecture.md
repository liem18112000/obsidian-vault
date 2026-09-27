---
ai_hash: 82e0e98a8da818e5
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 10
depth: 2.72
entities: []
relevance: 0.777
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20508042970/Luz_google+Architecture
space: LUZ
status: reference
tags:
- confluence
- architecture
- space/luz
title: Luz_google Architecture
topic: architecture
type: source
updated: 2020-07-30
---

# Luz_google Architecture

> [!info] Imported from Confluence
> Space **LUZ** · updated 2020-07-30 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20508042970/Luz_google+Architecture)
> Relevance 0.777 · topic `architecture`

On this document the technical data flows and activity diagrams of the luz_google module are documented.

### Google OAuth

The Google OAuth process requires the user to login with his google account and permit the respective Google App to get access to his Google My Business (GMB) profile. This process is documented in the diagram below.

#### <span class="legacy-color-text-default">Sequence Diagram for google Authentication  from Klara UI</span>


![[20508042970-gmb_auth_sequence.png]]



The Klara user login to his google account (or creates a new). This process is conducted by Google OAuth. As a response, Klara will get the refresh and access tokens. The refresh token is stored in DB on luz_google. After this process is completed, the Klara system can send the location details to the GMB API.

### Location API

#### <span class="legacy-color-text-default">Sequence Diagram of Location Api</span>


![[20508042970-location_api_sequence_diagram.png]]



The location API on luz_google has following actions:

- return all locations: getLocations
- return all categories: getCategories
- create new location
- update existing location

On create and update location, the API will also trigger the actions in the following sequence on GMB API:

- search for location address, if it already exists and return error message if so
- create / update location data (upload media)
- add agency manager as owner/manager if location is created

#### <span class="legacy-color-text-default">Sequence Diagram of Location Auto Verification API</span>


![[20508042970-Auto-verify_location_seq_diagram.png]]



  

The location auto verification API on luz_google has following actions:

- auto verify gmb location once its created 
- auto verify gmb location once its updated

Note: for auto verification process luz_google required a refresh token of agency account of Klara which is stored in wild-fly server if the token is valid then location is verified.

#### <span class="legacy-color-text-default">Sequence Diagram of Localpost API</span>


![[20508042970-localpost_api_seq_diagram.png]]



  

The localpost API on luz_google has following actions:

- create new local post (News/Event/Regio deal)
- update existing local post(News/Event/Regio deal)
- delete the existing local post

note: create new local post is only allowed once a location is verified.

#### Activity Diagram of Luz_google


![[20508042970-activity_diagram_luz_google.png]]

%% ai-graph-start %%

**Related notes:**
- [[Luz_google Api Document]]
- [[Luz-audit]]
- [[Investigate Analyze the API's which call to FileManager]]
- [[Implement Corporate API Access (LUZ-17959)]]
- [[Architecture]]

%% ai-graph-end %%