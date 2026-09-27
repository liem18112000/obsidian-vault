---
title: "Pre-Estimation Klara Public API"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/TS/pages/47006221360/Pre-Estimation+Klara+Public+API
space: "TS"
topic: programming
relevance: 0.738
depth: 2.65
updated: 2021-11-29
attachments: 0
tags:
  - confluence
  - programming
  - space/ts
---

# Pre-Estimation Klara Public API

> [!info] Imported from Confluence
> Space **TS** · updated 2021-11-29 · [open original](https://axonivy.atlassian.net/wiki/spaces/TS/pages/47006221360/Pre-Estimation+Klara+Public+API)
> Relevance 0.738 · topic `programming`

The idea is to work on our own public API while we implement the API for Google. For sure we must follow the Google guidelines in order to fulfill Reserve with Google

Scenario for the integration:

- When Partner doesn’t have a booking system and they want to use Online Booking from Klara

### List of API:

1.  <u>Get Availability</u> (5 points)  
    - Get necessary data in order to create a booking.

    - Store

    - Service

    - Resource

    - Available time slot

2.  <u>Create Booking</u> (5 points)

    - The partner backend makes a booking for the requested time slot

3.  <u>Update Booking</u> (3 points)

    - To **modify** booking: update time slot, add Customer Notes, …

    - And **cancel** an existing booking.

4.  <u>Get Booking List</u> (3 points)

    - End user can see their upcoming booking, …

------------------------------------------------------------------------

### Open points

1.  What if Klara Booking Server is unavailable?

    - Can not make booking on Partner side

    - User can still make booking on Partner side, need to have a job to sync to Klara at the end of the day (8 points)

2.  Should we consider these 2 scenarios?

    - End user create appointment <u>without choosing resource</u> for a service - Klara will handle it.

      - depending of the KLARA user setup, if the resource group can be chosen or not

    - Allow end user to <u>choose resource</u> for a service when they create appointment

      - depending of the KLARA user setup, if the resource group can be chosen or not
