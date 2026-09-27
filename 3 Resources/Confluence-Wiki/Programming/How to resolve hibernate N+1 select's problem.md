---
title: "How to resolve hibernate N+1 select's problem"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/TS/pages/31137694409/How+to+resolve+hibernate+N+1+select+s+problem
space: "TS"
topic: programming
relevance: 0.812
depth: 3
updated: 2020-01-03
attachments: 6
tags:
  - confluence
  - programming
  - space/ts
---

# How to resolve hibernate N+1 select's problem

> [!info] Imported from Confluence
> Space **TS** · updated 2020-01-03 · [open original](https://axonivy.atlassian.net/wiki/spaces/TS/pages/31137694409/How+to+resolve+hibernate+N+1+select+s+problem)
> Relevance 0.812 · topic `programming`

# **What is N+1 problem?**

The N+1 problem is the classic performance issue in ORM (Object Relational Mapping). It happens when the first query populates the primary object and the second query populates all the child objects for each of the unique primary objects returned.

For example:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="4af86c21-ef18-411b-84d3-cb0256c71bd0" macro-name="code" style="border-width: 1px;">

<div class="codeHeader panelHeader pdl" style="border-bottom-width: 1px;">

**CalendarEntry**

</div>

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
@Entity
@Table(name = "calendar_entry")
public class CalendarEntry{
    
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @OneToOne
    private BookingDetail bookingDetail;

}
```

</div>

</div>

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="f5676d9d-0e7b-4111-99c6-87d940c88a12" macro-name="code" style="border-width: 1px;">

<div class="codeHeader panelHeader pdl" style="border-bottom-width: 1px;">

**BookingDetail**

</div>

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
@Entity
@Table(name = "booking_detail")
public class BookingDetail{

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    @ManyToOne
    private Booking booking;
    
}
```

</div>

</div>

  

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="fed5cb1b-54d4-45ed-96ab-4c93ac243b75" macro-name="code" style="border-width: 1px;">

<div class="codeHeader panelHeader pdl" style="border-bottom-width: 1px;">

**Booking**

</div>

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
@Entity
public class Booking{

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

}
```

</div>

</div>

  

Now, we want to fetch all **CalendarEntry** from this table and print **BookingDetail** for each one. Very native Object Relational implementation could be.

First, Get All **CalendarEntry**:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="04faf521-c5c0-416a-a7d5-44f51cbfc0c7" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
SELECT c FROM CalendarEntry c
```

</div>

</div>

<span class="auto-cursor-target">  
Then Get **BookingDetail** for each **CalendarEntry**:</span>

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="dd003ab8-b782-48bf-b300-f605359dbc23" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
 SELECT c.bookingDetail FROM CalendarEntry c WHERE c.id := id
```

</div>

</div>

  

<span class="auto-cursor-target">So we need one select for **CalendarEntry** and N additional selects for fetching **BookingDetail** for each **CalendarEntry**, where N is total number of **CalendarEntry**.</span>

# **<span class="auto-cursor-target">How identify it?</span>**

### **<span class="auto-cursor-target">I. Enabling SQL logging</span>**

**<span class="auto-cursor-target"> </span>**<span class="auto-cursor-target">We enable log SQL statement in Postgres 10. Using logs you can easily see if hibernate is issuing N+1 queries for a given call. For more details. You can find it in <a href="https://axonivy.atlassian.net/wiki/display/TS/How+to+enable+log+SQL+statement+in+Postgres+10" rel="nofollow">this confluence page</a>.</span>

### **II. Typical N+1 SQL logs printed in logs**

**

![[31137694409-sql-log.png]]

**

If you see multiple entries for SQL for a given select query, then there are high chances that its due to N+1 problem.

### **III. Measure performance**

We use VisualVM tool to measure performance. And this is the record when we call the API through postman.


![[31137694409-oct-postman.png]]



# **Resolve N+1 SELECTs problem**

Hibernate provide mechanism to solve the N+1 issue. What ORM needs to archive to avoid N+1 is find a query that joins the two tables and get the combined results in single query.

- ## **Solution 1: Join fetch**

we apply hibernate approach to use **JOIN FETCH **SQL to retrieve everything.

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="14c45719-8daa-4ecc-85df-ecdef94e4adf" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
SELECT ce FROM CalendarEntry ce JOIN FETCH ce.bookingDetail bd JOIN FETCH bd.booking bo WHERE ce.date BETWEEN :start AND :end
```

</div>

</div>

It seems to enhance the performance after we fixed this issue.

Now, Let check again the logging and the result performance:

### **SQL logs:**

**

![[31137694409-sql-log-fetch-join.png]]

**

### **Measure performance:**


![[31137694409-oct-postman-fetch-join.png]]



## **Solution 2: Entity graph**

The definition and usage of the entity graph is query independent and results in only one select statement. 

The JPA specification introduces with version 2.1 so called NamedEntityGraphs. This annotation lets us describe the graph a JPQL query should load in more detail than a JOIN FETCH clause can do and therewith is another solution to the N+1 problem. The following example demonstrates a NamedEntityGraph for our *Resource* entity that is supposed to load only the *workDays* of the *Resource*. The *workDays* are described in the subgraph workDays-subgraph in more detail. Here we see that we only want to load shifts of the workDay.

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="b4f332fc-b8c1-4647-9271-87bef8d2dd4b" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
@Entity
@NamedEntityGraph(
    name = "graph.Resource.workDays", 
    attributeNodes = @NamedAttributeNode(value = "workDays", subgraph = "graph.WorkDay.shifts"),
    subgraphs = {
        @NamedSubgraph(name = "graph.WorkDay.shifts", attributeNodes = { @NamedAttributeNode("shifts")} ),
    }
)
public class Resource {
    
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    @Column(name = "id")
    private Long id;
    
    @OneToMany(cascade = CascadeType.ALL, orphanRemoval = true, fetch = FetchType.EAGER, mappedBy = "resource")
    private List<WorkDay> workDays;
}

@Entity
public class WorkDay{

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    @Column(name = "id")
    private Long id;
    
    @OneToMany(cascade = CascadeType.ALL, orphanRemoval = true, fetch = FetchType.EAGER)
    @JoinColumn(name = "workday_id")
    private List<Shift> shifts;
}
```

</div>

</div>

*@NamedEntityGraph: <span class="legacy-color-text-default">It defines a unique name and a list of attributes (the </span>attributeNodes<span class="legacy-color-text-default">) that shall be loaded.</span>*

*@NamedSubgraph: The definition of a named subgraph is similar to the definition of @NamedEntityGraph and can be referenced as an attributeNode. As above example, the defined entity graph will fetch an Resource with all Workdays and their Shifts.*

  

The NamedEntityGraph is given as a hint to the JPQL query, after it has been loaded via EntityManager using its name:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="4c19486a-4da0-4143-9263-73423e180c7f" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
public List<Resource> findAll() {
        TypedQuery<Resource> query =  em.createNamedQuery(Resource.FIND_ALL, Resource.class).setHint("javax.persistence.fetchgraph", em.getEntityGraph("graph.Resource.workDays"));
        return query.getResultList();
}
```

</div>

</div>

  

### **SQL logs:**


![[31137694409-image2020-1-3_12-0-55.png]]



### **Measure performance:**

**

![[31137694409-image2020-1-3_12-2-33.png]]

**

## ****Conclusion**:** 

We have seen that with JPA 2.1 we have two solutions for the N+1 problem: We can either use the FETCH JOIN clause to eagerly fetch a @OneToMany relation, which results in an inner join, or we can use @NamedEntityGraph feature that lets us specify which @OneToMany relation to load via left outer join.

  

**Reference:**

**<a href="https://martinsdeveloperworld.wordpress.com/2014/07/02/using-namedentitygraph-to-load-jpa-entities-more-selectively-in-n1-scenarios/" class="external-link" rel="nofollow">Using @NamedEntityGraph to load JPA entities more selectively in N+1 scenarios</a>**

**<a href="https://thoughts-on-java.org/jpa-21-entity-graph-part-1-named-entity/" class="external-link" rel="nofollow">How to Define and Use a @NamedEntityGraph</a>**

**<a href="https://thoughts-on-java.org/hibernate-tip-entitygraph-multiple-subgraphs/" class="external-link" rel="nofollow">Create an EntityGraph with multiple SubGraphs</a>**
