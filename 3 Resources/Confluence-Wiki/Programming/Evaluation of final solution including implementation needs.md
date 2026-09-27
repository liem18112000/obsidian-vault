---
ai_hash: 3eb0e7545d90f721
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 0
depth: 2.49
entities: []
relevance: 0.729
source: https://axonivy.atlassian.net/wiki/spaces/AI/pages/49144758376/Evaluation+of+final+solution+including+implementation+needs
space: AI
status: reference
tags:
- confluence
- programming
- space/ai
title: Evaluation of final solution including implementation needs
topic: programming
type: source
updated: 2026-02-23
---

# Evaluation of final solution including implementation needs

> [!info] Imported from Confluence
> Space **AI** · updated 2026-02-23 · [open original](https://axonivy.atlassian.net/wiki/spaces/AI/pages/49144758376/Evaluation+of+final+solution+including+implementation+needs)
> Relevance 0.729 · topic `programming`

<span class="status-macro aui-lozenge aui-lozenge-visual-refresh aui-lozenge-error conf-macro output-inline" hasbody="false" macro-id="6f107395-c2ea-4128-a4c7-7f70ec849c5f" macro-name="status">DRAFT</span>

# Solution 1: luz_tenant_dir: ‘allowScannedLetter’ flag

Each address of a tenant has an ‘allowScannedLetter’ flag. This value of this flag is synchronised for scanningprivate and scanningbusiness tenants based on subscription status and selection in the the apps. The scanningbusiness_premium tenants has this flag set via manual process.

Investigation details: [Investigation: Require active scanning subscription for matched tenants](https://axonivy.atlassian.net/wiki/spaces/AI/pages/49130078259/Investigation+Require+active+scanning+subscription+for+matched+tenants#Solution-1%3A-luz_tenant_dir%3A-%E2%80%98allowScannedLetter%E2%80%99-flag)

## Why?

- No new access required. Analyze API already has access to read the addresses of tenants and this flag

- It is a simple boolean flag

- This flag was previously used (together with subscription status) to create the list of addresses used on in the Tessi solution

- The business logic of a 5 day buffer at the end of subscription is considered.

## Why not?

- We identified some data quality issues with this flag, where non-subscribed tenants still had a ‘true’ flag set. This seems to be a synchronisation issue, or potentially old data.

- The synchronisation process takes into account the 5 days buffer period added after a subscription ends (business logic)

## Decision

tbd

## Implementation

1.  An address is matched to a tenant via luz_tenant_dir

2.  Via luz_teannt_dir `/api/tenant-entries/{participant-id}` we then obtain the list of addresses of this tenant

3.  If at least one of these addresses has ‘allowScannedLetter’ = true, we accept this tenant, otherwise we reject the tenant

# Solution 2: luz_store: Read subscriptions directly

The ground truth of subscriptions exist in luz_store. In this solution we read the subscriptions directly and, thereby, get the most up-to-date subscription information.

Investigation details: [Investigation: Require active scanning subscription for matched tenants](https://axonivy.atlassian.net/wiki/spaces/AI/pages/49130078259/Investigation+Require+active+scanning+subscription+for+matched+tenants#Solution-2%3A-luz_store%3A-Read-subscriptions-directly)

## Why?

- Ground truth subscription information

## Why not?

- Access to luz_store is restricted, making it more challenging to safely access this data

- The business logic of a 5 days buffer is not included in the data.

## Decision

*tbd*

## <span class="inline-comment-marker" ref="08679b05-6c73-4e04-bc55-e7ed5ca5c9b5">Implementation</span>

Core issue implementing this solution is the security concern of exposing an all-tenant token.

### Option A: PSC proxy with token upgrade

1.  An address is matched to a tenant via luz_tenant_dir

2.  A tenant specific access token is obtained via luz_sec (from Analyze API)

3.  Analyze API makes a request via the PSC to obtain a subscription data for the tenant, it includes the access token obtained in step 2 (`/api/{company-tenant-id}/subscriptions`)

    1.  On ePost side

        1.  PSC proxy validates the tenant specific access token (correct tenant etc)

        2.  If valid token: Forwards the request with an ‘all-tenant-access’ token to luz_store

4.  Analyze API process the subscription data, and validates if an active subscription to either scanningprivate, scanningbusiness, or scanningbusiness_premium exists. The validation should also include a 5 days buffer period after a subscription is ended

### Option B: Team Future’s suggestion

1.  A ‘VN team’ implements a container according to the [Recipe: How to use long-live token in cronjob](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/48047882241/Recipe+How+to+use+long-live+token+in+cronjob) page.

2.  The ‘VN team’ sends the refresh token exported from this container via email to team AI

3.  Team AI makes this refresh token available in Analyze API

4.  Analyze API uses this refresh token to obtain role scoped access token via luz_sec

5.  Analyze API makes a request using the token obtained in step 4 to query luz_store

6.  Analyze API process the subscription data, and validates if an active subscription to either scanningprivate, scanningbusiness, or scanningbusiness_premium exists. The validation should also include a 5 days buffer period after a subscription is ended

### Option C: luz_store provides less restricted read access to subscription status

To limit the security issues with the all-tenants token, the question is if read access to tenant subscription status could not be modified. Can the read access to subscription data bt modified to be role based and only required a tenant specific access token (equivalent to the access model used in the luz_tenant_dir)?

# Solution 3: luz_tenant_dir: List Scanning Tenants

Luz_tenant_dir already provides access to a list of addresses with tenant identifiers of active scanning tenants (previously used by Tessi). This currently does not include the scanningbusiness_premium tenants. The 5 days buffer at the end of subscription is included.

Investigation details: [Investigation: Require active scanning subscription for matched tenants](https://axonivy.atlassian.net/wiki/spaces/AI/pages/49130078259/Investigation+Require+active+scanning+subscription+for+matched+tenants#Solution-3%3A-luz_tenant_dir%3A-List-Scanning-Tenants)

## Why?

- Existing solution to get scanning tenants

- No special access token needed from Analyze API side

- Would only require a minor update to include scanningbusiness_premium tenants (luz_tenant_dir)

- The business logic of a 5 days buffer at the end of the subscription is included

## Why not?

- More complexity on Analyze API side to cache this list of active tenants

- More database heavy as it requires processing all tenants subscriptions daily

## Decision

tbd

## Implementation

1.  Daily Analyze API will fetch the list of active scanning tenants.

2.  The list will be stored in an Apache Ignite Cache to make it available for all analyze nodes

3.  When a Analyze API receives a tenant id from the luz_tenant_dir lookup, it will check if the tenant id exist in the cached list. If yes, the tenant id is accepted.

# Solution 4: luz_tenant_dir: add parameter to filter tenants based on scanning subscription

## Decision

Outcome of [Architectural Decision – Enforce Active Scanning Subscription](https://axonivy.atlassian.net/wiki/spaces/AI/pages/49141219330/Architectural+Decision+Enforce+Active+Scanning+Subscription) is that no additional filtering should be done in luz_tenant_dir. This solution is therefore rejected.

# Solution 5: luz_tenant_dir provides a look-up path for ‘scanning tenants’

The idea of solution 5 is to build on existing code available to get a list scanning tenant from luz_tenant_dir (i.e. the list solution 3 would use). Luz_tenant_dir has the access to required to luz_store and could provide a look-up path for “scanning tenants”.

Investigation details:[Investigation: Require active scanning subscription for matched tenants](https://axonivy.atlassian.net/wiki/spaces/AI/pages/49130078259/Investigation+Require+active+scanning+subscription+for+matched+tenants#Solution-5%3A-luz_tenant_dir-provides-a-look-up-path-for-%E2%80%98scanning-tenants%E2%80%99)

## Why?

- Analyze API would not need additional access to luz_store

- A complete transversal of tenant subscriptions (as done to generate the list in Solution 3) is not needed.

- Less complexity on Analyze API side to handle a cached list of tenant ids

## Why not?

- Might not follow the design decision made on [Architectural Decision – Enforce Active Scanning Subscription](https://axonivy.atlassian.net/wiki/spaces/AI/pages/49141219330/Architectural+Decision+Enforce+Active+Scanning+Subscription) ?

- More complexity on luz_tenant_dir side

## Decision

tbd

%% ai-graph-start %%

**Related notes:**
- [[Integrating The Analyze API Into luz_scanscenter New Flow Proposal]]
- [[System design]]
- [[How to run export API for specific tenant and date - Manual export]]
- [[Research on Delete Access class]]
- [[Analytics Analyze API call when accessing eArchive]]

%% ai-graph-end %%