---
ai_hash: edfb716f66b36a7f
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 6
depth: 3
entities: []
relevance: 0.886
source: https://axonivy.atlassian.net/wiki/spaces/LUZFIN/pages/20964547120/How+to+implement+an+audit+log+using+Hibernate+Envers
space: LUZFIN
status: reference
tags:
- confluence
- programming
- space/luzfin
title: How to implement an audit log using Hibernate Envers
topic: programming
type: source
updated: 2018-09-17
---

# How to implement an audit log using Hibernate Envers

> [!info] Imported from Confluence
> Space **LUZFIN** · updated 2018-09-17 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZFIN/pages/20964547120/How+to+implement+an+audit+log+using+Hibernate+Envers)
> Relevance 0.886 · topic `programming`

1.  

2.  ### Project Setup

    Because Hibernate Envers is packaged as a separate dependency, if you want to use it, you need to declare the following Maven dependency in your project:

    <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="7b9da0e3-42e1-4bc9-9c90-419178e6f321" macro-name="code" style="border-width: 1px;">

    <div class="codeContent panelContent pdl">

    ``` syntaxhighlighter-pre
    <dependency>
              <groupId>org.hibernate</groupId>
              <artifactId>hibernate-envers</artifactId>
              <version>${hibernate.version}</version>
    </dependency>
    ```

    </div>

    </div>

    **Note**: Wildfly 10 has built-in **hibernate-envers-5.0.7.Final.jar **(under this location: {wilfly-folder}\modules\system\layers\base\org\hibernate\main)

3.  ### Audit An Entity

    Now, after adding the hibernate-envers dependency, you need to instruct Hibernate which entities should be audited, and this can be done via the @Audited annotation.  
    The @Audited annotation either on an @Entity (to audit the whole entity) or on specific @Columns (if you need to audit specific properties only)  
    For example we config to audit the whole entity:

    <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="09f6f098-70ed-4b46-9ac0-17536ab49bb0" macro-name="code" style="border-width: 1px;">

    <div class="codeContent panelContent pdl">

    ``` syntaxhighlighter-pre
    @Setter
    @Getter
    @Entity
    @Table(name = "booking_header")
    @Audited
    @AuditOverride( forClass = BaseEntity.class, isAudited = true )
    public class BookingHeaderEntity extends BaseEntity {

    private static final long serialVersionUID = 1L;

    @OneToMany(cascade = { CascadeType.PERSIST, CascadeType.REMOVE, CascadeType.MERGE }, mappedBy = "bookingHeader", fetch = FetchType.LAZY)
    @OrderBy("seq ASC")
    @AuditMappedBy(mappedBy="bookingHeader")
    private Set<BookingDetailEntity> bookingDetails;

    @Column(name = "company_id", nullable = false)
    private long companyId;

    @Column(name = "document_date")
    private LocalDate documentDate;

    @Column(name = "booking_date", nullable = false)
    private LocalDate bookingDate;
    ```

    </div>

    </div>

4.  ### Creating Audit Log Tables 

    1.  Revinfo

        The Revinfo table stores the revision number and its epoch timestamp while the audit table stores the entity snapshot at a particular revision.The following code snippet shows an example of an **audit table as default**:

        <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="c58f0ee6-64ab-4626-9ca8-5377354552cc" macro-name="code" style="border-width: 1px;">

        <div class="codeContent panelContent pdl">

        ``` syntaxhighlighter-pre
        CREATE TABLE revinfo
        (
        rev bigserial not null,
        revtstmp bigint,
        primary key (rev)
        );
        ```

        </div>

        </div>

        **Note in Postgres**: If the type of primary key is **bigserial, the DBMS itself automatically create sequence for this primary key.** Otherwise, you must **define yourself a sequence (hibernate_sequence)** to support automatically generate id.

        If you want **more customization** in the Revinfo table, you can find out it here (section 15.5): <a href="https://docs.jboss.org/hibernate/devguide/en-US/html/ch15.html" class="external-link" rel="nofollow">https://docs.jboss.org/hibernate/devguide/en-US/html/ch15.html</a>

    2.  An audit table for each entity  
        You also need to create an audit table for each entity you want to audit. Each audit table contains the primary key of the original entity, all audited fields, the revision number and the revision type. The revision number has to match a record in the revision table and is used together with the id column to create a combined primary key. The revision type persists the type of operation that was performed on the entity in the given revision. The following code snippet shows an example of an audit table:

        <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="2ee45880-58d3-439f-af2f-766898baca90" macro-name="code" style="border-width: 1px;">

        <div class="codeContent panelContent pdl">

        ``` syntaxhighlighter-pre
        CREATE TABLE booking_header_history
        (
        id bigserial NOT NULL,
        create_by varchar(100),
        create_date timestamp,
        update_by varchar(100),
        update_date timestamp,
        booking_date date NOT NULL,
        booking_status varchar(255),
        business_case varchar(255),
        company_id int8 NOT NULL,
        document_date date,
        document_id varchar(255),
        internal_comment varchar(255),
        business_case_id varchar(1024),
        related_booking_header_links varchar(1024),
        order_management_invoice_link varchar(255),
        rev integer not null,
        revtype smallint,
        revend integer,
        PRIMARY KEY (id, rev)
        );

        /* These two ALTER statement below can be embedded into the CREATE TABLE */

        alter table booking_header_history
        add constraint FK_BookingHeaderVersionRev
        foreign key (REV)
        references REVINFO;

        alter table booking_header_history
        add constraint FK_BookingHeaderVersionRevend
        foreign key (REVEND)
        references REVINFO;
        ```

        </div>

        </div>

5.  ### Configuring Envers Properties 

    You can configure Envers properties look like any other Hibernate property in persistence.xml  
    For example, we change the audit table suffix (which defaults to “\_aud“) to “\_histoty“, switch from the `DefaultAuditStrategy` to `ValidityAuditStrategy and config ``the entity data be stored in the revision when the entity is deleted``.`Here is how to set the value of the corresponding properties:

    <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="e663b6a2-541b-49b1-a81e-545760f55389" macro-name="code" style="border-width: 1px;">

    <div class="codeContent panelContent pdl">

    ``` syntaxhighlighter-pre
    <?xml version="1.0" encoding="UTF-8" standalone="yes"?>
    <persistence xmlns="http://xmlns.jcp.org/xml/ns/persistence"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/persistence http://xmlns.jcp.org/xml/ns/persistence/persistence_2_1.xsd"
    version="2.1">
    <persistence-unit name="luzAccountingPU" transaction-type="JTA">
    <jta-data-source>java:jboss/datasources/luz_accounting</jta-data-source>
    <exclude-unlisted-classes>false</exclude-unlisted-classes>
    <properties>
      <property name="org.hibernate.envers.audit_strategy"
         value="org.hibernate.envers.strategy.ValidityAuditStrategy" />
      <property name="org.hibernate.envers.audit_table_suffix" value="_history" />
      <property name="org.hibernate.envers.store_data_at_delete" value="true"/>
    </properties>
    </persistence-unit>
    </persistence>
    ```

    </div>

    </div>

    **Note**: You can reference the list of configurations in here: **<a href="https://docs.jboss.org/envers/docs/" class="external-link" rel="nofollow">https://docs.jboss.org/envers/docs/</a>**

%% ai-graph-start %%

**Related notes:**
- [[Hibernate Envers generates audit tables from an annotation]]
- [[Persistence layer implementation]]
- [[ORM - DBFlow guidelines]]
- [[Hibernate Envers on luz_store SubscriptionEntity is field-scoped and omits price_plan]]
- [[How to resolve hibernate N+1 select's problem]]

%% ai-graph-end %%