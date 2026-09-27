---
title: "System Architecture & Overview - Training Guide"
created: 2025-10-08
updated: 2025-10-22
type: source
status: reference
source: "Confluence · LUZ - LUZ"
url: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/48732667920/System+Architecture+Overview+-+Training+Guide
confluence_id: "48732667920"
confluence_path: "LUZ Home > Product Documentation > LUZ Developer Guide > Cookbook > Training hub > New Developer Training Program - Landing Page"
tags: [confluence]
---

# System Architecture & Overview - Training Guide

*Confluence source · LUZ Home › Product Documentation › LUZ Developer Guide › Cookbook › Training hub › New Developer Training Program - Landing Page · [view original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/48732667920/System+Architecture+Overview+-+Training+Guide) · updated 2025-10-22*

### New Developer Reference Guide

#### Architecture Principles

Our system is built on the following core principles that guide all architectural decisions:

##### 1. **Separation of Concerns**

- Clear boundaries between business logic, data access, and presentation layers

- Each component has a single, well-defined responsibility

- Minimal coupling between different system components

- Service-oriented architecture with microservices approach

##### 2. **Scalability & Performance**

- Horizontal scaling capabilities using Google Cloud Platform

- Efficient resource utilization with Cloud Run and GKE

- Caching strategies at multiple levels (Redis, in-memory)

- Asynchronous processing where appropriate

##### 3. **Security by Design**

- Authentication and authorization at every layer

- Data encryption in transit and at rest using Cloud KMS

- Input validation and sanitization

- Principle of least privilege with IAM policies

##### 4. **Maintainability**

- Clean, readable, and well-documented code

- Consistent coding standards and patterns

- Comprehensive testing strategy (unit, integration, E2E)

- Modular and extensible design

------------------------------------------------------------------------

### System Overview

#### High-Level Architecture

Our system follows a modern microservices architecture deployed on Google Cloud Platform:

![[high_level_architecture_overview.png]]

------------------------------------------------------------------------

### Technology Stack

Based on our Technology Radar and current implementations:

#### **Frontend Technologies**

- **Framework:**

  - Next.js (React-based)

  - Axon Ivy Engine

- **Build Tools:** to be fulfilled

- **Testing:** to be fulfilled

#### **Backend Technologies**

- **Programming languages:** Typescript, Java

- **Specifications:** MicroProfile, JPA

- **Application server:** wildfly

- **Framework:** Quarkus

- **API Style:** RESTful APIs

- **Documentation:** OpenAPI/Swagger specifications

#### **Database & Storage**

- **Primary Database:**

  - Postgres 9.5. But we are migrating to Google AlloyDB (PostgreSQL)

  - Mongodb Atlas

  - Mongodb Percona

- **Caching:** Google Memorystore (Redis)

- **File Storage:** Cloud Storage - **Status: Adopt**

------------------------------------------------------------------------

### Multi-tenancy

- Postgresql: one tenant per schema. But we are changing one tenant per database

- Mongodb:

  - for documents: one tenant per database, follow Encryption at Rest, each db has its own key.

  - for processing data: one database for all

## Attachments

*Attached to the Confluence page but not embedded in its body.*

- [[3 Resources/Confluence/LUZ/Product Documentation/LUZ Developer Guide/Cookbook/attachments/system-architecture-overview-training-guide/high_level_architecture_overview|high_level_architecture_overview]]
