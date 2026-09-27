---
title: "Efficient way to write data parallelly into both Postgres and MongoDB"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/HACKA/pages/48430710817/Efficient+way+to+write+data+parallelly+into+both+Postgres+and+MongoDB
space: "HACKA"
topic: infra
relevance: 0.731
depth: 2.38
updated: 2025-04-10
attachments: 34
tags:
  - confluence
  - infra
  - space/hacka
---

# Efficient way to write data parallelly into both Postgres and MongoDB

> [!info] Imported from Confluence
> Space **HACKA** · updated 2025-04-10 · [open original](https://axonivy.atlassian.net/wiki/spaces/HACKA/pages/48430710817/Efficient+way+to+write+data+parallelly+into+both+Postgres+and+MongoDB)
> Relevance 0.731 · topic `infra`

#### **Current Situation:**

- The Eletter system currently only supports saving data to a **PostgreSQL database**. However, there is a new requirement to **simultaneously save data to MongoDB** alongside PostgreSQL.

- The solution is also viable for later only using MongoDB for processing and no longer using Postgres

- Ticket: <a href="https://axonivy.atlassian.net/browse/LUZ-131213" class="external-link" rel="nofollow">[LUZ-131213] Research: How to write data parallelly into Postgres and MongoDB - Jira</a>

**Solutions**: In this article we have three solutions and the **ONE_API service** will apply one of them, **ONE_API** is a group of services. In this article, I will use **luz_eletter** as an example

As discussed with Future team on 03 April 2025 , we agreed on the **third solution**.

- Adding MongoDBDao Implementation to an Existing PostgreSQL DAO

- Creating a new Module for Storing Data in MongoDB

- Creating a New MongoDBService to Interact with the Mongo Database (new → fix for Solution 1) (**Accepted**)

**Concern**: about entity ID is that MongoDB does not have mechanism to manage IDENTITY IDs → Hacka team will investigate this

**Currently implementation:** Eletter services are using Dao Class to interact with Postgres Database only


![[48430710817-solution 1 current implementation.png]]



# 1. Adding MongoDBDao Implementation to an Existing PostgreSQL DAO

**Situation** : Currently, our service uses only one DAO for a single database, which is not extendable when adding another database.  
**Solution**: we will apply Dependency Injection Pattern and following the standard DAO pattern to separate processing logic between databases. Just make changes in Dao layer (data access object)

### **1.1 Hight level:**


![[48430710817-Solution 1 hight level.png]]



**1.2 How to implement**  
We will apply “Dependency injection” and following the standard DAO pattern to separate processing logic between databases to deal with this. Just make changes in Dao layer (data access object)  
  
In this design, `EletterDao` is just a common name for DAO classes, in real life, it can be `DeliveryDao`, `DocumentDao`, `RecipientTrackingDao`, etc.


![[48430710817-New Implementation.png]]




![[48430710817-star_blue.png]]

 Service will use an Interface to interact with database (the primary instance of implementation will be decided by our setting, in this article it’s `ParrallelEltterDao`)


![[48430710817-star_blue.png]]

 `IBaseDao` is an interface that defines some common actions for interacting with a database (such as `insert`, `delete`, `update`, `findById`, etc.).


![[48430710817-star_blue.png]]

 Create three classes that implement `EletterDao`: one for PostgreSQL, one for MongoDB, and one for a parallel store (which writes data to both PostgreSQL and MongoDB).

These classes will extend 

![[48430710817-star_blue.png]]

`BaseDao<Entity>` to reuse common method implementations (`insert`, `delete`, `update`, `findById`, etc.) and use `EntityManager` and `MongoDbManager` to interact with the databases.

<span style="background-color: rgb(253,208,236);">1ST </span> `ParrallelDao` is a class that uses `MongoEletterDao` and `PostgresEletterDao` to write to the database in parallel.

➡️ Make the `ParrallelDao` class the primary bean. This means that when the service injects the `EletterDao` interface, the application will inject `ParrallelDao` by default

- **Pseudo Code**


![[48430710817-image-20250328-025841.png]]



**1.3 Remove Postgres**

After completely migrating to MongoDB, we can set `MongoDao` as the primary bean by changing our bean configuration, which means the service will use this class instance to interact with the database


![[48430710817-Solution1_completely remove postgres.png]]



### **Advantages:**

- Easily extendable.

- Separates logic between databases, only changes at the DAO layer, and does not affect the service layer.

### **Disadvantages:**

- The MongoDB implementation is still depend on the entity Postgres.

# 2. Creating a New Module for Storing Data in MongoDB

**Solution**: We will create separate classes for MongoDB and add them to the current Postgres DAO.

**2.1 How to implement**


![[48430710817-Solution 2 new implementation.png]]



**Advantages:** Separation of logic for storing data into MongoDB.

**Disadvantages:**

- Dependent on the state of the broker.

- Requires the creation of an additional service.

- Does not ensure rollback when either PostgreSQL or MongoDB fails.

# 3. Creating a New MongoDBService to Interact with the Mongo Database (new → fix for Solution 1) (**Accepted**)

Solution: Creating a New MongoDBService to Interact with the Mongo Database

**3.1 Hight Level**


![[48430710817-solution 3 hight level.png]]



**3.2 How to implement**


![[48430710817-Solution_3 new implement.png]]



**3.3 Remove Postgres**


![[48430710817-solution_3 remove postgres.png]]



**Advantages**: The MongoDB implementation is decoupled from the entity Postgres.

**Disadvantages**: Requires changes in multiple places.
