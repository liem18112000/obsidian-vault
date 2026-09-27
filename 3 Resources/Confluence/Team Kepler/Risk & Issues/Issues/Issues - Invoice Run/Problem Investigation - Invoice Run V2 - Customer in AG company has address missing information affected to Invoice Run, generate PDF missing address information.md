---
ai_hash: 26a7469fc243d569
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
confluence_id: '48934879242'
confluence_path: 'Team Kepler > Risk & Issues > Issues > Issues: Invoice Run'
created: 2025-12-04
entities: []
source: Confluence · TK - Team Kepler
status: reference
tags:
- confluence
- invoice-run
title: 'Problem Investigation: [Invoice Run V2] - Customer in AG company has address
  missing information affected to Invoice Run, generate PDF missing address information'
type: source
updated: 2025-12-04
url: https://axonivy.atlassian.net/wiki/spaces/TK/pages/48934879242/Problem+Investigation+Invoice+Run+V2+-+Customer+in+AG+company+has+address+missing+information+affected+to+Invoice+Run+generate+PDF+missing+address+information
---

# Problem Investigation: [Invoice Run V2] - Customer in AG company has address missing information affected to Invoice Run, generate PDF missing address information

*Confluence source · Team Kepler › Risk & Issues › Issues › Issues: Invoice Run · [view original](https://axonivy.atlassian.net/wiki/spaces/TK/pages/48934879242/Problem+Investigation+Invoice+Run+V2+-+Customer+in+AG+company+has+address+missing+information+affected+to+Invoice+Run+generate+PDF+missing+address+information) · updated 2025-12-04*

------------------------------------------------------------------------

## Source Code Analysis

### Problem Description

Customer in AG company has address partially missing in Invoice Run PDF generation. The source data (pic1) contains full address information, but the generated invoice PDF (pic2) only displays part of the address.

------------------------------------------------------------------------

### Investigation Summary

#### Address Data Flow in PDF Generation

There are **two separate address representations** in the PDF:

1.  `customer` **field** (formatted string) - Built by `CustomerBuilder.getCustomerAsStringForPrintingInDocument()`:

    - Uses `address.type` to determine name source

    - Format: `Salutation\nName\nAddressLine\nAdditionalAddress\nPostcode City\nCountry`

2.  **Individual fields** - Set by `setDataToNameAndAddress()`:

    - `customerName` - from Person/Company object

    - `address` - from `address.getAddressLines()`

    - `additionalAddress` - from `address.getAdditionalAddress()`

    - `postCodeAndCity` - from `city.addressPostcode + city.cityName27`

------------------------------------------------------------------------

### Key Code Flow

```
buildOrderDataForPrinting() [InvoiceRunServiceController.java:969]
    │
    ├── CustomerBuilder.getCustomerAsStringForPrintingInDocument(partner) [line 987]
    │       │
    │       ├── getAddressNameForCompany() [CustomerBuilder.java:92-110]
    │       │       └── Uses address.type to get name from Address object
    │       │
    │       └── buildEnvelopInfo() [CustomerBuilder.java:128-147]
    │               └── Uses CustomerUtils.addressInfo() for address fields
    │
    └── setDataToNameAndAddress(orderPrinting, partner) [line 989]
            │
            └── setAddressIntoOrderPrinting() [line 1194-1198]
                    └── Gets address fields directly from Address object
```

------------------------------------------------------------------------

### Potential Root Causes for Partial Address Missing

|  |  |  |
|----|----|----|
| Scenario | Code Location | Effect |
| `address.type` **is set but name fields empty** | `CustomerBuilder.java:92-110` | Falls back to company name which might be different/empty |
| `address.companyName` **is null when** `type=COMPANY` | `CustomerBuilder.java:98-99` | Uses fallback `company.getName()` |
| `additionalAddress` **field is null** | `InvoiceRunServiceController.java:1196` | Second address line empty in PDF |
| `city` **object is null or incomplete** | `InvoiceRunServiceController.java:1197` | NullPointerException or empty postcode/city |
| **Country missing for non-CH addresses** | `CustomerBuilder.java:141-144` | Country line not displayed |
| **Truncation of long address/name** | `CustomerBuilder.java:137-138` | Address truncated with "..." |

------------------------------------------------------------------------

### Address Name Resolution Logic

> [!note]- For Company Customer (getAddressNameForCompany - CustomerBuilder.java:92-110)
>
>
>
> ```
> private static String getAddressNameForCompany(Company company) {
>     Address address = company.getAddresses().get(0);
>     String result = company.getName();  // Default fallback
>
>     if (Objects.isNull(address.getType()))
>         return result;  // Case 1: No type set -> use company.getName()
>
>     if (address.getType() == CustomerType.COMPANY)
>         return Objects.isNull(address.getCompanyName()) ? result : address.getCompanyName();
>         // Case 2: Type=COMPANY -> use address.companyName or fallback
>
>     // Case 3: Type=PERSON -> use person name from address
>     if (Objects.isNull(address.getSalutation())
>             || StringUtils.isEmpty(address.getFirstName())
>             || StringUtils.isEmpty(address.getLastName()))
>         return result;  // Missing person fields -> fallback to company name
>
>     String name = StringUtil.joinString(StringUtils.SPACE, address.getFirstName(), address.getLastName());
>     return NullResolver.resolve(() -> address.getSalutation().getCode()).isPresent()
>             ? StringUtil.joinString(BREAK_LINE, buildSalutation(...), name)
>             : name;
> }
> ```
>
>
>

#### Key Observation

The name displayed on the invoice can come from **different sources**:

- `company.getName()` - The Company object's name

- `address.companyName` - The Address object's company name field

- `address.firstName + address.lastName` - Person name fields on Address

If these are **not synchronized** or **partially populated**, the invoice may show unexpected/partial data.

------------------------------------------------------------------------

### Address Field Structure

#### Address Model (`fin/model/Address.java`)

|                     |                         |                        |
|---------------------|-------------------------|------------------------|
| Field               | Description             | Used In                |
| `addressLines`      | Street address          | PDF address line       |
| `additionalAddress` | c/o, apartment, etc.    | PDF additional line    |
| `city`              | City object             | Postcode + City        |
| `type`              | PERSON or COMPANY       | Determines name source |
| `companyName`       | Company name on address | Name when type=COMPANY |
| `firstName`         | Person first name       | Name when type=PERSON  |
| `lastName`          | Person last name        | Name when type=PERSON  |
| `salutation`        | Mr/Mrs/etc.             | Salutation line        |

#### City Model (`person/model/City.java`)

|                   |                     |
|-------------------|---------------------|
| Field             | Description         |
| `addressPostcode` | Postal/ZIP code     |
| `cityName27`      | City name (27 char) |
| `country`         | Country object      |

------------------------------------------------------------------------

### Related Code Files

|  |  |
|----|----|
| File | Purpose |
| `InvoiceRunServiceController.java` | Main orchestrator for invoice PDF creation |
| `CustomerBuilder.java` | Builds formatted customer address for printing |
| `CustomerUtils.java` | Utility for extracting address info from Customer |
| `CompanyLogoBuilder.java` | Formats AG company address for invoice header |
| `AddressUtil.java` | Validates and retrieves latest address from list |
| `DocumentAddress.java` | Handles alternate recipient address |
| `OrderPrinting.java` | Model containing all PDF fields |

------------------------------------------------------------------------

### Expected Address Format (from test cases)

```
Mr                          <- Salutation (for PERSON)
David Silva                 <- Name (from address.type logic)
Le Thanh Ton 9C            <- addressLines
Additional Info            <- additionalAddress (optional)
1000 Lausanne              <- postcode + cityName27
VIETNAM                    <- country (only for non-CH)
```

------------------------------------------------------------------------

### Debugging Checklist

To identify the specific missing field:

1.  Check if `address.type` is set on the customer's Address

2.  If `type == COMPANY`, verify `address.companyName` is populated

3.  If `type == PERSON`, verify `address.firstName`, `address.lastName`, `address.salutation`

4.  Check if `address.additionalAddress` is populated

5.  Verify `address.city` object exists with `addressPostcode` and `cityName27`

6.  For non-CH addresses, check `city.country.countryName` and `city.country.iso2Code`

7.  Compare `company.getName()` vs `address.companyName` - are they different?

------------------------------------------------------------------------

### Suggested Fix Approaches

1.  **Add logging** in `buildOrderDataForPrinting()` to log all address fields before PDF generation

2.  **Add validation** to ensure address fields are consistent between Company/Person and Address objects

3.  **Investigate data source** - check if the issue is in how data is fetched/populated from the backend

4.  **Sync address fields** - ensure `address.companyName` matches `company.getName()` when saving

------------------------------------------------------------------------

### Test Cases Reference

See `CustomerBuilderTest.java` for expected address formatting:

- `getCustomerAsStringForPrintingInDocument_shouldAssertCorrectData()` - line 108

- `customerForPrintDocument_givenCase_longAdditionAddress_then_display2LineForInvoiceRunOnly()` - line 116

------------------------------------------------------------------------

### Data Source Tracing

#### Complete Data Flow

> [!note]- DATA SOURCE FLOW
>
>
>
> ```
> ┌─────────────────────────────────────────────────────────────────────────────┐
> │                           DATA SOURCE FLOW                                   │
> └─────────────────────────────────────────────────────────────────────────────┘
>
> 1. CUSTOMER DATA FETCH
>    ┌────────────────────────────────────────────────────────────────────────┐
>    │ InvoiceRunServiceController.getInvoiceItemData() [line 903-914]        │
>    │     │                                                                   │
>    │     └── klaraAGCachingService.getCustomerCache(token, partnerUri)      │
>    │              │                                                          │
>    │              └── Returns Customer from cache or fetches from API        │
>    └────────────────────────────────────────────────────────────────────────┘
>                               │
>                               ▼
> 2. CACHING SERVICE
>    ┌────────────────────────────────────────────────────────────────────────┐
>    │ KlaraAGCachingService.getCustomerCache() [line 152-160]                │
>    │     │                                                                   │
>    │     ├── If in cache: return mapCustomer.get(customerId)                │
>    │     │                                                                   │
>    │     └── If not in cache:                                               │
>    │         luzfinFinanceRestClientService.getCustomerById(...)            │
>    └────────────────────────────────────────────────────────────────────────┘
>                               │
>                               ▼
> 3. REST CLIENT SERVICE
>    ┌────────────────────────────────────────────────────────────────────────┐
>    │ LuzfinFinanceRestClientService.getCustomerById() [line 54-56]          │
>    │     │                                                                   │
>    │     └── luzfinFinanceRestClient.getCustomerById(...)                   │
>    └────────────────────────────────────────────────────────────────────────┘
>                               │
>                               ▼
> 4. REST CLIENT (API CALL)
>    ┌────────────────────────────────────────────────────────────────────────┐
>    │ LuzfinFinanceRestClient.getCustomerById() [line 54-59]                 │
>    │                                                                         │
>    │ @GET                                                                    │
>    │ @Path("{tenant-id}/companies/{company-id}/customers/{id}")             │
>    │ Customer getCustomerById(...)                                          │
>    │                                                                         │
>    │ API: GET /luzfin-finance/{tenant-id}/companies/{company-id}/customers/{id}
>    └────────────────────────────────────────────────────────────────────────┘
>                               │
>                               ▼
> 5. JSON RESPONSE → Customer Model
>    ┌────────────────────────────────────────────────────────────────────────┐
>    │ JSON Response deserialized to:                                         │
>    │                                                                         │
>    │ Customer (fin/model/Customer.java)                                     │
>    │     ├── customerType: PERSON | COMPANY                                 │
>    │     ├── person: Person                                                 │
>    │     │     └── addressList: List<Address>                               │
>    │     └── company: Company                                               │
>    │           └── addresses: List<Address>                                 │
>    │                                                                         │
>    │ Address (fin/model/Address.java)                                       │
>    │     ├── addressLines: String                                           │
>    │     ├── additionalAddress: String                                      │
>    │     ├── city: City                                                     │
>    │     │     ├── addressPostcode: String                                  │
>    │     │     ├── cityName27: String                                       │
>    │     │     └── country: Country                                         │
>    │     ├── type: CustomerType (PERSON | COMPANY)                          │
>    │     ├── companyName: String                                            │
>    │     ├── firstName: String                                              │
>    │     ├── lastName: String                                               │
>    │     └── salutation: Salutation                                         │
>    └────────────────────────────────────────────────────────────────────────┘
> ```
>
>
>

#### API Endpoints

|  |  |  |
|----|----|----|
| Service | Endpoint | Purpose |
| luzfin-finance | `GET /{tenant}/companies/{companyId}/customers/{id}` | Fetch single customer |
| luzfin-finance | `POST /{tenant}/companies/{companyId}/customers/fetch` | Bulk fetch by URIs |

#### Data Model Hierarchy

> [!note]- Data Model
>
>
>
> ```
> Customer
> ├── customerType: CustomerType (PERSON or COMPANY)
> ├── person: Person (when customerType=PERSON)
> │   ├── firstName, lastName
> │   ├── salutation
> │   ├── language
> │   └── addressList: List<Address>
> │       └── Address[0]
> │           ├── addressLines (street)
> │           ├── additionalAddress (c/o, apt)
> │           ├── city
> │           │   ├── addressPostcode
> │           │   ├── cityName27
> │           │   └── country
> │           ├── type (PERSON/COMPANY - for alternate recipient)
> │           ├── companyName (if type=COMPANY)
> │           ├── firstName, lastName (if type=PERSON)
> │           └── salutation
> │
> └── company: Company (when customerType=COMPANY)
>     ├── name
>     ├── language
>     └── addresses: List<Address>
>         └── Address[0] (same structure as above)
> ```
>
>
>

#### Key Files in Data Flow

|  |  |  |
|----|----|----|
| File | Location | Purpose |
| `InvoiceRunServiceController.java` | `service/controller/` | Entry point, calls caching service |
| `KlaraAGCachingService.java` | `service/` | Caches customer data, calls REST client |
| `LuzfinFinanceRestClientService.java` | `service/` | Wrapper for REST client |
| `LuzfinFinanceRestClient.java` | `rest/caller/` | REST client interface (MicroProfile) |
| `Customer.java` | `fin/model/` | Customer model |
| `Person.java` | `fin/model/` | Person model with addressList |
| `Company.java` | `fin/model/` | Company model with addresses |
| `Address.java` | `fin/model/` | Address model with all fields |
| `City.java` | `person/model/` | City model with postcode, name, country |

------------------------------------------------------------------------

### Root Cause Analysis

#### Possible Issues in Data Source

1.  **Backend API (luzfin-finance) returns incomplete data**

    - The API might not return all address fields

    - City object might be missing `addressPostcode` or `cityName27`

    - Address `type` field is set but corresponding name fields are empty

2.  **JSON Deserialization mismatch**

    - Field names in API response don't match model properties

    - Nested objects (City, Country) not properly mapped

3.  **Data inconsistency between sources**

    - `company.name` vs `address.companyName` have different values

    - Address fields populated differently based on how customer was created

#### Recommended Debug Steps

1.  **Add logging to trace API response**

```
    // In KlaraAGCachingService.getCustomerCache()
    Customer customer = luzfinFinanceRestClientService.getCustomerById(...);
    LOGGER.info("Customer address data: {}",
        new Gson().toJson(customer.getCompany().getAddresses()));
```

2.  **Check luzfin-finance API directly**

    - Call the API endpoint directly and inspect the JSON response

    - Verify all address fields are present

3.  **Compare source data vs Customer model**

    - Check if the original data (pic1) matches what's stored in luzfin-finance

    - Verify the mapping from source system to luzfin-finance

## Referenced Information from External Team

### Extract info from ticket LUZ-144345

![[image-20251204-021410.png]]

%% ai-graph-start %%

**Related notes:**
- [[Troubleshooting articles]]
- [[Invoice Run v2 silently skips an individual whose store customer has a blank partner_uri]]
- [[Invoice Run V2UAT - Execute - Apply Distributed Cache for customer information during the process of Invoice Run V2ecute]]
- [[Sprint 158 - Invoice Run V2 Executive Overview]]
- [[Invoice Run V2UAT - Apply Distributed Cache for customer information during the process of Invoice Run V2]]

%% ai-graph-end %%