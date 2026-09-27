---
title: "IVY API Calls Overview for luz Modules"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/TS/pages/48714416131/IVY+API+Calls+Overview+for+luz+Modules
space: "TS"
topic: programming
relevance: 0.769
depth: 2.68
updated: 2025-10-02
attachments: 0
tags:
  - confluence
  - programming
  - space/ts
---

# IVY API Calls Overview for luz Modules

> [!info] Imported from Confluence
> Space **TS** · updated 2025-10-02 · [open original](https://axonivy.atlassian.net/wiki/spaces/TS/pages/48714416131/IVY+API+Calls+Overview+for+luz+Modules)
> Relevance 0.769 · topic `programming`

### Module that calls IVY

- **luz_tenant_deletion**

  - API Called:

    - `sessions/terminate"`

- **luz_admin**

  - API Called to IVY

    - NO

- **luz_mobile**

  - API Called to IVY

    - **Many**

- **luz_insurance**

  - API called to IVY

    - /insurances/tasks/recommendations

    - /users/{user-name}/login-language

    - /insurances/tasks/connection

    - /insurances/tasks/{assignee}/connection

- **luz_doc_output_mgmt**

  - API Called to IVY

    - `/create-documents`

- **luz_profile**

  - API called to IVY

    - `{tenant-id}/trigger-authentication-letter`

- **luzfin_finance**

  - upload document: /{tenant-id}/companies/{company-id}/partners/{partner-id}/documents

  - Upload picture reporter /{tenant-id}/companies/{company-id}/reporters/{reporter-id}/picture

  - Upload document for order /{tenant-id}/companies/{company-id}/orders/{order-id}/documents

  - DELETE /{tenant-id}/companies/{company-id}/reporters/{reporter-id}/picture/{picture-id}

  - GET /{tenant-id}/companies/{company-id}/documents/metadata

  - GET /{tenant-id}/companies/{company-id}/documents/{document-id}

  - DELETE /{tenant-id}/companies/{company-id}/partners/{partner-id}/documents/{document-id}

  - DELETE /{tenant-id}/companies/{company-id}/orders/{order-id}/documents/{document-id}

  - GET invoice printing POST /{tenant-id}/companies/{company-id}/print-orders/invoices/

  - POST /{tenant-id}/companies/{company-id}/print-orders/clone-attachments/

  - DELETE {tenant-id}/documents

- **jwt_service**

  - Get Role: `/users/{username}/{uuid}"`

  - Create User: `/users/initial`

### The luz_epost calls ivy-related services:

- **luz_profile**

  - API called

    - PATCH `/v2/:tenantId/profile` ✅

- **jwt_service**

  - `/user-generic-token` ✅

  - `/refreshtokens/tenants/:tenantId` ✅

- **luzfin_finance**

  - `/:tenantId/companies/1/partner-import/analyzing ✅`

  - `/:tenantId/companies/1/partner-import/template-file ✅`

- **luz_admin_service**

  - `/feature-switch/roles?tenant=:tenantId&username=:us&feature=:featureSwitch`✅

✅Means safe, it does not call luz_webclient (IVY)
