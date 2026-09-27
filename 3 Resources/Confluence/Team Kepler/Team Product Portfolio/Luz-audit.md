---
ai_hash: 5f72aada77b21490
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
confluence_id: '49013424356'
confluence_path: Team Kepler > Team Product Portfolio
created: 2026-01-05
entities: []
source: Confluence · TK - Team Kepler
status: reference
tags:
- confluence
- luz-audit
title: Luz-audit
type: source
updated: 2026-01-23
url: https://axonivy.atlassian.net/wiki/spaces/TK/pages/49013424356/Luz-audit
---

# Luz-audit

*Confluence source · Team Kepler › Team Product Portfolio · [view original](https://axonivy.atlassian.net/wiki/spaces/TK/pages/49013424356/Luz-audit) · updated 2026-01-23*

### 1. Definition & Business Purpose

**luz-audit** is a blockchain-based **audit trail module** for the DOCUMENT MANAGEMENT domain.
It records all relevant events related to document management and system usage (e.g. document creation, enrichment, update, deletion, exports), forming an immutable and verifiable history for each tenant.

Key business goals:

- **Regulatory compliance & legal evidence**

  - Record every significant event in the **information system** and the **document lifecycle** with a precise UTC timestamp.

  - Provide a tamper-evident, chronological audit trail which can serve as legal evidence.

- **Integrity & non‑repudiation via blockchain model**

  - Each event contains a **fingerprint** (hash) that includes data from the previous event of the same tenant, creating a chain of events (proof of history).

  - Any modification or removal of an event breaks the chain and becomes detectable.

- **Tenant‑level export & data ownership**

  - Events are regularly exported to **Google Cloud Storage (GCS)** as CSV files.

  - Upon termination of the service, all tenant audit data can be exported and Klara retains no customer information.

- **Operational transparency & traceability**

  - Enable support and business stakeholders to answer “who did what, when, and on which object” in a reliable way.

For a more detailed, product‑oriented view, see:
<https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47432958855/LUZ-AUDIT+DOCUMENTATION>

------------------------------------------------------------------------

### 2. High‑Level Architecture & Relations with Other Modules

#### 2.1 Role in the Luz Ecosystem

- **Domain**: DOCUMENT MANAGEMENT

- **Consumers**: primarily **luz-docs**, potentially other services sending audit events.

- **Core responsibilities**:

  - Receive events from other modules (event producers) via **Google Pub/Sub**.

  - Validate and enrich events, attach sequential fingerprints.

  - Persist the event chain per tenant.

  - Periodically export validated events to **GCS**.

  - Provide secure export/download capabilities for full tenant audit data.

#### 2.2 Relation to luz-docs (interactive module/service)

- **luz-docs** manages:

  - Document metadata (MongoDB)

  - Document files (GCS)

  - Security, access control, enrichment, lifecycle (soft/hard delete, etc.)

- **luz-audit** provides:

  - **Blockchain-based audit logging for all document management activities.**

  - It **receives events from luz-docs (via Pub/Sub)** and logs them immutably.

  - It store the audit event in luz-jsonstore service tenant DB

  - It **exports audit trails to GCS** for long‑term archival and compliance.

Typical interactions:

- When a user or system:

  - uploads a document

  - edits metadata

  - deletes or restores a document
    luz-docs creates an **audit event** and publishes it to a **Pub/Sub topic** dedicated to luz-audit.

- luz-audit:

  - Consumes these events from Pub/Sub (subscription).

  - Validates event order and integrity.

  - Constructs/extends the **event chain** per tenant by generating a new fingerprint referencing the previous event’s fingerprint.

  - Stores events and later exports them to GCS.

![[audit-general.png]]

#### 2.3 Other technical relationships

- **Google Cloud Pub/Sub**

  - **Topic** (e.g. `LUZ_AUDIT_PUBSUB_TOPIC_ID`) for event producers to publish messages.

  - **Subscription** (e.g. `LUZ_AUDIT_PUBSUB_SUBSCRIPTION_ID`) consumed by luz-audit for processing.

- **luz-jsonstore**

  - tenant database: Audit service tenant
    [KLARA SERVICE TENANT](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47104721347/KLARA+SERVICE+TENANT)

  - Store auditlogs sequently of each tenant into one service tenant database - auditlogs collection

  - Store fingerPrint of each tenant in lastFingerPrintcollection.

- **Google Cloud Storage (GCS)**

  - Bucket (e.g. `LUZ_AUDIT_STORAGE_BUCKET_NAME`) where luz-audit exports validated events as CSV files.

  - Per‑tenant folder structure;

- **Service module / Service tenant**

  - luz-audit is registered as a **service module** in `luz-tenant` for machine‑to‑machine authentication.

  - Client credentials (client_id, client_secret) are stored securely and exposed to runtime via Kubernetes secrets.

------------------------------------------------------------------------

### 3. Event Model & Blockchain‑like Chain

An audit event typically contains:

- **UniqueID** – internal identifier of the event.

- **Transaction ID** – ID per transaction (e.g. request or business transaction).

- **Event Type / Sub‑type** – e.g. `CREATE/DOCUMENT`, `SET/CONTENT_TYPE`, `DELETE/DOCUMENT`, `LOGIN/TENANT`.

- **Software module** – source module (e.g. `luz-docs` or other).

- **Tenant ID** – tenant to which this event belongs.

- **Object ID** – target object (e.g. document id, folder id).

- **User** – user id or service id that triggered the event.

- **Event date/time (UTC)** – precise timestamp in UTC.

- **Event data** – JSON payload with context metadata (request details, previous state, changed values, etc.).

- **Event fingerprint** – hash that includes this event’s data and the **previous event fingerprint for the same tenant**.

- **Event Description** - The short description of the event.

- **Event Status** - Status **SUCCESSFULLY** or **FAILED**

The **fingerprint** is what creates the blockchain‑like **proof of history**:

- For each tenant, events are sorted by time and linked via fingerprints.

- If any event is changed or deleted, the chain **after** that event becomes invalid.

- The system periodically verifies the integrity of the chain before exporting events.

Example events (excerpt from [https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/46967948288/luz-audit):](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/46967948288/luz-audit)

- Tenant login

- Document creation

- Content type enrichment

- etc., each with a generated fingerprint.

------------------------------------------------------------------------

### 4. Export & Archiving

#### 4.1 Periodic export

- **Schedule**: Every night, luz-audit exports validated events of each tenant to **GCS**.

- **Format**: Typically CSV (per tenant, per period).

- **Validation before export**:

  - Verify that all events for the tenant form a **valid, continuous chain** (no missing fingerprints, no hash mismatch).

  - Only if integrity checks pass, the export is considered valid.

#### 4.2 Log archiving & TSA integration

- Exports are stored under a tenant-specific path in GCS.

- The **export operation itself is logged in luz-audit** as an event, including:

  - Export time

  - Target bucket/folder

  - Reference to TSA signature (if used)

- Logs may be signed by an official **Time Stamping Authority (TSA)**:

  - TSA returns a signature and timestamp which are either:

    - Stored in the event’s data field, or

    - Stored as a separate file in GCS
      (details “to be decided” in the original spec, implementation-dependent).

------------------------------------------------------------------------

### 5. Interaction Overview (Sequence Diagram)

![[audit-sequence-diagram.png]]

------------------------------------------------------------------------

The major disadvantages:

1.  The nature of the fingerprint cause the verify process become strictly SEQUENTIAL. It mean that this process could not run CONCURRENTLY. It mean that this service is hardly scaled.

### 6. Summary

- **Business requirement**: Provide a legally compliant, immutable, and exportable audit trail for document and system events, with strong integrity guarantees (blockchain‑like chaining, TSA) and aligned archival policy with document retention.

- **Relation with interactive modules/services**: luz-audit is a backend audit service, primarily driven by **luz-docs** and other modules via Pub/Sub events; it does not expose a direct UI to end users but supports them indirectly through traceability, compliance, and evidence.

- **Architecture**: Event‑driven, Pub/Sub‑based ingestion, blockchain‑style fingerprinting, nightly GCS exports, and controlled operator access.

%% ai-graph-start %%

**Related notes:**
- [[LUZ Audit - Basic Understanding Guide]]
- [[Luz-vault]]
- [[LUZ Audit Refactor- 2025-2026]]
- [[Solution - Enhanced Chain-Signature Hybrid]]
- [[One API Module Responsibilities]]

%% ai-graph-end %%