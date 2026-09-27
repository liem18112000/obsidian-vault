---
title: "Fit OneAPI Postgres data model to MongoDB"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/48375595082/Fit+OneAPI+Postgres+data+model+to+MongoDB
space: "LUZ"
topic: infra
relevance: 0.806
depth: 2.8
updated: 2025-03-25
attachments: 0
tags:
  - confluence
  - infra
  - space/luz
---

# Fit OneAPI Postgres data model to MongoDB

> [!info] Imported from Confluence
> Space **LUZ** · updated 2025-03-25 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/48375595082/Fit+OneAPI+Postgres+data+model+to+MongoDB)
> Relevance 0.806 · topic `infra`

<span class="confluence-jim-macro jira-issue conf-macro output-block" client-id="SINGLE_d3f195c5-8684-3b17-b4f6-e9ee3a0b0fe2_48375595082_712020:87b0f7f1-aaab-4406-a25d-fa0fc075c4d4" hasbody="false" jira-key="LUZ-131212" macro-id="7189affa-e706-4c5d-9798-43139a08a627" macro-name="jira"> <a href="https://axonivy.atlassian.net/browse/LUZ-131212" class="jira-issue-key"><span class="aui-icon aui-icon-wait issue-placeholder"> </span>LUZ-131212</a> - <span class="summary">Getting issue details...</span> <span class="aui-lozenge aui-lozenge-subtle aui-lozenge-default issue-placeholder">STATUS</span> </span>

## Overview

How can the OneAPI Postgre data model be fitted to MongoDB collection for unified monitoring and later use in OneAPI processing?

## Database schema

All tenants share a common collection in a single database  
To see the pros and cons, refer here: <a href="https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/48189767681/Database+of+Monitoring#Multi-tenant-architecture-in-MongoDB-Atlas" data-card-appearance="inline" rel="nofollow">https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/48189767681/Database+of+Monitoring#Multi-tenant-architecture-in-MongoDB-Atlas</a>

## Ideas

**Each table (Delivery, Document, Recipient) in Postgres has a corresponding MongoDB collection**

Pros

- Allows for easier querying and updates, you can target and extract only the exact information you need

- Giving the flexibility to retrieve specific data from each collection

- Easy for migrating oneAPI from Postgre to MongoDB(later)

Cons

- Requires multiple queries to retrieve combined data, which can be less efficient.

- It may not fully leverage the flexibility of MongoDB's document model.

**Combine all tables in Postgres into the finalized monitoring MongoDB collection → Message collection**

Pros

- A single query retrieves all related data, improving performance.

- Data is optimized for unified monitoring, as it's already in the desired format.

- Leverages MongoDB's document model for efficient data storage.

- Data for read-heavy operations → The single combined collection is generally the better choice

Cons

- More complex updates(?)

- If we need only a subset of data, retrieving the whole document may be less efficient. (?)

## Meeting note

**Meeting 14/03/2025**

- A single combined collection will be better if changing to MongoDB

- Hacka will have a meeting next week with Yan to review the JSON schema, to fulfill data if needed

- After that, propose the JSON schema with Daniel

**Meeting 17-18/03/2025**

- Yan, Sebastian, Hacka team, and Titan team finished reviewing the JSON Schema

- Hacka will set up a meeting with Yan, Sebastian, Titan, Future, and Daniel to review about the changes in the previous meetings.

**Meeting 24/03/2025**

- Daniel will review the JSON schema and give feedback

- Yan continues to discuss with the business team 'the “business status“ of message

## MongoDB Collections

### Single combined collection

Refer JSON schema for monitoring documentation: [JSON schema for monitoring](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/48201891847/JSON+schema+for+monitoring)

Refer Epost communication monitoring repository for the final version of the JSON schema: <a href="https://bitbucket.org/axonivy-prod/epost_communication_monitoring/src/master/" class="external-link" data-card-appearance="inline" rel="nofollow">https://bitbucket.org/axonivy-prod/epost_communication_monitoring/src/master/</a>
