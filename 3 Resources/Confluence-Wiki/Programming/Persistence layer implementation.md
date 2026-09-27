---
ai_hash: 841d5f5a7c71c1f3
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 2
depth: 3
entities: []
relevance: 0.792
source: https://axonivy.atlassian.net/wiki/spaces/LUZCOMP/pages/20675756658/Persistence+layer+implementation
space: LUZCOMP
status: reference
tags:
- confluence
- programming
- space/luzcomp
title: Persistence layer implementation
topic: programming
type: source
updated: 2016-06-06
---

# Persistence layer implementation

> [!info] Imported from Confluence
> Space **LUZCOMP** · updated 2016-06-06 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZCOMP/pages/20675756658/Persistence+layer+implementation)
> Relevance 0.792 · topic `programming`

1.  Project structure  
    

![[20675756658-error.png]]



2.  Create datasource in Wildfly
    1.  Add a datasource in standalone.xml of Wildfly server

        <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="b4c62b17-f1e3-4d8a-9749-367fb1383a86" macro-name="code" style="border-width: 1px;">

        <div class="codeHeader panelHeader pdl" style="border-bottom-width: 1px;">

        **standalone.xml**

        </div>

        <div class="codeContent panelContent pdl">

        ``` syntaxhighlighter-pre
        <datasource jta="true" jndi-name="java:jboss/datasource/luz_person" pool-name="luz_person" enabled="true" use-java-context="true" spy="true">
            <connection-url>jdbc:postgresql://104.199.155.66:5432/luz_person</connection-url>
            <driver-class>org.postgresql.Driver</driver-class>
            <driver>postgresql</driver>
            <security>
                <user-name>admin</user-name>
                <password>admin</password>
            </security>
            <validation>
                <check-valid-connection-sql>SELECT 1</check-valid-connection-sql>
            </validation>
        </datasource>
        ```

        </div>

        </div>

    2.  Add a driver postgresql we use for datasource, this code block is under datasource we added above

        <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="0e8973cc-ff03-44dd-a661-ffbe46430348" macro-name="code" style="border-width: 1px;">

        <div class="codeHeader panelHeader pdl" style="border-bottom-width: 1px;">

        **driver**

        </div>

        <div class="codeContent panelContent pdl">

        ``` syntaxhighlighter-pre
        <drivers>
            <driver name="postgresql" module="org.postgresql">
                <xa-datasource-class>org.postgresql.xa.PGXADataSource</xa-datasource-class>
            </driver>
        </drivers>
        ```

        </div>

        </div>

    3.  Copy postgres driver library postgresql-9.3-1104-jdbc41.jar  to Wildfly server at path {WILDFLY_HOME}\modules\system\layers\base\org\postgresql and with module.xml to initialize this driver

        <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="777d0097-b63f-4563-be23-883931770c50" macro-name="code" style="border-width: 1px;">

        <div class="codeHeader panelHeader pdl" style="border-bottom-width: 1px;">

        **module.xml**

        </div>

        <div class="codeContent panelContent pdl">

        ``` syntaxhighlighter-pre
        <?xml version="1.0" encoding="UTF-8"?>
        <module xmlns="urn:jboss:module:1.1" name="org.postgresql">
            <resources>
                <resource-root path="postgresql-9.3-1104-jdbc41.jar" />
            </resources>
            <dependencies>
                <module name="javax.api" />
                <module name="javax.transaction.api" />
            </dependencies>
        </module>
        ```

        </div>

        </div>

3.  Add persistence.xml file
    1.  Add a persistence.xml to path /src/main/resources/META-INF

        <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="c14df231-4e47-4c58-82a8-0c7dd4cfaea2" macro-name="code" style="border-width: 1px;">

        <div class="codeHeader panelHeader pdl" style="border-bottom-width: 1px;">

        **persistence.xml**

        </div>

        <div class="codeContent panelContent pdl">

        ``` syntaxhighlighter-pre
        <?xml version="1.0" encoding="UTF-8"?>
        <persistence version="2.1"
            xmlns="http://xmlns.jcp.org/xml/ns/persistence" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/persistence http://xmlns.jcp.org/xml/ns/persistence/persistence_2_1.xsd">
            <persistence-unit name="luzPersonPU" transaction-type="JTA">
                <jta-data-source>java:jboss/datasource/luz_person</jta-data-source>
                <exclude-unlisted-classes>false</exclude-unlisted-classes>
                <properties>
                    <property name="javax.persistence.schema-generation.database.action" value="drop-and-create"/>
                    <property name="hibernate.show_sql" value="true" />
                </properties>
            </persistence-unit>
        </persistence>
        ```

        </div>

        </div>

4.  Add annotation for entities  
    1.  We should create a class BaseEntity to store some common fields for all our entities

        <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="05f8df7b-fb7f-42e7-b52a-e171ec3f6b6d" macro-name="code" style="border-width: 1px;">

        <div class="codeHeader panelHeader pdl" style="border-bottom-width: 1px;">

        **BaseEntity**

        </div>

        <div class="codeContent panelContent pdl">

        ``` syntaxhighlighter-pre
        @MappedSuperclass
        @EntityListeners(AuditingEntityListener.class)
        public class BaseEntity implements Serializable {
            private static final long serialVersionUID = 7636259301305238530L;
            @Id
            @GeneratedValue(strategy = GenerationType.IDENTITY)
            @Column(name = "id")
            protected Long id;
            @Column(name = "create_date")
            @Temporal(TemporalType.TIMESTAMP)
            private Date createDate;
            @Column(name = "create_by", length = 100)
            private String createBy;
            @Column(name = "update_date")
            @Temporal(TemporalType.TIMESTAMP)
            private Date updateDate;
            @Column(name = "update_by", length = 100)
            private String updateBy;
        ...
        ```

        </div>

        </div>

    2.  Update annotation for entities that extends from BaseEntity

        <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="24f83483-c8e4-458c-a319-68ce51dde693" macro-name="code" style="border-width: 1px;">

        <div class="codeContent panelContent pdl">

        ``` syntaxhighlighter-pre
        @Entity
        @Table(name = "company")
        public class CompanyEntity extends BaseEntity {
            private static final long serialVersionUID = 1L;
            @NotNull(message = "Company name cannot be null")
            @Column(name = "name")
            private String name;
            @OneToMany(cascade={CascadeType.ALL})
            @JoinTable(
                name="company_phone",
                joinColumns={ @JoinColumn(name="company_id", referencedColumnName="id") },
                inverseJoinColumns={ @JoinColumn(name="phone_id", referencedColumnName="id", unique=true, nullable=true) }
            )
            private List<PhoneEntity> phones;
            @OneToMany(cascade={CascadeType.ALL})
            @JoinTable(
                name="company_address",
                joinColumns={ @JoinColumn(name="company_id", referencedColumnName="id") },
                inverseJoinColumns={ @JoinColumn(name="address_id", referencedColumnName="id", unique=true, nullable=true) }
            )
            private List<AddressEntity> addresses;
        ...
        ```

        </div>

        </div>

5.  Add audit-trail information
    1.  We user a listener for handling audit-trail info 

        <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="721c6ada-84e7-4b94-958f-4a46650c71ea" macro-name="code" style="border-width: 1px;">

        <div class="codeHeader panelHeader pdl" style="border-bottom-width: 1px;">

        **BaseEntity**

        </div>

        <div class="codeContent panelContent pdl">

        ``` syntaxhighlighter-pre
        @EntityListeners(AuditingEntityListener.class)
        public class BaseEntity implements Serializable {
        ```

        </div>

        </div>

        <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="31de9b50-9163-45b3-948f-248cbc42fd17" macro-name="code" style="border-width: 1px;">

        <div class="codeHeader panelHeader pdl" style="border-bottom-width: 1px;">

        **AuditingEntityListener**

        </div>

        <div class="codeContent panelContent pdl">

        ``` syntaxhighlighter-pre
        @Stateless
        public class AuditingEntityListener {
            private final Logger LOGGER = Logger.getLogger(AuditingEntityListener.class.getName());
            @PrePersist
            public void onPrePersist(BaseEntity entity) {
            entity.setCreateBy(getPrincipalName(entity));
            entity.setCreateDate(new Date());
            LOGGER.info("PERSIST");
            }
            @PreUpdate
            public void onPreUpdate(BaseEntity entity) {
            entity.setUpdateBy(getPrincipalName(entity));
            entity.setUpdateDate(new Date());
            LOGGER.info("UPDATE");
            }
            @PreRemove
            public void onPreRemove(BaseEntity entity) {
            LOGGER.info("REMOVE");
            }
            public String getPrincipalName(BaseEntity entity) {
                String principalName = Constants.PRINCIPAL_DEFAULT_NAME;
                if (entity != null) {
                    // TODO
                }
                return principalName;
            }
        }
        ```

        </div>

        </div>

6.  Create DAO 
    1.  Create an abstract BaseDao

        <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="bd0d8e40-61ad-48ba-897a-3d522fc347fc" macro-name="code" style="border-width: 1px;">

        <div class="codeContent panelContent pdl">

        ``` syntaxhighlighter-pre
        @Stateless
        public class BaseDao<T extends BaseEntity> {
            @PersistenceContext(unitName = Constants.LUZ_PERSON_PERSISTENCE_UNIT_NAME)
            private EntityManager entityManager;
            private Class<T> persistentClass;
            @Resource
            private SessionContext sessionContext;

            public T persist(T entity) {
                entityManager.persist(entity);
                return entity;
            }

            public T merge(T entity) {
                if (entity != null && entity.getId() != null) {
                    T existed = find(entity.getId());
                    if (existed != null) {
                            entity.setCreateBy(existed.getCreateBy());
                            entity.setCreateDate(existed.getCreateDate());
                        }
                }
                return entityManager.merge(entity);
            }
         
            public void remove(Long id) {
                entityManager.remove(find(id));
            }
        ....
        ```

        </div>

        </div>

    2.  The other DAO have to extend from abstract BaseDao

        <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="a56c645f-bb88-459d-806a-897fa33edd6d" macro-name="code" style="border-width: 1px;">

        <div class="codeContent panelContent pdl">

        ``` syntaxhighlighter-pre
        @Stateless
        public class CompanyDao extends BaseDao<CompanyEntity> {
        }
        ```

        </div>

        </div>

7.  Add Flyway for updating SQL database version  
    1.  Update pom.xml

        <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="e61255e3-1fb8-4587-9200-d0c3a0357847" macro-name="code" style="border-width: 1px;">

        <div class="codeHeader panelHeader pdl" style="border-bottom-width: 1px;">

        **pom.xml**

        </div>

        <div class="codeContent panelContent pdl">

        ``` syntaxhighlighter-pre
        <dependency>
            <groupId>com.googlecode.flyway</groupId>
            <artifactId>flyway-core</artifactId>
            <version>${flyway.version}</version>
        </dependency>
        ```

        </div>

        </div>

    2.  Create a class to handle flyway 

        <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="c9e55ea4-8c16-47e9-9320-2600eeea6477" macro-name="code" style="border-width: 1px;">

        <div class="codeHeader panelHeader pdl" style="border-bottom-width: 1px;">

        **PersonDbMigrationService.class**

        </div>

        <div class="codeContent panelContent pdl">

        ``` syntaxhighlighter-pre
        @Singleton
        @Startup
        @TransactionManagement(TransactionManagementType.BEAN)
        public class PersonDbMigrationService {
            private final Logger LOGGER = Logger.getLogger(PersonDbMigrationService.class.getName());
            @Resource(lookup = Constants.LUZ_PERSON_DATA_SOURCE)
            private DataSource dataSource;

            @PostConstruct
            private void onStartup() {
            if (dataSource == null) {
                LOGGER.severe("no datasource found to execute the db migrations!");
                throw new EJBException("no datasource found to execute the db migrations!");
            }
            Flyway flyway = new Flyway();
            flyway.setInitOnMigrate(true);
            flyway.setDataSource(dataSource);
            for (MigrationInfo i : flyway.info().all()) {
                LOGGER.info("migrate task: " + i.getVersion() + " : " + i.getDescription() + " from file: " + i.getScript());
            }
            flyway.migrate();
            }
        }
        ```

        </div>

        </div>

    3.  Add scripts to path src\main\resources\db\migration\your_script.sql as example format below:

        <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="216ffc3f-6e25-4fd2-87ee-d4c73275bee5" macro-name="code" style="border-width: 1px;">

        <div class="codeContent panelContent pdl">

        ``` syntaxhighlighter-pre
        V20160524120000__Update_database.sql
        ```

        </div>

        </div>

8.  Update Dockerfile to deploy to Wildfly

    <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="2799f4c5-44cb-494a-8be7-964b440ff4fc" macro-name="code" style="border-width: 1px;">

    <div class="codeContent panelContent pdl">

    ``` syntaxhighlighter-pre
    FROM jboss/wildfly
    ADD target/*.war /opt/jboss/wildfly/standalone/deployments/
    ADD src/deployment/wildfly/configuration/*.xml /opt/jboss/wildfly/standalone/configuration/
    ADD src/deployment/wildfly/dbdriver/ /opt/jboss/wildfly/modules/system/layers/base/org/
    RUN /opt/jboss/wildfly/bin/add-user.sh admin Admin
    EXPOSE 8080 9990
    CMD ["/opt/jboss/wildfly/bin/standalone.sh", "-c", "standalone_luz_person.xml", "-b", "0.0.0.0", "-bmanagement", "0.0.0.0"]
    ```

    </div>

    </div>

%% ai-graph-start %%

**Related notes:**
- [[How to implement an audit log using Hibernate Envers]]
- [[ORM - DBFlow guidelines]]
- [[API models libraries for reducing duplicated code and increasing the maintainability of our JEE]]
- [[Hibernate Envers generates audit tables from an annotation]]
- [[Reporting - Java class configuration]]

%% ai-graph-end %%