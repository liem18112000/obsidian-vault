---
ai_hash: 0078621c97242d80
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 0
depth: 2.38
entities: []
relevance: 0.755
source: https://axonivy.atlassian.net/wiki/spaces/ISMS/pages/49302831167/Communities+Privacy+Concept+Working+Page+for+PO+and+Engineering
space: ISMS
status: reference
tags:
- confluence
- architecture
- space/isms
title: Communities Privacy Concept – Working Page for PO and Engineering
topic: architecture
type: source
updated: 2026-06-04
---

# Communities Privacy Concept – Working Page for PO and Engineering

> [!info] Imported from Confluence
> Space **ISMS** · updated 2026-06-04 · [open original](https://axonivy.atlassian.net/wiki/spaces/ISMS/pages/49302831167/Communities+Privacy+Concept+Working+Page+for+PO+and+Engineering)
> Relevance 0.755 · topic `architecture`

## Purpose of this page

This page collects only the points that still need a functional or technical answer from PO, architecture and engineering. Known baseline conditions are treated as fixed and are not asked again.

This page is intended to support:

- Privacy concept

- ISDS

- Protection needs assessment

- DPIA

------------------------------------------------------------------------

# Refined by <a href="https://axonivy.atlassian.net/wiki/people/605414fe66c87900683d008b?ref=confluence" class="confluence-userlink user-mention" data-account-id="605414fe66c87900683d008b" target="_blank" data-base-url="https://axonivy.atlassian.net/wiki">Gianfranco Gaio</a> and <a href="https://axonivy.atlassian.net/wiki/people/5a096f417174496061892bb5?ref=confluence" class="confluence-userlink user-mention" data-account-id="5a096f417174496061892bb5" target="_blank" data-base-url="https://axonivy.atlassian.net/wiki">Robin Engbersen</a>

## 1. Fixed Baseline Conditions:

- Scope: ePost Communities cover B2B, B2C, and C2C use cases with chat, file sharing, status messages, and group communication. Higher-level features such as tasks or appointments will be built on top of Matrix.

- Platform components: ePost App, ePost Web App, ePost Services, and a Matrix server hosted in Switzerland.

- Data location: All data is hosted in Switzerland. (stimmt das <a href="https://axonivy.atlassian.net/wiki/people/712020:c3358a78-48bb-4869-a798-cf99511aed4f?ref=confluence" class="confluence-userlink user-mention" data-account-id="712020:c3358a78-48bb-4869-a798-cf99511aed4f" target="_blank" data-base-url="https://axonivy.atlassian.net/wiki">Jiyan Akgül</a> ?)

- Encryption: End-to-end encryption (E2EE) applies to messages and attachments in “message events”. Other events like reactions, read receipts, and other communication events within encrypted rooms are unencrypted.

- ~~Encryption: <span class="inline-comment-marker" ref="f8bd6000-21aa-4042-ab0c-a4e2f61c3e32">End-to-end encryption (E2EE) applies to messages, attachments, reactions, read receipts, and other communication events within encrypted rooms</span>. In unencrypted rooms, content is not end-to-end encrypted.~~

- Metadata: Technical metadata (e.g., sender/recipient IDs, room IDs, timestamps, device information) is not end-to-end encrypted.

- Key management: Encryption keys are generated on the device but securely stored server-side in our setup. Upon login, keys are retrieved to allow decryption of the user’s rooms.

- Access: ePost has no access to encrypted communication content. In unencrypted public rooms, content could be accessible per configuration.

## 2. Standard data flow for all use cases

In ePost Communities, any user can create a chat or room, but before doing so, they must provide a recipient credential (email, mobile, or postal address) for verification. The ePost matching mechanism confirms if the recipient is registered and contactable. Once verified, a room is created on the Matrix homeserver, with the ePost App, Web client, and Services facilitating the process. All message content and events are stored on the homeserver. Recipients see system messages and room content based on their membership. History remains unless purged, and if accounts are deleted or suspended, prior messages remain visible, though the user is marked as left. Both mobile and web clients aim for a consistent experience, with the room creation dependent on successful matching.

### **Standard Data Flow – Step-by-Step Description**

1.  **Initiation by User**

    - A user (B2B, B2C, or C2C) initiates a new conversation in the ePost App or Web App.

2.  **Recipient Selection (Source of Data)**

    - The user selects or enters a recipient credential.

    - The credential is typically taken from:

      - the **mobile address book**, or

      - the **“People” section** within the application

    - Supported credentials include:

      - email address

      - mobile number

      - postal address

3.  **Recipient Identification (Matching)**

    - The selected credential is sent to the **ePost matching mechanism**.

    - The system verifies whether the recipient is registered and reachable within ePost Communities.

4.  **Matching Result**

    - **Positive match:** Communication is allowed.

    - **Negative match:** No room/chat can be created.

5.  **Room Creation**

    - A room (1:1 or group) is created on the **Matrix homeserver**.

    - Room configuration (e.g. invite permissions) is defined by the creator.

6.  **Participant Invitation**

    - Participants are invited according to room configuration (power levels / permissions).

7.  **Message Exchange**

    - Messages, attachments, and events are sent via:

      - <span class="inline-comment-marker" ref="91d7c6bd-76c5-4383-8ac6-1f22ab7b35f7">ePost App / Web App → Matrix Server</span>

    - In encrypted rooms:

      - Content is end-to-end encrypted on the client

      - Decryption keys are retrieved from server-side storage (your setup)

    - In unencrypted rooms:

      - Content is processed without E2EE

8.  **Data Processing & Storage**

    - **Stored on Matrix Server:**

      - Message events (encrypted or unencrypted)

      - Attachments (encrypted in E2EE rooms)

      - Technical metadata (sender, timestamp, room ID, etc.)

    - **Transported via ePost Services:**

      - Routing and matching data

    - **Client-side (App/Web):**

      - Temporary caching and local session data

9.  **Data Display to Users**

    - Users see:

      - Messages and attachments of joined rooms

      - System messages (join/leave, changes)

    - Visibility depends on:

      - Room membership

      - History visibility settings

10. **Lifecycle & Persistence**

- Message history is retained by default (Matrix standard)

- Deleted/suspended users:

  - Messages remain visible

  - User is marked accordingly (e.g. deactivated)

- No automatic deletion unless defined by retention policies

11. **Client Consistency**

- Mobile App and Web App provide largely the same functionality

- Minor UI/UX differences may exist

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="ed2bf7c7-037e-4e20-9c3b-a2ab3e56c30d" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
[User (App/Web)]
        │
        │ selects recipient from:
        │ - Mobile Address Book
        │ - People Section
        │ or enters credential manually
        ▼
[Recipient Credential]
(email / mobile / postal address)
        │
        ▼
[ePost Matching Service]
        │
        ├── No match → Stop (no chat possible)
        │
        └── Match found
                │
                ▼
        [Room Creation]
        (Matrix Server)
                │
                ▼
        [Invite Participants]
        (based on room configuration)
                │
                ▼
        [Message Flow]
(App/Web ↔ ePost Services ↔ Matrix Server)
                │
                ├── Encrypted Room (E2EE)
                │       - Content encrypted on client
                │       - Keys retrieved from server (your setup)
                │
                └── Unencrypted Room
                        - Content processed without E2EE
                │
                ▼
        [Storage]
        - Matrix Server:
            • Message events
            • Attachments
            • Technical metadata
        - ePost Services:
            • Matching / routing data
        │
        ▼
        [Client Display]
        - Messages & attachments
        - System messages
        - Based on room membership
        │
        ▼
        [Lifecycle / Persistence]
        - History retained (default)
        - Deactivated users remain visible
        - No automatic deletion (unless defined)
```

</div>

</div>

## **3. Roles, Rights and Administrative Cases**

**Roles:**

- Member

- Moderator

- Administrator

Roles are defined per room and follow the standard Matrix power level model.

### **Permissions**

**Member**

- Send messages and attachments

- Read messages and documents

- React and interact within the room

- Invite participants (depending on room configuration)

**Moderator**

- All member permissions

- Moderate content (e.g. redact messages)

- Manage participants (depending on configuration)

**Administrator**

- Full control over the room

- Manage room settings (name, topic, join rules, visibility)

- Define permissions (power levels)

- Add and remove participants

- Control who can invite others

- Potentially delete or archive content (depending on implementation)

Note: Exact permissions depend on room configuration and can be adjusted by administrators.

### **Administrative and Special Cases**

**User leaves a room**

- The user leaves the room manually or is removed

- A system message may indicate that the user left

- The user loses access to the room immediately

- Past messages remain visible to other participants

**Role change (promotion/demotion)**

- Users can be promoted or demoted (e.g. Member → Moderator → Admin)

- Changes are applied immediately

- Typically no system message is generated

- Permissions update according to the new role

**Account suspension / deactivation (standard Matrix behavior – not fully implemented yet)**

- The user is effectively removed from all rooms

- The user appears as “left” or “deactivated” in room history

- No further access to rooms or content

- Past messages remain visible to other participants

**Account deletion**

- Follows Matrix deactivation behavior

- The account can no longer be used

- Messages, attachments, and interactions remain in the room history

- The user is marked as deactivated (planned / not fully implemented yet)

### **Impact on Rooms and History**

- Conversation history remains for all remaining participants

- Messages and attachments are not deleted when a user leaves or is deactivated

- Access rights are immediately revoked upon leaving, removal, or deactivation

- System messages may indicate join/leave events (depending on configuration)

- Role changes are not necessarily visible in the conversation history

### **Role & Permission Matrix (Current Implementation)**

<div>

|  |  |  |  |
|----|----|----|----|
| **Action** | **Admin (PL 100)** | **Moderator (PL 50)** | **Member (PL 0)** |
| Send messages | Yes | Yes | Yes |
| Read messages | Yes | Yes | Yes |
| Send attachments | Yes | Yes | Yes |
| React to messages | Yes | Yes | Yes |
| Invite users | Yes | Yes | Yes (if allowed) |
| Accept / decline invitations | Yes | Yes | Yes |
| Kick / remove users | Yes | Yes | No |
| Ban / unban users | Yes | Yes | No |
| Redact messages (delete content) | Yes | Yes | No |
| Mute / unmute users | Yes | Yes | No |
| Set power levels | Yes | Yes (up to own level) | No |
| Send state events | Yes | Yes | No |
| Change room settings | Yes | No | No |
| Manage room aliases | Yes | No | No |
| Upgrade room | Yes | No | No |
| Manage history visibility | Yes | No | No |
| Manage room encryption | Yes | No | No |
| Manage membership | Yes | No | No |
| View room members | Yes | Yes | Yes |
| View membership status | Yes | Yes | Yes |
| View roles | Yes | Yes | Yes |
| Set profile picture (avatar) | Yes | Yes | Yes |

</div>

### **Important Clarifications (for Privacy Concept)**

- Permissions are based on **Matrix power levels (PL 0 / 50 / 100)**

- Some features are **not yet implemented** (e.g. ban, mute, room upgrade) but are **conceptually assigned**

- “Invite users” for members is **configurable via room settings**

- “Redact messages” removes content but leaves a **traceable placeholder**

- Only **Admins can change structural room properties** (settings, encryption, visibility)

## **4. Identities and Lifecycle**

**Identity Management**

Private users and organisational users are not strictly separated on a technical level but can be distinguished based on usage patterns and identifiers:

- **Private users**

  - Primarily use the **mobile app**

  - Registered via Post/ePost onboarding

  - Example matrixID @e81802e2-515e-490c-bfce-b2f04b16a855:epost.ch

- **Organisational users**

  - Primarily use the **web client**

  - Are associated with an organisation/tenant

  - Planned: Matrix IDs may include **custom domains** to reflect organisational identity

### **Membership and Role Management**

- Membership in rooms is managed via **Matrix invitations**

- Roles (Member, Moderator, Admin) are assigned **per room**

- Role changes (promotion/demotion) are handled **manually**

- No automated lifecycle or role sync (e.g. from external systems) is currently in place

### **Lifecycle Events and Handling**

1.  **Role Change**

- Roles are updated manually within a room

- Changes take effect immediately

- No automatic system message required

- Access rights change according to the assigned role

2.  **User Leaves Room**

- User leaves voluntarily or is removed

- Access to the room is revoked immediately

- A system message may indicate that the user left

- Conversation history remains visible to other participants

3.  **Account Suspension / Deactivation (Standard Matrix behavior – planned/not fully implemented)**

- User is effectively removed from all rooms

- User appears as “left” or “deactivated” in room history

- No further access to rooms or content

- Messages and attachments remain visible to other participants

4.  **Account Deletion**

- Follows Matrix deactivation logic

- Matrix ID is permanently disabled (not reusable)

- User can no longer access the system

- Existing conversations remain unchanged for other participants

- User is marked as deactivated (planned)

### **Impact on Existing Histories**

<div>

|  |  |  |  |
|----|----|----|----|
| **Scenario** | **Visibility for Others** | **User Representation** | **Messages / Attachments** |
| Role change | No impact | Unchanged | Remain visible |
| User leaves room | No impact | Marked as “left” | Remain visible |
| Account suspended | No impact | Marked as “deactivated” | Remain visible |
| Account deleted | No impact | Marked as “deactivated” | Remain visible |

</div>

### **General Principles**

- Conversation history is **not deleted** when users leave, are removed, or are deactivated

- Access rights are **revoked immediately**, but **historical data remains**

- Identity changes (e.g. deletion, suspension) are **reflected in user representation**, not in content removal

- Lifecycle handling follows **standard Matrix behavior**, with some aspects not yet fully implemented

## **5. Target Architecture and Component View**

The ePost Communities platform is already in production and consists of the following core components:

- **Post/Post/ePost Mobile Apps (iOS / Android)**

  Provide the primary interface for private users to access communities, send messages, and interact with content.

- **ePost Web App**

  Used mainly by organisational users to access communities, manage communication, and perform administrative tasks.

- **Matrix Homeserver (Tuwunel-based)**

  Acts as the core communication backend. It handles message routing, room management, storage of events, and enforcement of access control via power levels.

- **ePost Services**

  Provide additional application-layer functionality such as user matching (via email, mobile number, or postal address), identity handling, and orchestration between frontend clients and the Matrix backend.

### **Roles of Components**

- **Clients (Mobile & Web Apps)**

  - User interface for communication

  - Encryption/decryption of content (with server-supported key retrieval in current setup)

  - Display of messages, system events, and room data

- **Matrix Homeserver**

  - Storage of messages, events, and metadata

  - Room and membership management

  - Enforcement of permissions (power levels)

  - Transport of encrypted and unencrypted communication data

- **ePost Services**

  - Matching mechanism to identify recipients before communication starts

  - Identity and account management

  - Integration layer for future extensions

### **Persistence, Logging, Monitoring, Key Handling**

- **Persistence**

  - Messages, events, and metadata are stored on the Matrix homeserver

  - Additional user and system-related data may be stored within ePost Services

- **Key Management**

  - Encryption keys are generated on the client side

  - In the current implementation, keys are securely stored server-side and retrieved during login to enable message decryption

- **Logging & Monitoring**

  <a href="https://axonivy.atlassian.net/wiki/people/712020:c3358a78-48bb-4869-a798-cf99511aed4f?ref=confluence" class="confluence-userlink user-mention" data-account-id="712020:c3358a78-48bb-4869-a798-cf99511aed4f" target="_blank" data-base-url="https://axonivy.atlassian.net/wiki">Jiyan Akgül</a>

### **Differences Between Clients**

- **Mobile App vs. Web App**

  - Functionality is largely aligned

  - Minor differences may exist in UI/UX and feature availability

  - Both clients rely on the same backend architecture and data flows

### **Future / Planned Components**

- **Third-party integrations** (planned)

  - External systems (e.g. KLIBnet, document repositories, automation tools)

  - May introduce additional data flows and processing layers

  - Not part of the current productive scope

## **6. Data categories and technical data**

ePost Communities processes different categories of personal and technical data. The current implementation is based on the standard Matrix protocol. No additional custom business-specific data fields are currently defined.

### **Processed data categories**

**Master data**

Basic account and organisational data required for platform operation, such as user ID, display name, organisation affiliation, role, and account status.

**Contact data**

Identifiers of communication partners and memberships, such as Matrix user IDs, room memberships, and participant assignments.

**Message content**

The actual communication content entered by users, in particular text messages.

**Attachments / documents**

Files exchanged in conversations, such as PDFs, images, and office documents.

**Task and appointment related data**

Not currently applicable, as no such functionality exists in the present scope.

**Status messages**

Technical communication states, such as sent, delivered, or read status, where supported.

**Technical metadata**

Contextual data required for transmission, synchronisation, and display, such as sender ID, recipient ID, room ID, event ID, timestamp, file name, file size, and message type.

**Log data**

Operational and security-related records, such as login attempts, failed logins, server events, error logs, and administrative actions.

### **Summary**

In the current state, ePost Communities processes communication content, communication metadata, account-related master data, and technical operational data required for secure operation. Since the solution currently uses the standard Matrix protocol without custom extensions, the classification is based on standard Matrix structures. Task and appointment-related data is currently out of scope.

## **7. Storage, display and local data handling**

**PLEASE ADD BY DEVELOPERS**

**To be completed by engineering and architecture** Please describe, per component, which data is stored, displayed or held locally:

- ePost App

- ePost Web App

- ePost Services

- Matrix Server

- database

- file storage

- logs

- backups

- monitoring

### Also clarify for app and web

- Which data is stored locally on the device or in the browser?

  - **iOS:**

    - iOS Keychain:  
      Stores access token, refresh token, device ID, and database encryption passphrase inside `ePostMatrixRestorationToken`.

    - UserDefaults:  
      Stores lightweight session information such as login status and `userId`.

    - SQLite Databases:

      - `matrix-sdk-crypto.sqlite3` → E2EE keys and device trust information

      - `matrix-sdk-state.sqlite3` → Room and membership state

      - `matrix-sdk-event-cache.sqlite3` → Timeline events and cached messages

    - Media Storage:  
      Downloaded images, avatars, videos, and attachments are cached locally for offline access and faster loading.

  - **Android**

    - Matrix-rust-sdk:

      - maintains a local sqlite DB of all the data received from the matrix server including, messages and their decryption keys.

    - ePost App:

      - Session data: UserId, homeserver, access token, refresh token. Stored Encrypted using Keystore

      - cache of all the Media files downloaded from matrix home-server in the app's private cache directory. This cache may be cleared by O.S if the device is low on storage.

- Are tokens stored locally?

  - **iOS:**

    - Yes. Tokens are securely stored in the standard iOS Keychain using the `KeychainAccess` library. The application does not directly use the Secure Enclave. Database encryption keys are generated using Apple `CryptoKit` and securely stored in the Keychain alongside token data.

  - **Android**

  - Yes. Stored in Session Data in the app’s private directory and encrypted using Android Keystore

- Is there caching or temporary download storage?

  - **iOS:**

    - Yes. The SDK implements multi-layer caching:

      - Database caching for room state, messages, and synchronization data

      - Memory cache (~50MB) for faster UI rendering

      - Disk cache (~500MB–1GB) for media storage using Kingfisher

      - Media is stored inside:

      - Kingfisher cache directories

      - Session-specific cache directories when applicable

  - **Android**

    - Yes.

      - Disk Cache: Messages and rooms are cached in an Sqlite DB in the app’s private directory. Downloaded media in app’s cache directory.

      - Memory Cache: Room and its events when its opened except media

- Which data remains locally after logout or session end?

  - **iOS:**

    - No sensitive data remains after logout.

      During logout, the SDK:

      - Removes tokens from Keychain

      - Deletes session databases

      - Clears cache directories

      - Removes media caches

      - Clears session-related UserDefaults entries

      This ensures no orphaned files, tokens, encryption keys, or sensitive session data remain on the device after logout.

  - **Android**

    - All data is cleared on logout.

**Expected output**

- Per-component overview of storage, display and local data handling

## **8. Interfaces to External Partner Systems**

**Current state**

At present, no productive interface with external partner systems is implemented. Accordingly, no regular automated exchange of personal data with external partner systems takes place at this time.

**Planned target state**

For future integration scenarios, external partner systems may be connected to ePost Communities through one of two general models:

1.  **Connection to the ePost Communities homeserver**

    In this model, the external partner uses the ePost Communities homeserver and backend services, while providing its own user interface or embedding the communication functionality into a third-party application.

2.  **Federated connection between homeservers**

    In this model, the external partner operates its own Matrix-compatible homeserver, which communicates with the ePost Communities homeserver through federation in accordance with the Matrix protocol.

In both models, the target architecture is to ensure that data exchange follows the Matrix standard as far as possible. Any deviations from the standard protocol should be limited to cases where this is technically or functionally necessary.

A core functional principle of ePost Communities is that the initiation of communication is based on a credential-controlled matching mechanism. A conversation can only be initiated where the sender possesses a valid credential for the intended recipient. Unrestricted directory search, open browsing of users, rooms, or comparable communication objects is not envisaged. This is intended to limit discoverability and reduce unnecessary exposure of personal data.

To the extent that future integrations require additional functions beyond the current Matrix baseline, for example structured task-related data, appointment-related data, or comparable interaction formats, such functions should, wherever possible, be designed in a standardized and reusable manner. The objective is to avoid isolated partner-specific solutions and to promote interoperable extensions that may also be used in future partner scenarios.

From a privacy and governance perspective, the specific configuration of each partner integration must be assessed separately before implementation. This includes, in particular, the definition of:

- the categories of personal data to be exchanged,

- the purpose and legal basis of the processing,

- whether transmission is manual, semi-automated, or automated,

- whether and to what extent data is written back to the partner system,

- whether external repositories or third-party storage systems are connected,

- and which elements form part of the minimum viable product versus later implementation phases.

**Data protection assessment**

Since no concrete productive interface currently exists, this section describes only the intended architectural target picture. A detailed privacy assessment, including the precise data flows, roles and responsibilities, and technical and organisational safeguards, must be carried out individually for each future partner integration prior to productive use.

## **9. Encryption and Key Management**

**PLEASE REVIEW**

*ePost Communities uses transport encryption and end-to-end encryption to protect communication data against unauthorised access during transmission and storage. TLS is used to secure communication channels between clients and servers as well as, where applicable, between federated servers. In addition, end-to-end encryption is used to protect message content and attachments at application level.*

*Key management includes both client-side cryptographic processes and server-side support functions. Key material required for end-to-end encrypted communication is generated within the Matrix-based cryptographic architecture. In addition, a server-side backup solution is implemented to enable recovery of encrypted communication data independently of a specific end user device. For this purpose, backup key material is stored server-side.*

*Accordingly, key material is not exclusively bound to a single device. Server-side handling is limited to the storage and provision of backup-related key material within the defined recovery process. The exact technical and organisational controls governing access to such key material, including administrator access restrictions and internal authorisation concepts, must be documented separately in the security architecture.*

*Attachments are protected within the same encryption concept as message content, meaning that end-to-end encryption also applies to files exchanged within the communication context. <span class="inline-comment-marker" ref="7012b101-c0cd-440d-982a-155b60531720">The SQLite store is to be understood as a local technical storage component used by the client or application context for cryptographic and protocol-related data required for secure operation.</span>*

*In summary, the protection concept relies on layered security: TLS protects transport paths, while end-to-end encryption protects content at payload level. Because server-side backup and recovery mechanisms are implemented, special attention must be paid to the technical design, protection, and governance of backup key handling.*

## **10. Metadata, Visibility and Retention**

ePost Communities processes metadata as part of its Matrix-based communication service. This includes metadata that is visible to users in the communication context, metadata processed by ePost for technical and security purposes, and technical operational data processed in supporting systems.

As a general rule, the client applications display only such metadata as is necessary for the use of the service. In particular, **sender ID** and **recipient ID** in the form of Matrix user IDs are visible to communication participants. **Room or channel IDs** may be visible in room settings or comparable technical views. **Timestamps** are displayed for messages, files, images, and system events. Where files are shared, **file names** and, depending on the client, **file sizes** may also be visible. **Delivery and read status** may be shown in simplified form, for example by status icons.

By contrast, **device information**, **IP addresses**, and **login or failed login data** are not normally visible to end users. Such data may be processed and logged on the server side where necessary for secure operation, access control, troubleshooting, abuse prevention, and incident handling.

The visibility of **roles** depends on the room context. Elevated roles such as moderator or administrator may be visible to participants where this follows from the room structure and permissions model. A separate structured field for **organisation assignment** is not currently implemented. Rooms are created by Matrix users and are not formally bound to an organisation as a mandatory system attribute. **Routing data** is not currently defined as a separate functional category; where such data exists, it is treated as technical operational data.

With regard to retention, communication-related metadata should generally be retained in line with the lifecycle of the relevant message, attachment, or room. Security-related metadata, such as login events and IP-related records, should be retained only for as long as necessary for security, traceability, and incident response. Purely technical logs should be retained only for as long as needed for system operation and troubleshooting, subject to defined deletion periods.

## **Metadata table with visibility, logging and retention**

<div>

|  |  |  |  |  |  |
|----|----|----|----|----|----|
| **Data type** | **Visible to ePost** | **Visible to users** | **<span class="inline-comment-marker" ref="80ea06e8-0720-4e92-a5bf-eaf0e23207e4">Logged</span>** | **Retention period** | **Comments** |
| Sender ID (Matrix user ID) | Yes | Yes | Yes | In line with message / room lifecycle; log retention separately defined | Visible to communication participants as part of the communication context. |
| Recipient ID (Matrix user ID) | Yes | Yes | Yes | In line with message / room lifecycle; log retention separately defined | Visible to participants in the relevant room or conversation. |
| Room / channel ID | Yes | Yes, where exposed in room settings or technical views | Yes | In line with room lifecycle; log retention separately defined | Room identifier may be visible in settings or similar client views. |
| Timestamp | Yes | Yes | Yes | In line with message / event lifecycle; log retention separately defined | Displayed for messages, files, images, and system events. |
| File name | Yes | Yes | Yes | In line with attachment lifecycle; log retention separately defined | Visible where attachments are shared in the conversation. |
| File size | Yes | Yes, where shown by client | No / limited | In line with attachment lifecycle, if stored as message-related metadata | Visibility depends on client functionality. Logging should be limited to technical necessity. |
| Delivery status | Yes | Yes | Yes | In line with message lifecycle; log retention separately defined | May be shown through status icons such as delivered / read indicators. |
| Device information | Yes, where technically processed | No | Yes | According to security / operational log retention rules | Not intended for display to end users. Processed only where technically necessary. |
| IP address | Yes, where technically processed | No | Yes | According to security / operational log retention rules | Used for technical operation, security, abuse prevention, and troubleshooting. |
| Logins / failed logins | Yes | No | Yes | According to security log retention rules | Security-relevant metadata; not visible to normal users. |
| Organisation assignment | No dedicated structured field currently | No | No | Not applicable in current state | Not currently implemented as a separate technical metadata field. |
| Roles | Yes | Yes, in part | Yes | In line with room / membership lifecycle; log retention separately defined | Elevated roles such as moderator or administrator may be visible in the room context. |
| Routing data | Potentially yes, as technical operational data | No | Yes, where technically processed | According to technical log retention rules | Not currently defined as a separate functional metadata category; treat as technical operational data where applicable. |

</div>

### **Interpretation notes**

**Visible to ePost** means that the platform operator or its technical systems may process the data in order to provide, secure, or administer the service.

**Visible to users** means visible within the client application to end users in the normal communication context.

**Logged** means that the data may be contained in technical, security, or operational logs. For several categories, the exact scope of logging still needs to be confirmed in the logging concept.

**Retention period** is not yet finally defined in absolute terms in this section. At this stage, the table reflects the logic that:

- communication-related metadata should normally follow the lifecycle of the related message, attachment, or room;

- security-related metadata should follow the defined security log retention period;

- purely technical operational data should follow the technical log retention period.

## **11. Changes to Messages and Content**

Within ePost Communities, users are currently able to modify certain communication content after it has been sent. This applies in particular to the editing of their own messages. Where a message is edited, this is visibly indicated to conversation participants in the client by an “edited” marker or equivalent status display. From a transparency perspective, content changes are therefore not made silently at user interface level.

Users are also able to delete their own messages and files they have sent. In addition, users with administrative permissions may delete messages of other users where such action is permitted under the applicable room permissions or moderation model. Where content is deleted, the conversation currently does not remove the event without trace; instead, a visible indication is shown in the conversation that the message has been deleted. This supports transparency for participants regarding the fact that content previously existed and has subsequently been removed.

From a privacy, accountability, and audit perspective, the visible client behaviour must be distinguished from the underlying technical handling of content changes. At present, it is confirmed that edits are visibly marked and deletions are visibly indicated. However, essential technical questions remain open and require clarification before a final assessment can be made. In particular, it is currently not yet confirmed whether the original version of an edited message remains technically stored, whether deleted content remains recoverable in productive systems, backups, or logs, and whether edit and deletion events are logged in a manner that allows later traceability.

For this reason, the current privacy concept should explicitly distinguish between the confirmed current functionality and the still unresolved points:

**Current confirmed state**

- Users may edit their own messages.

- Edited messages are visibly marked in the client.

- Users may delete their own messages and files they have sent.

- Administrators may delete messages of other users in accordance with the applicable permission model.

- Deleted content is visibly indicated in the conversation.

**<span class="inline-comment-marker" ref="3806dbc0-9446-42c6-aba9-7b9cc6b0eac5">Open points requiring technical clarification</span>**

- Whether the original version of an edited message remains technically stored or can be reconstructed.

- Whether deleted messages or deleted attachments remain available in server-side storage, backups, or logs for any period.

- Which edit, delete, and administrative intervention events are logged.

- Whether such events are later traceable.

- Whether any of these records are designed to be immutable, evidence-relevant, or audit-proof.

Until these points have been clarified with engineering, security, and operations, no final statement should be made that changes to content are fully traceable, reversible, or audit-proof. The section should therefore reflect that the visible handling of edits and deletions is known, while the technical depth of traceability and retention remains subject to further analysis.

**Data protection assessment**

The ability to edit and delete communication content is relevant for transparency, user expectations, accountability, and retention management. Visible indication of edits and deletions supports transparency towards communication participants. At the same time, the technical treatment of prior versions, deletion traces, and related log records must be clearly defined in order to assess compliance with the principles of data minimisation, storage limitation, and accountability. These aspects should therefore be finalised before the section is marked as decided in the privacy concept.

## **12. Logging, traceability and audit-proofness**

Certainly — here is a formal **privacy concept / DPIA-style version in English** for **Section 13** based on the assumptions clarified so far and with the open points explicitly marked.

## **13. Retention, Deletion and History Logic**

**Current state**

At present, ePost Communities follows the standard Matrix protocol baseline with no dedicated product-specific retention or deletion concept implemented beyond the protocol and current technical configuration.

Based on the current implementation status, messages and attachments are, as a rule, retained for an indefinite period unless they are actively removed through user or administrative actions. No general automatic retention limit for communication content has been defined at this stage.

Where a message is deleted, the deletion is displayed in the conversation by means of a visible deletion marker. The conversation history as such remains available to the other participants in accordance with the existing room history and event model. Deletion of an individual message therefore does not result in deletion of the full conversation history for other participants.

At present, accounts are not understood to be fully deleted in the ordinary course of operation, but rather deactivated or otherwise technically retained in line with the Matrix-based system architecture. Where an account is suspended or deactivated, the underlying data is currently retained. No final concept has yet been defined for the case of a full tenant deletion or a full account deletion including the related data handling consequences.

**Current assessment by data category**

- **Messages:** retained indefinitely unless actively deleted.

- **Attachments / files:** retained indefinitely unless actively deleted.

- **Metadata:** currently assumed to remain available in accordance with the underlying Matrix event and system architecture; no separate deletion period has yet been defined.

- **Security logs / audit logs:** retention periods remain to be defined separately.

- **Backups:** retention periods and deletion logic remain to be defined separately.

**History logic in lifecycle events**

From the current understanding, the following principles apply:

- **Deletion of a message**

  If a user deletes a message, the deletion is represented visibly within the conversation. The surrounding conversation history remains intact for other participants.

- **Deletion of an attachment**

  If an attachment is deleted, this must be assessed in line with the same functional logic as message deletion. The exact technical effects on stored file objects, references, previews, caches and backups should be clarified separately.

- **Account suspension or deactivation**

  If an account is suspended or deactivated, the relevant data is currently retained. No final deletion concept has yet been defined for such lifecycle events.

- **Employee leaving an organisation / role change / similar lifecycle events**

  No final rule has yet been defined in this section. These cases require separate specification, in particular regarding access withdrawal, continuity of room history, and organisational governance.

- **Full tenant deletion / full account deletion**

  This is currently an open point and must be clarified explicitly. In particular, it must be defined whether and to what extent communication history, metadata, backups, and technical records are removed, anonymised, blocked, or retained for legal, security or evidentiary purposes.

**Data protection assessment**

From a privacy perspective, indefinite retention of communication content and related metadata constitutes a relevant topic and requires explicit justification and governance. In particular, a final privacy concept should define:

- retention periods for messages, attachments, metadata, logs and backups,

- deletion triggers and responsibilities,

- the effect of deletion on visible conversation history,

- the handling of deactivated, suspended and fully deleted accounts,

- the treatment of tenant termination and end-of-contract scenarios,

- and whether deleted content remains recoverable in logs, backups or other technical stores.

At present, the implemented logic can be described only as a preliminary current-state model. A final retention and deletion concept remains to be completed.

**Open points / follow-up items**

The following points remain open and should be clarified in the final privacy concept:

1.  Whether metadata is retained indefinitely or subject to separate retention limits.

2.  Retention periods for security logs, audit logs and backups.

3.  The exact technical effect of deleting attachments, including storage objects, cached copies and backups.

4.  The handling of full account deletion and full tenant deletion.

5.  The rules applicable where an employee leaves an organisation or where organisational access changes.

6.  Whether deletion actions are traceable later and, if so, in which systems and for how long.

## **14. Responsibility Model**

For the purposes of this privacy concept, ePost Communities applies a uniform responsibility model for communication on the platform. From a technical perspective, the platform does not differentiate at protocol level between communication scenarios that may be described in legal or business terms as B2B, B2C, or C2C. In all cases, communication takes place between Matrix users based on the same underlying technical architecture, the same communication protocol, and the same core product functions.

ePost Communities acts as the provider and operator of the platform and is responsible for the operation of the technical infrastructure and for those processing activities that are necessary to provide, secure, maintain, and govern the service. This includes, in particular, user and access management at platform level, system and transport security, availability of backend services, implementation of product guardrails, and processing of technical and operational data required for secure platform operation.

Responsibility for the substantive content of communications does not lie with ePost Communities, but with the communicating parties themselves. This applies irrespective of whether the communicating parties are organisations, private individuals, or a combination of both. ePost Communities does not define, verify, or assume responsibility for the lawfulness, accuracy, or appropriateness of the content exchanged between users, except to the extent required by applicable law or by product-level governance measures.

This separation of responsibilities is reinforced by the technical design of the platform. Where communication content is protected by end-to-end encryption, ePost Communities is generally not able to access such content in plain text during regular operation. As a result, the role of ePost Communities is limited to platform operation and related technical processing, whereas responsibility for communication content remains with the communicating users or, where applicable, the organisations on whose behalf such users act.

Accordingly, the same responsibility model applies across all communication relationships on the platform: ePost Communities is responsible for platform operation and security; the communicating parties are responsible for the content and context of the communication. Any internal communication rules, organisational policies, or room-specific usage requirements that apply within a particular organisational context remain the responsibility of the relevant organisation, unless such rules are explicitly defined and enforced by ePost Communities as platform-wide requirements.

**Open point / future assessment**

If future product developments introduce differentiated organisational roles, moderation functions, workflow-based processing, delegated access, or partner-specific business logic, the allocation of responsibilities may need to be specified in greater detail for those scenarios. At present, however, no such differentiation is reflected in the core protocol-based communication model.

## **15. Content restrictions and usage rules**

Currently, no content is restricted from the web or mobile side. There is also no automatic or manual validation process in place to detect or verify whether unacceptable content has been uploaded or exchanged.

At this stage, no dedicated content policy has been defined beyond the existing Terms and Conditions. This means there is currently no explicit product-level no-go list for specific content types, data types, special category data, or highly sensitive personal data, even where content is exchanged using E2E encryption.

**Current answer / status:**

- No product-specific content restrictions are currently implemented.

- No automatic content validation or moderation is currently implemented.

- No manual content review process is currently implemented.

- No dedicated content policy or no-go content list has been defined yet.

- No additional product restrictions beyond the Terms and Conditions are currently documented.

- No specific rules for special category or highly sensitive personal data have been defined from a product perspective.

## **16. Protection needs and risk-relevant topics**

Based on a high protection need context and utilizing a **Matrix protocol** architecture deployed via a **Tuwunel server** (a high-performance, enterprise-ready Rust homeserver and successor to *conduwuit*), the architecture inherently faces unique decentralization, state-synchronization, and cryptography challenges.

Below is the structured, prioritized assessment tailored for your Data Protection Impact Assessment (DPIA) and Privacy-by-Design concept.

### Prioritised List of Risk-Relevant Topics for Privacy Concept and DPIA

### 1. Technical Risks (Priority 1)

Because Matrix is a decentralized, eventually consistent state machine that replicates data across federated homeservers, technical structural failures pose the highest threat to confidentiality and system integrity.

- **Federated Key Management & Exchange Vulnerabilities:** While Tuwunel supports End-to-End Encryption (E2EE) using the Megolm/Olm protocols, a technical compromise in key exchange mechanisms or a vulnerability in client-side cryptographic implementations could result in the massive exposure of highly protected data.

- **Homeserver Federation Data Leakage:** When users join a room spanning multiple homeservers, their communication data is replicated to external infrastructure. If a federated server has weak security parameters, the data is exposed outside the core organization's boundary.

- **Rust Memory/Concurrency Exploits at Scale:** Although Rust drastically minimizes memory corruption issues, Tuwunel handles asynchronous event loops via extensive multi-threading. High-concurrency race conditions or database engine (e.g., RocksDB/Sled) corruptions could lead to unauthorized data exposure or denial of service (DoS).

- **TLS/Network-Level Interception:** Weak cipher suites or failure to enforce strict TLS 1.3/post-quantum hybrid key agreements (e.g., X25519MLKEM768) could allow sophisticated actors to harvest encrypted payloads for future decryption.

### 2. Risks from Metadata Processing (Priority 2)

Even with E2EE active, a high protection need system is heavily vulnerable to metadata analysis, which Matrix inherently generates in large quantities.

- **Exposed Communication Topology (Traffic Analysis):** Tuwunel maps room histories via unencrypted "State Events." This includes unencrypted metadata detailing **who** talks to whom, **when**, and **from which IP addresses**, allowing adversaries to map internal organizational structures and high-profile targets.

- **Push Notification/UnifiedPush Disclosures:** Delivering notifications to mobile devices via third-party services (Apple APNs or Google FCM) can leak room IDs, sender tokens, or unencrypted message snippets unless dedicated private notification architectures (e.g., local `ntfy` servers) are strictly enforced.

- **User Presence and Device Tracking:** Matrix sync logs capture active user sessions, device footprints, operating systems, and location-correlated IP addresses. If retained indefinitely, this compromises user physical privacy.

### 3. Risks from Edits, Deletions, and Weak Traceability (Priority 3)

The conflict resolution design of the Matrix protocol conflicts heavily with traditional data deletion rights (e.g., GDPR Art. 17 Right to Be Forgotten).

- **Impossibility of Absolute Erasure:** When a user deletes a message, a "Redaction Event" is broadcast. However, federated external homeservers are not technically forced to honor this redaction, leading to residual copies of high-protection data persisting indefinitely outside of local control.

- **Immutable State History (Room Graph):** Membership changes, bans, and administrative adjustments are recorded as permanent, cryptographically chained state events. These cannot be altered or deleted without breaking the entire room structure.

- **Weak Auditing vs. Privacy Paradox:** While deep logs are required to investigate internal data leaks (Traceability), maintaining verbose application logs in Tuwunel inevitably captures unencrypted user identifiers and operational metadata, creating an intrinsic privacy risk.

### 4. Risks from Roles and Administration (Priority 4)

Tuwunel structures control via Room Power Levels and Server Admin privileges, introducing severe insider-threat surfaces.

- **Over-Privileged Server Administrators:** A Tuwunel server administrator can access the underlying database directly. While they cannot read E2EE message payloads without private keys, they can forcefully inject themselves into private rooms, alter room visibility settings, or extract the entirety of the system's metadata.

- **Power Level Escalation & Social Engineering:** Misconfigured room power levels can allow a compromised or malicious internal user to escalate privileges, invite unauthorized external entities into highly protected rooms, or redact audit logs within a chat room.

- **Dehydrated Device Key Risks:** Tuwunel supports "Dehydrated Devices" (allowing users to receive encrypted messages while offline). If an administrator compromises the server-side storage containing these encrypted key backups, brute-force attacks on the backup passphrase become viable.

### 5. Risks from Future Interfaces (Priority 5)

As the system scales to include new collaboration vectors, the attack surface expands horizontally.

- **Third-Party Bridges (Function Creep & Protocol Downgrade):** Interfacing Tuwunel with legacy or alternative platforms (e.g., WhatsApp, Signal, or Microsoft Teams via Matrix bridges) requires decryption/re-encryption at the bridge gateway. This completely breaks E2EE integrity, making the bridge host a high-value target for interception.

- **MatrixRTC and LiveKit Integrations (VoIP/Video Privacy):** Introducing real-time voice/video features (via Element Call/MatrixRTC hubs) introduces WebRTC signaling risks. Misconfigured STUN/TURN servers (like `eturnal`) can leak real-world user IP addresses to external peers.

- **Widget API and Bot Exploits:** Integrating external web applications (e.g., NeoBoard whiteboards, task managers) via the Matrix Widget API risks Cross-Site Scripting (XSS) or arbitrary data exfiltration if the sandboxing environment of the Matrix client is bypassed.

## 17. Marking of future topics

### 🟢 Current State

These topics are existing specification and fully supported by Communites. They require configuration, not development.

- **Team Mailboxes:** Implemented by creating shared, persistent functional rooms. Users are assigned equal administrative rights to monitor and reply.

- **Role Change:** Handled dynamically within rooms via **Roles (Moderator, Admin and User)**. Permissions to invite, or change role data migration.

- **Case Handover:** When a new employee takes over a case, they are invited to the relevant Matrix room. The new employee instantly gains access to the complete chronological history of the case.

- **Message Editing:** Fully integrated into Communities. The Tuwunel server tracks the event chain, while compliant clients render the updated text while preserving an edit history flag for audit purposes.

### 🟡 Planned (Roadmap Dependencies / Extensions Required)

These topics are technically feasible and standard implementations exist, but they require architectural definitions, client-side configuration and adequate prioritisation.

- **Vacation / Absence Handling:** Basic presence states exist, but true automated out-of-office handling requires a dedicated application service (bot). Because messages are E2EE, the bot must be invited to the room or utilize a secure server-side key backup to read incoming pings and send automated replies.

- **Export / Handover at Contract End:** Exporting standard data is supported by appclient only (not on Webclient), but structured compliance exports for high-protection data require server-side auditing tools. Because Tuwunel cannot decrypt E2EE payloads natively, a cryptographic auditing bridge must be deployed to capture and store unencrypted logs securely for compliance handovers.

- **Employee Leaving the Organisation:** Can be automated via standard lifecycle management. Tuwunel interfaces with your central IAM/IdP via OpenID Connect (OIDC). Disabling the user in the IdP immediately invalidates active Matrix tokens.

### 🔴 Open (Awaiting Strategic Alignment & Custom Engineering)

These topics represent fundamental conflicts between the decentralized, encrypted architecture of Matrix and traditional corporate IT compliance requirements. They require dedicated project resources and policy decisions.

- **Delegation / Substitute Access:** \* *Architecture Approach:* In a standard system, an administrator can grant access to another user's inbox. In an E2EE Tuwunel environment, the server *cannot* pass a user's private cryptographic keys to a substitute. To solve this, a proxy client structure or specific cryptographic key-forwarding mechanisms (such as MSC3069) must be integrated and approved.

- **Account Deletion:** \* *Architecture Approach:* Purging a user from the local Tuwunel database is straightforward. However, if that user participated in federated rooms with external partners, their messages, cryptographic signatures, and user ID metadata will remain cached on external homeservers. Achieving absolute erasure across a federated graph is a known architectural challenge that requires strict room-level retention policies.

- **Third-Party Systems / Document Repositories:** Integration is natively achieved via the Matrix Widget API (embedding secure frames into clients) or application services/bots acting as middleware between Tuwunel and your internal document management systems.

%% ai-graph-start %%

**Related notes:**
- [[E2EE covers message content only; metadata and server-held keys narrow it further]]
- [[ePost AI Solution Concept]]
- [[Architecture]]
- [[iLetter current backend architecture]]
- [[MessageV2 Field Encryption Approach]]

%% ai-graph-end %%