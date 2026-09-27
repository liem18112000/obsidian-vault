---
ai_hash: 3c6593ce1488ab2f
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 3
depth: 3
entities: []
relevance: 0.792
source: https://axonivy.atlassian.net/wiki/spaces/HACKA/pages/48584982860/Roles+and+Permissions+check+for+accessing+public+API
space: HACKA
status: reference
tags:
- confluence
- programming
- space/hacka
title: Roles and Permissions check for accessing public API
topic: programming
type: source
updated: 2025-07-18
---

# Roles and Permissions check for accessing public API

> [!info] Imported from Confluence
> Space **HACKA** · updated 2025-07-18 · [open original](https://axonivy.atlassian.net/wiki/spaces/HACKA/pages/48584982860/Roles+and+Permissions+check+for+accessing+public+API)
> Relevance 0.792 · topic `programming`

## How are permissions checked for a public API call

Let’s take an example with public API group ePost Digital Letterbox.


![[48584982860-image-20250718-075629.png]]



When a user makes a call to endpoint GET /epost/v2/letters, the request comes to luz-public-api-adapter, then luz-public-api-adapter forwards the call to luz-doc-view-controller and forwards the result back to the user.


![[48584982860-API calls.png]]



During the call, luz-public-api-adapter does not check any permission. Instead, it lets the check for the corresponding internal service.

## Privacy Issue?

The following 4 roles can access public API GET /epost/v2/letters because they have permission LUZ_DOCS_VIEW_CONTROLLER.

1.  Online Booking

2.  Accountant

3.  Employees and payroll

4.  Customer relationship manager

5.  Smart Send

But these roles must not have access to this public API.

## Expectation

Access to public API group ePost Digital Letterbox are blocked for the above 4 roles but we have to make sure these roles still can access other necessary internal APIs in luz-docs-view-controller.  

## Possible Solutions

1.  Check role/permission at luz-public-api-adapter before forwarding calls to corresponding internal services.

2.  Split big permission like LUZ_DOCS_VIEW_CONTROLLER into smaller permissions and assign only necessary permissions for roles.

##

%% ai-graph-start %%

**Related notes:**
- [[Research Protect ePost inbox with new Digital_Letterbox permission]]
- [[Role concept for ePost]]
- [[Public API (30 August 2021)]]
- [[One API Module Responsibilities]]
- [[IVY API Calls Overview for luz Modules]]

%% ai-graph-end %%