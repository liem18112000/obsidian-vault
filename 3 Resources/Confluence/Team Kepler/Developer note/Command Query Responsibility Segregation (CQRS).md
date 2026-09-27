---
title: "Command Query Responsibility Segregation (CQRS)"
created: 2025-12-05
updated: 2025-12-05
type: source
status: reference
source: "Confluence · TK - Team Kepler"
url: https://axonivy.atlassian.net/wiki/spaces/TK/pages/48938188847/Command+Query+Responsibility+Segregation+CQRS
confluence_id: "48938188847"
confluence_path: "Team Kepler > Developer note"
tags: [confluence]
---

# Command Query Responsibility Segregation (CQRS)

*Confluence source · Team Kepler › Developer note · [view original](https://axonivy.atlassian.net/wiki/spaces/TK/pages/48938188847/Command+Query+Responsibility+Segregation+CQRS) · updated 2025-12-05*

------------------------------------------------------------------------

### What is CQRS?

**Command Query Responsibility Segregation (CQRS)** is an architectural pattern that separates read and write operations into different models. The term was coined by Greg Young and is derived from Bertrand Meyer's **Command Query Separation (CQS)** principle.

#### CQS vs CQRS

|  |  |  |
|----|----|----|
| Aspect | CQS (Command Query Separation) | CQRS |
| Scope | Method level | Architecture level |
| Concept | Methods should either change state OR return data, never both | Separate models for reading and writing |
| Origin | Bertrand Meyer (Eiffel language) | Greg Young |

------------------------------------------------------------------------

### Core Concepts

![[image-20251205-014025.png]]

#### Commands (Write Side)

Commands represent **intentions to change state**. They:

- Are imperative (e.g., `CreateOrder`, `UpdateCustomer`, `DeleteProduct`)

- Modify data

- Should not return data (except success/failure)

- Are validated before execution

- May trigger domain events

#### Queries (Read Side)

Queries represent **requests for data**. They:

- Are interrogative (e.g., `GetOrder`, `FindCustomers`, `ListProducts`)

- Read data only

- Never modify state

- Can be optimized independently

- May use denormalized views

------------------------------------------------------------------------

### Benefits of CQRS

![[image-20251205-014530.png]]

#### **1. Independent Scalability**

- Read and write workloads can be scaled separately

- Read-heavy applications can add more query replicas

- Write-heavy applications can optimize the command path

#### 2. Performance Optimization

- Query side can use denormalized, pre-computed views

- Command side focuses on data integrity and business rules

- Different storage technologies for each side

#### 3. Simplified Models

- Each model handles one responsibility

- Easier to understand and maintain

- Reduced complexity in each part

#### 4. Enhanced Security

- Different authorization rules for reads vs writes

- Command validation and audit logging

- Query filtering based on user permissions

------------------------------------------------------------------------

### When to Use CQRS

#### Good Fit For

- **High read/write disparity** - Many more reads than writes

- **Complex business logic** - Rich domain models

- **Multiple read models** - Different views of same data

- **Collaborative domains** - Multiple users modifying same data

- **Event-driven systems** - Already using events/messaging

- **Performance critical** - Need optimized read/write paths

#### Not Recommended For

- **Simple CRUD** - Basic create/read/update/delete operations

- **Small applications** - Overhead not justified

- **Strong consistency required** - Real-time consistency needs

- **Small team** - Added complexity burden

- **Rapid prototyping** - Slows initial development

------------------------------------------------------------------------

### Typical CQRS Architecture

![[image-20251205-024231.png]]

------------------------------------------------------------------------

### Command Side in Detail

#### Command Flow

![[image-20251205-024333.png]]

------------------------------------------------------------------------

### Query Side in Detail

#### Query Flow

![[image-20251205-024438.png]]

------------------------------------------------------------------------

### Synchronization Strategies

#### 1. Synchronous Synchronization

![[image-20251205-024614.png]]

- Same transaction updates both stores

- Strong consistency

- Higher latency

- Suitable for simple systems

#### 2. Asynchronous Synchronization (Event-Driven)

![[image-20251205-024710.png]]

- Eventual consistency

- Lower latency for writes

- Higher scalability

- Suitable for complex systems

#### 3. Projection-Based Synchronization

![[image-20251205-024830.png]]

------------------------------------------------------------------------

### Consistency Models

#### Strong Consistency - Block

![[image-20251205-025117.png]]

#### Eventual Consistency - Non-block

![[image-20251205-025204.png]]

### Best Practices

#### Command Side

|                           |                                    |
|---------------------------|------------------------------------|
| Practice                  | Description                        |
| **Validate early**        | Fail fast before processing        |
| **Single responsibility** | One intent per command             |
| **Idempotency**           | Handle duplicate commands          |
| **Domain events**         | Communicate changes asynchronously |
| **Audit logging**         | Track all commands                 |

#### Query Side

|                         |                                |
|-------------------------|--------------------------------|
| Practice                | Description                    |
| **Optimize for reads**  | Denormalize data structures    |
| **Use caching**         | Reduce database load           |
| **Handle staleness**    | Inform users of data freshness |
| **Design per use case** | Create specific query models   |
| **Paginate results**    | Don't return unbounded data    |

#### General

|                        |                                        |
|------------------------|----------------------------------------|
| Practice               | Description                            |
| **Keep it simple**     | Don't over-engineer                    |
| **Monitor separately** | Track read/write metrics independently |
| **Plan for failures**  | Handle sync issues gracefully          |
| **Document clearly**   | Maintain architecture documentation    |
| **Start simple**       | Evolve to CQRS when needed             |

### Summary

**Key Takeaways:**

1.  **Separate read and write models** for independent optimization

2.  **Commands change state**, queries read state - never mix

3.  **Choose consistency model** based on requirements

4.  **Use events** for synchronization in distributed systems

5.  **Start simple**, evolve to CQRS when complexity demands it

------------------------------------------------------------------------

### Further Reading

- [Martin Fowler - CQRS](https://martinfowler.com/bliki/CQRS.html)

- [Greg Young - CQRS Documents](https://cqrs.files.wordpress.com/2010/11/cqrs_documents.pdf)

- [Microsoft - CQRS Pattern](https://docs.microsoft.com/en-us/azure/architecture/patterns/cqrs)

- [Udi Dahan - Clarified CQRS](https://udidahan.com/2009/12/09/clarified-cqrs/)

- [Event Sourcing & CQRS](https://www.eventstore.com/cqrs-pattern)

------------------------------------------------------------------------

*Generated: December 2024*
