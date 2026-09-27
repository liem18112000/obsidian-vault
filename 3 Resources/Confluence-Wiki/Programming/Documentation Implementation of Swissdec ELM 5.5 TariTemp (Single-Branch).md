---
title: "Documentation: Implementation of Swissdec ELM 5.5 TariTemp (Single-Branch)"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/49389076511/Documentation+Implementation+of+Swissdec+ELM+5.5+TariTemp+Single-Branch
space: "LUZ"
topic: programming
relevance: 0.818
depth: 3
updated: 2026-05-05
attachments: 0
tags:
  - confluence
  - programming
  - space/luz
---

# Documentation: Implementation of Swissdec ELM 5.5 TariTemp (Single-Branch)

> [!info] Imported from Confluence
> Space **LUZ** · updated 2026-05-05 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/49389076511/Documentation+Implementation+of+Swissdec+ELM+5.5+TariTemp+Single-Branch)
> Relevance 0.818 · topic `programming`

<div hasbody="true" macro-id="cf1ae4e6-caf2-48c8-8eda-930650214fd2" macro-name="info">

<span class="aui-icon aui-icon-small aui-iconfont-info confluence-information-macro-icon"> </span>

<div>

Analyzed and summarized by AI

</div>

</div>

## 1. Core Concept and Data Source

The implementation of ELM 5.5 for staffing agencies (Suva risk class 70C) is based on automated web service communication. Deduction percentages are not maintained manually but are retrieved electronically.

- **Primary Interface:** Data retrieval is performed via the web service `GetCompanyProfile`.

- **Data Source:** Suva provides the company-specific insurance profile via this service, including the TariTemp catalogs.

- **Update Cycle:**

  - **Company Level:** General profile update annually (as of Jan 1st) or upon contractual changes.

  - **Employee Level:** Dynamic application of rates for every assignment or job change.

## 2. Organizational Structure: CompanyBranch

The identification of insurance units is carried out via the **CompanyBranch ID** assigned by the insurer (e.g., Suva).

- **Assignment:** The ID (sub-number) is defined by the insurer and delivered within the XML profile.

- **Structure:** A company (tenant) can have multiple `CompanyBranch` IDs (e.g., separation by branch offices or between internal headquarters and external staff leasing).

- **Validation:** The ERP must ensure that payroll calculations are only processed against Branch IDs existing in the retrieved profile.

## 3. Differentiation of Calculation Logic

Within a company, a distinction is made between two paths of contribution determination:

### A. Internal Employees (Headquarters/Administration)

- **Logic:** Fixed percentages per operating department.

- **Assignment:** Based exclusively on the `CompanyBranch ID`.

- **Rates:** BU (Occupational Accident) and NBU (Non-Occupational Accident) rates are stored directly in the XML node of the operating department.

### B. Temporary Employees (Staff Leasing/TariTemp)

- **Logic:** Variable percentages based on the specific activity.

- **Assignment:** Based on the combination of `CompanyBranch ID` and an **Occupation Code**.

- **Rates:** Retrieved from the `<TariTemp>` catalog provided in the XML.

## 4. TariTemp Specifics (Single-Branch)

In the Single-Branch certification, the company is insured for a specific field of activity.

- **Occupation Code:** A 4-digit code (based on ISCO/Suva classes) identifying the occupational group.

- **Catalog Validation:** The ERP must only allow the selection of Occupation Codes that are included in the currently valid XML profile provided by Suva for that specific branch.

- **Data Fields in ERP:**

  - `CompanyBranch ID` (Mandatory for all)

  - `Occupation Code` (Mandatory for Temporary/TariTemp)

  - `Validity Date` (Effective-from date for historization)

## 5. Payroll and Historization Requirements

For ELM 5.5 certification, the following calculatory requirements must be met:

- **Split Logic:** If an employee changes their activity (and thus the Occupation Code) during the month, the system must be able to perform a pro-rata split of the UVG (Accident Insurance) contributions.

- **Historization:** Previously processed periods must not be altered during a profile update. New rates only apply from their defined validity date (`ValidFrom`).

- **Rounding Rules:** The calculation of deductions must comply with the exact mathematical specifications of Suva (usually rounding to two decimal places for the deduction amount).

## 6. XML Structure (Conceptual)

The technical processing in the ERP must be able to parse the following node structures from the `InsuranceProfile`:

- **Fixed Rates (Internal):** `InsuranceProfile -> CompanyBranch -> PremiumRates`

- **Variable Rates (TariTemp):** `InsuranceProfile -> CompanyBranch -> TariTemp -> Occupation (Code/Rates)`

## 7. Certification-Relevant Functions

To successfully complete the ELM 5.5 certification, the ERP system must demonstrably:

1.  Retrieve insurance profiles for all insurance types (UVG, KTG, UVGZ, etc.) via API.

2.  Issue error messages if profiles are missing or if invalid Occupation Codes are used.

3.  Ensure the correct allocation of wage totals to the respective risk classes (Occupation Codes) in the year-end declaration.
