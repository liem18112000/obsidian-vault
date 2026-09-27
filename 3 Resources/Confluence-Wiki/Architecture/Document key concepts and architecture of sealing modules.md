---
ai_hash: c5790993e96cd90d
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 4
depth: 3
entities: []
relevance: 0.864
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/49116020740/Document+key+concepts+and+architecture+of+sealing+modules
space: LUZ
status: reference
tags:
- confluence
- architecture
- space/luz
title: Document key concepts and architecture of sealing modules
topic: architecture
type: source
updated: 2026-02-04
---

# Document key concepts and architecture of sealing modules

> [!info] Imported from Confluence
> Space **LUZ** · updated 2026-02-04 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/49116020740/Document+key+concepts+and+architecture+of+sealing+modules)
> Relevance 0.864 · topic `architecture`

<span class="confluence-embedded-file-wrapper conf-macro output-inline" hasbody="false" macro-id="7a1c6601-d0e8-4fe5-abad-96e3794f2a9d" macro-name="view-file"><a href="../_attachments/49116020740-INSTALL.md" class="confluence-embedded-file" data-nice-type="Text File" data-file-src="/wiki/download/attachments/49116020740/INSTALL.md?version=1&amp;modificationDate=1770190621916&amp;cacheVersion=1&amp;api=v2" data-mime-type="text/plain" data-has-thumbnail="true">

![[49116020740-INSTALL.md]]

</a></span><span class="confluence-embedded-file-wrapper conf-macro output-inline" hasbody="false" macro-id="c30b1e20-631c-453e-abcc-e4c789c168a5" macro-name="view-file"><a href="../_attachments/49116020740-CONTRIBUTING.md" class="confluence-embedded-file" data-nice-type="Text File" data-file-src="/wiki/download/attachments/49116020740/CONTRIBUTING.md?version=1&amp;modificationDate=1770190621698&amp;cacheVersion=1&amp;api=v2" data-mime-type="text/plain" data-has-thumbnail="true">

![[49116020740-CONTRIBUTING.md]]

</a></span><span class="confluence-embedded-file-wrapper conf-macro output-inline" hasbody="false" macro-id="b96ff8d8-fd3b-4cf5-bf20-0040ceffece0" macro-name="view-file"><a href="../_attachments/49116020740-BUSINESS-DOMAIN.md" class="confluence-embedded-file" data-nice-type="Text File" data-file-src="/wiki/download/attachments/49116020740/BUSINESS-DOMAIN.md?version=1&amp;modificationDate=1770190621735&amp;cacheVersion=1&amp;api=v2" data-mime-type="text/plain" data-has-thumbnail="true">

![[49116020740-BUSINESS-DOMAIN.md]]

</a></span><span class="confluence-embedded-file-wrapper conf-macro output-inline" hasbody="false" macro-id="e79db593-2eab-4658-b6ac-3523f3794432" macro-name="view-file"><a href="../_attachments/49116020740-ARCHITECTURE.md" class="confluence-embedded-file" data-nice-type="Text File" data-file-src="/wiki/download/attachments/49116020740/ARCHITECTURE.md?version=2&amp;modificationDate=1770197443425&amp;cacheVersion=1&amp;api=v2" data-mime-type="text/plain" data-has-thumbnail="true">

![[49116020740-ARCHITECTURE.md]]

</a></span>

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="31e3ef83-6c44-41ae-ace6-d29fec010eaf" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

```` syntaxhighlighter-pre
# ECP Auditing POC - Quick Start Guide

This document provides a quick overview and navigation to detailed documentation.

## 📖 Documentation

The complete documentation has been organized into focused guides for better readability:

### Core Documentation
- **[ARCHITECTURE.md](ARCHITECTURE.md)** - System design, components, and data flows
- **[INSTALL.md](INSTALL.md)** - Installation, configuration, and quick start
- **[CONTRIBUTING.md](CONTRIBUTING.md)** - Development conventions and contributor guidelines
- **[ROADMAP.md](ROADMAP.md)** - Known limitations, technical debt, and future plans

### Business Context
- **[BUSINESS-DOMAIN.md](BUSINESS-DOMAIN.md)** - Problem domain and use cases

### Testing
- **[tryout/TRYOUT.md](tryout/TRYOUT.md)** - Complete end-to-end testing guide with examples

---

## ⚡ Quick Start (5 minutes)

### 1. Prerequisites
- Java 21
- Maven 3.9+
- Node.js 18+
- Docker

### 2. Build & Run (in this order)

**Terminal 1:**
```bash
cd luz-crypto-utils
mvn clean install
```

**Terminal 2:**
```bash
cd luz-crypto
./mvnw quarkus:dev
```

**Terminal 3:**
```bash
cd luz-immutable-log
./mvnw quarkus:dev
```

**Terminal 4:**
```bash
cd luz_ecp_log
./mvnw quarkus:dev
```

**Terminal 5:**
```bash
cd demo-client
npm install
npm run dev
```

### 3. Verify
```bash
curl http://localhost:3000
```

---

## 📚 What Each Guide Contains

| Guide | Purpose |
|-------|---------|
| **ARCHITECTURE** | Understand the system design, components, responsibilities, and data flows |
| **INSTALL** | Set up, configure, and start all services |
| **CONTRIBUTING** | Learn development conventions, best practices, and extension points |
| **ROADMAP** | Check known issues, limitations, and planned improvements |
| **BUSINESS-DOMAIN** | Understand the problem domain and use cases |
| **TRYOUT** | Run a complete end-to-end workflow with test data |

---

## 🎯 First Steps

1. **New to the project?** → Start with [BUSINESS-DOMAIN.md](BUSINESS-DOMAIN.md)
2. **Want to understand the architecture?** → Read [ARCHITECTURE.md](ARCHITECTURE.md)
3. **Ready to set up locally?** → Follow [INSTALL.md](INSTALL.md)
4. **Planning to contribute?** → Review [CONTRIBUTING.md](CONTRIBUTING.md)
5. **Want to test the system?** → See [tryout/TRYOUT.md](tryout/TRYOUT.md)
6. **Checking limitations?** → Review [ROADMAP.md](ROADMAP.md)

---

## 🔗 Project Links

- **Repository**: [ecp-auditing-poc](.)
- **Architecture Diagram**: See [ARCHITECTURE.md](ARCHITECTURE.md#high-level-architecture)
- **API Documentation**: See [ARCHITECTURE.md](ARCHITECTURE.md#key-execution-flows)
- **Testing Guide**: [tryout/TRYOUT.md](tryout/TRYOUT.md)

---

**For detailed information, please refer to the specific documentation files above.**
````

</div>

</div>

%% ai-graph-start %%

**Related notes:**
- [[Architecture]]
- [[LUZ Audit - Basic Understanding Guide]]
- [[Architecture Overview LUZ]]
- [[LUZ Audit Refactor- 2025-2026]]
- [[Vault overview]]

%% ai-graph-end %%