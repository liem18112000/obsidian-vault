---
ai_hash: a69929ef4cc4c432
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
confluence_id: '48867967003'
confluence_path: 'Team Kepler > Developer note > LUZ Audit Refactor- 2025-2026 > LUZ
  Critical Concerns: Brief Summary'
created: 2025-11-12
entities: []
source: Confluence · TK - Team Kepler
status: reference
tags:
- confluence
- luz-audit
title: LUZ Audit - Basic Understanding Guide
type: source
updated: 2025-11-12
url: https://axonivy.atlassian.net/wiki/spaces/TK/pages/48867967003/LUZ+Audit+-+Basic+Understanding+Guide
---

# LUZ Audit - Basic Understanding Guide

*Confluence source · Team Kepler › Developer note › LUZ Audit Refactor- 2025-2026 › LUZ Critical Concerns: Brief Summary · [view original](https://axonivy.atlassian.net/wiki/spaces/TK/pages/48867967003/LUZ+Audit+-+Basic+Understanding+Guide) · updated 2025-11-12*

------------------------------------------------------------------------

### What is LUZ Audit?

LUZ Audit is an **enterprise audit logging system** that records and tracks all activities in the LUZ Document Management System. Think of it as a secure, tamper-proof activity log that records "who did what, when" for compliance and security purposes.

------------------------------------------------------------------------

### Core Concepts

#### 1. Audit Events

Every action in the system creates an **audit event** that records:

- **What happened**: Document created, file uploaded, folder deleted, etc.

- **Who did it**: User information

- **When it happened**: Timestamp

- **Where**: Which tenant (company)

- **Result**: Success or failure

**Example Events:**

- User John created a document

- User Mary downloaded a file

- User Bob deleted a folder

- User Alice uploaded a document

#### 2. Fingerprint Chain (Tamper Detection)

The system uses a **blockchain-like fingerprint chain** to detect if anyone tries to modify or delete audit logs.

**How it works:**

```
Log 1 → Fingerprint A
Log 2 → Fingerprint B (calculated using Log 2 + Fingerprint A)
Log 3 → Fingerprint C (calculated using Log 3 + Fingerprint B)
```

Each log's fingerprint depends on the previous one, creating a chain. If someone modifies Log 2, the fingerprints won't match anymore, proving tampering occurred.

**Key Points:**

- Uses SHA-256 hashing algorithm

- Creates an **immutable chain of custody**

- **Cannot be processed in parallel** (each log must wait for the previous one)

- Stored in `LastFingerprint` document in MongoDB

#### 3. Digital Timestamping (Legal Proof)

The system can add **RFC 3161 digital timestamps** to audit logs, providing legal proof that a log existed at a specific time.

**Use Case:** In court, you can prove "This document existed and was signed on January 15, 2025 at 10:30 AM"

**How it works:**

- Connects to Time Stamp Authority (TSA) servers

- Gets cryptographic timestamp certificate

- Embeds timestamp in audit records during export

#### 4. Multi-Tenant Architecture

The system supports **multiple companies (tenants)** using the same application:

- Each tenant's data is completely isolated

- Tenant ID is part of every API call: `/api/{tenant-id}/audits`

- Separate storage folders in Google Cloud Storage

- Separate MongoDB collections

------------------------------------------------------------------------

### System Architecture

#### Layered Architecture

**Layers:**

1.  **REST Resources**: Endpoints, request/response handling (AuditLogResource, JobResource)

2.  **Service Layer**: Business logic, transactions (AuditLogCreatingService, ExportService)

3.  **Integration Layer**: External service communication (RestClients, JsonStore, GCS, Vault)

4.  **External Services**: Cloud services, storage (MongoDB, GCS, Vault, Pub/Sub, TSA)

#### Audit Log Creation Flow

**Process:**

1.  Client sends POST /{tenant}/audits

2.  AuditLogResource assigns transaction ID

3.  Publish event to MessageBroker → Google Pub/Sub

4.  **Immediate response to client** (201 Created)

5.  **Asynchronous background processing:**

    - Pub/Sub pulls message (10 threads)

    - AuditLogCreatingService processes (@Stateless, @Transactional)

    - Encrypt sensitive fields via Vault (if needed)

    - Store audit log in MongoDB/JsonStore

    - FingerprintService calculates fingerprint:

      - Get LastFingerprint (with version)

      - Calculate SHA-256 hash

      - Update LastFingerprint (optimistic locking)

    - Update audit log with fingerprint

    - Acknowledge message

#### Export Process Flow

**Steps:**

1.  POST /jobs/exports triggers ExportJobForTenantEvent

2.  ExportJobController observes event (@ObservesAsync)

3.  ExportService.exportAuditLogs executes:

    - Query MongoDB (500 records/page)

    - Build CSV files (100k records/file)

    - Add RFC 3161 timestamp (if requested)

    - Store CSV to GCS (4-10 threads)

    - Update export metadata in MongoDB

    - Create audit event for export completion

#### Fingerprint Validation Flow

**Process:**

1.  ProofOfHistoryService starts validation

2.  Query all audit logs (oldest to newest)

3.  Get LastFingerprint from MongoDB

4.  For each audit log:

    - Calculate expected fingerprint (SHA-256)

    - Compare with stored fingerprint

    - If match: continue to next log

    - If mismatch: tampering detected, create failed audit event

5.  All match: create success audit event

#### Service Dependencies

|  |  |
|----|----|
| Service | Purpose |
| **jwt-service** | Authentication |
| **luz-vault** | Encryption & Secrets |
| **luz-jsonstore** | MongoDB Metadata |
| **luz-message-broker** | Pub/Sub Publishing |
| **luz-cache** | Distributed Caching |
| **Google Cloud Storage** | Export Files |
| **Google Cloud Pub/Sub** | Event Messaging |
| [tsa.pki.admin.ch](http://tsa.pki.admin.ch) | Swiss TSA (primary) |
| [http://freetsa.org](http://freetsa.org) | Free TSA (fallback) |

#### Technologies

- **Java 17** + Jakarta EE 8

- **WildFly 26** application server

- **MongoDB** via luz_jsonstore REST service

- **Google Cloud** (Storage, Pub/Sub)

- **Docker + Kubernetes**

------------------------------------------------------------------------

### Main Features

#### 1. Create Audit Logs

**API:** `POST /api/{tenant-id}/audits`

**Process:**

1.  Client sends audit event

2.  System assigns transaction ID

3.  Message published to Google Pub/Sub

4.  **Immediate response to client** (201 Created)

5.  Background processing:

    - Store in MongoDB

    - Calculate fingerprint

    - Update chain

    - Encrypt sensitive fields

**Benefit:** Client doesn't wait for database operations

#### 2. Query Audit Logs

**API:** `GET /api/{tenant-id}/audits?from=...&to=...&eventType=...`

**Features:**

- Search by date range, event type, user, etc.

- Pagination (500 records per page)

- Filter and sort capabilities

- Multi-tenant isolation enforced

#### 3. Export to CSV

**API:** `POST /api/{tenant-id}/jobs/exports`

**Process:**

1.  Query MongoDB (500 records per page)

2.  Build CSV files (100,000 records per file max)

3.  Optionally add digital timestamps (RFC 3161)

4.  Compress to ZIP archive

5.  Upload to Google Cloud Storage

6.  Create audit event for export completion

**Use Case:** Download all audit logs for compliance reporting or legal investigation

#### 4. Download Exported Files

**API:** `GET /api/{tenant-id}/audits/download?from=...&to=...`

**Process:**

1.  Retrieve encrypted CSV files from Google Cloud Storage

2.  Decrypt sensitive fields (using Vault)

3.  Stream ZIP archive to client

#### 5. Proof of History Validation

**API:** `POST /api/{tenant-id}/jobs/proof-of-history`

**Process:**

1.  Query all audit logs (oldest to newest)

2.  Recalculate fingerprints from scratch

3.  Compare with stored fingerprints

4.  Report any mismatches (tampering detected)

**Use Case:** Verify audit log integrity, detect unauthorized modifications

#### 6. Fingerprint Correction

**API:** `POST /api/{tenant-id}/jobs/correct-fingerprint`

**Process:**

1.  Identify broken fingerprint chains

2.  Recalculate all fingerprints sequentially

3.  Update LastFingerprint document

4.  Batch processing with configurable limits

**Use Case:** Repair fingerprint chains after system errors or data migration

------------------------------------------------------------------------

### Event Types

|                    |                                               |
|--------------------|-----------------------------------------------|
| Event Type         | Examples                                      |
| **Document**       | Create, Read, Update, Delete, Download, Share |
| **Folder**         | Create, Move, Delete, Rename                  |
| **File**           | Upload, Download, Scan, Delete                |
| **Group**          | Create, Update, Delete, Add Member            |
| **System**         | Export, Timestamp, Fingerprint Validation     |
| **Authentication** | Login, Logout, Permission Change              |

------------------------------------------------------------------------

### Security Features

#### 1. Authentication

- **JWT (JSON Web Token)** with RS512 encryption

- Bearer token in Authorization header

- Token validated on every request

- Service-to-service authentication using dedicated credentials

#### 2. Encryption

- **Sensitive fields encrypted** using HashiCorp Vault

- **Vault Transit Engine** for encryption-as-a-service

- Base64 encoding for encrypted values

- Decryption only on authorized download

#### 3. Fingerprint Chain

- **SHA-256 HMAC** for tamper detection

- Each log linked to previous via fingerprint

- **Immutable audit trail**

- Proof of history validation

#### 4. Digital Timestamping

- **RFC 3161 TSA** integration

- Legal proof of existence at specific time

- Certificate chain validation

- Non-repudiation

#### 5. Multi-Tenant Isolation

- Tenant-specific data segregation

- Path-based tenant routing

- Permission-based access control

- Separate storage and collections

------------------------------------------------------------------------

### Performance Characteristics

#### Asynchronous Processing

**Benefit:** Client receives immediate response without waiting for:

- Database writes

- Fingerprint calculation

- Encryption operations

- Chain updates

**Mechanism:** Google Cloud Pub/Sub with 10 concurrent message processors

#### Thread Pools

|                            |         |                                      |
|----------------------------|---------|--------------------------------------|
| Pool                       | Threads | Purpose                              |
| **CSV Storage**            | 4-10    | Upload files to Google Cloud Storage |
| **Tenant Deletion**        | 3-9     | Delete tenant data                   |
| **Pub/Sub Pull**           | 10      | Process incoming audit events        |
| **Fingerprint Correction** | 4       | Fix broken fingerprint chains        |

#### Caching

**Dual-Layer Strategy:**

- **Memory cache**: Fast, local (300-second TTL, 2000 entries)

- **Distributed cache**: Shared across instances (via luz_cache service)

**Cached Data:**

- Tenant authentication tokens

- Reduces authentication overhead

#### Pagination and Limits

- **MongoDB queries**: 500 records per page

- **CSV export**: 100,000 records per file

- **Prevents memory exhaustion**

------------------------------------------------------------------------

### Limitations and Constraints

#### 1. Sequential Fingerprint Processing

**Issue:** Each audit log must wait for the previous fingerprint to complete

**Impact:**

- Cannot parallelize for single tenant

- Potential bottleneck for high-volume tenants

- Optimistic locking may cause retries under high concurrency

**Mitigation:** Special handling for high-volume tenants (configured separately)

#### 2. REST-based MongoDB Access

**Issue:** Access via luz_jsonstore REST API instead of direct connection

**Impact:**

- Network overhead

- Additional latency

- JSON serialization/deserialization overhead

**Benefit:** Service decoupling, abstraction layer

#### 3. Large Export Memory Usage

**Issue:** 100,000 records per file requires significant memory

**Impact:**

- Memory pressure during large exports

- 2-hour timeout for very large datasets

#### 4. Vault Encryption Overhead

**Issue:** Network call for each encryption/decryption operation

**Impact:**

- Additional latency

- May become bottleneck at high volume

------------------------------------------------------------------------

### Common Operations

#### Create an Audit Log

```
POST /api/{tenant-id}/audits
Authorization: Bearer <jwt-token>

{
  "eventType": "DOCUMENT",
  "eventSubType": "CREATE",
  "status": "SUCCESS",
  "userId": "user123",
  "documentId": "doc456",
  "details": "Created new document"
}
```

#### Query Audit Logs

```
GET /api/{tenant-id}/audits?from=2025-01-01&to=2025-01-31&eventType=DOCUMENT
Authorization: Bearer <jwt-token>
```

#### Export Audit Logs

```
POST /api/{tenant-id}/jobs/exports
Authorization: Bearer <jwt-token>

{
  "from": "2025-01-01T00:00:00Z",
  "to": "2025-01-31T23:59:59Z",
  "includeTimestamp": true
}
```

#### Validate Proof of History

```
POST /api/{tenant-id}/jobs/proof-of-history
Authorization: Bearer <jwt-token>
```

------------------------------------------------------------------------

### Deployment

#### Infrastructure

- **Platform**: Kubernetes (Google Kubernetes Engine)

- **Base Image**: Custom WildFly 26 Docker image

- **Registry**: Google Artifact Registry (europe-west6)

- **Configuration**: Environment variables + XML snippets

#### External Services Required

1.  **MongoDB** (via luz_jsonstore)

2.  **Google Cloud Storage** (bucket: luz_audit_storage)

3.  **Google Cloud Pub/Sub** (asynchronous messaging)

4.  **HashiCorp Vault** (encryption)

5.  **JWT Service** (authentication)

6.  **Message Broker** (event publishing)

7.  **Distributed Cache** (multi-instance sync)

8.  **TSA Services** (optional, for timestamping)

#### Health Monitoring

- **Liveness**: `/health/live`

- **Readiness**: `/health/ready`

- **Metrics**: `/metrics`

- **API Documentation**: `/openapi-ui` (Swagger UI)

- **Version Info**: `/api/version`

------------------------------------------------------------------------

### Key Takeaways

1.  **LUZ Audit is a secure, tamper-proof audit logging system** for compliance and security

2.  **Fingerprint chain prevents unauthorized modifications** using blockchain-like technology

3.  **Asynchronous processing** provides fast response times to clients

4.  **Multi-tenant architecture** supports multiple companies on same platform

5.  **Digital timestamping** provides legal proof of document existence

6.  **Enterprise-grade security** with JWT, encryption, and access control

7.  **Sequential fingerprint processing** is both a strength (security) and limitation (performance)

8.  **Export to CSV** for compliance reporting and legal investigations

------------------------------------------------------------------------

### Further Reading

For detailed technical analysis, see the full document: `analysis.md`

For performance concerns and recommendations, see: `luz-audit-concerns.md`

%% ai-graph-start %%

**Related notes:**
- [[Luz-audit]]
- [[LUZ Audit Refactor- 2025-2026]]
- [[Solution - Enhanced Chain-Signature Hybrid]]
- [[Investigation Stories - Audit Logs Current Implementation]]
- [[Luz Audit System - Performance Optimization Proposal]]

%% ai-graph-end %%