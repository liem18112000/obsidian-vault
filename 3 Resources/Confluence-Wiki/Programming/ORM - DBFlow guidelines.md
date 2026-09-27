---
ai_hash: bd2b871fcbdc06de
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 0
depth: 3
entities: []
relevance: 0.792
source: https://axonivy.atlassian.net/wiki/spaces/Helios/pages/38139200781/ORM+-+DBFlow+guidelines
space: Helios
status: reference
tags:
- confluence
- programming
- space/helios
title: ORM - DBFlow guidelines
topic: programming
type: source
updated: 2016-01-08
---

# ORM - DBFlow guidelines

> [!info] Imported from Confluence
> Space **Helios** · updated 2016-01-08 · [open original](https://axonivy.atlassian.net/wiki/spaces/Helios/pages/38139200781/ORM+-+DBFlow+guidelines)
> Relevance 0.792 · topic `programming`

Note: This guidelines is based on version 3.0-beta1 of DBFlow

    Reference: https://github.com/Raizlabs/DBFlow

     

<div class="toc-macro client-side-toc-macro conf-macro output-block" cssliststyle="none" hasbody="false" headerelements="H1,H2,H3,H4,H5,H6,H7" macro-id="87ab12b4-d888-4d8a-9962-e5ab71e7104a" macro-name="toc">

</div>

### Including in your project

We need to include the <a href="https://bitbucket.org/hvisser/android-apt" class="external-link" rel="nofollow">apt plugin</a> in our classpath to enable Annotation Processing:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="1ab7ec2f-61c6-483b-aad3-a99b5991ebb2" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
buildscript {
    repositories {
      // required for this library, don't use mavenCentral()
        jcenter()
    }
    dependencies {
        classpath 'com.neenbedankt.gradle.plugins:android-apt:1.8'
    }
}
```

</div>

</div>

Add this maven url to your project.

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="1c059ab9-a869-48d0-86be-ff26bbdabc74" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
allProjects {
  repositories {
    maven { url "https://jitpack.io" }
  }
}
```

</div>

</div>

Add the library to the project-level build.gradle, using the apt plugin to enable Annotation Processing:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="dc792a32-a6dd-4d2b-91e7-966c38a499e1" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
 apply plugin: 'com.neenbedankt.android-apt'

  dependencies {
    apt 'com.github.Raizlabs.DBFlow:dbflow-processor:3.0.0-beta1'
    compile "com.github.Raizlabs.DBFlow:dbflow-core:3.0.0-beta1"
    compile "com.github.Raizlabs.DBFlow:dbflow:3.0.0-beta1"
  }
```

</div>

</div>

<div class="highlight highlight-source-groovy">

### Setting Up DBFlow

To initialize DBFlow, open databases, and begin migrations and creations, place this code in a custom `Application` class:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="475d3ea8-3b71-44e3-bbd1-48361737d52f" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
public class ExampleApplication extends Application {

    @Override
    public void onCreate() {
        super.onCreate();
        FlowManager.init(this);
    }
}
```

</div>

</div>

Lastly, add the definition to the manifest (with the name that you chose for your custom application):

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="6d82b972-90af-4f9b-be7c-1eba7be3c6f4" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
<application
  android:name="{packageName}.ExampleApplication"
  ...>
</application>
```

</div>

</div>

 

<div class="highlight highlight-text-xml">

    Defining our database

In DBFlow, databases are placeholder objects that generate interactions from which tables "connect" themselves to.

We need to define where we store our ant colony:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="e038f4c2-6dfd-4d74-b02f-b91bf46997f3" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
@Database(name = ColonyDatabase.NAME, version = ColonyDatabase.VERSION)
public class ColonyDatabase {

  public static final String NAME = "Colonies";

  public static final int VERSION = 1;
}
```

</div>

</div>

For best practices, we create the constants `NAME` and `VERSION` as public, so other components we define for DBFlow can reference it later (if needed).

Note: Don't include dot '.' character into database's name. Otherwise, it will be failed on processing annotation.

Don't "database.name". Use "databaseName"

### Creating tables

To properly define a table we must: 1. Mark the class with `@Table` annotation 2. Point the database to the correct database, in this case `ColonyDatabase` 3. Define at least one primary key 4. The class and all of its database columns must be package private or `public` 5. so the generated `_Adapter` class can access it. Note: Columns may be private with getter and setters specified.

#### Queen table

The basic definition we can use is:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="cff3a440-5aa9-4208-934a-0f41eeda462d" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
@Table(database = ColonyDatabase.class)
public class Queen extends BaseModel {

  @PrimaryKey(autoincrement = true)
  long id;

  @Column
  String name;

}
```

</div>

</div>

<div class="highlight highlight-source-java">

#### Colony table

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="2fd7a32d-e3dd-4383-8087-07d4f0a83bda" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
@ModelContainer

@Table(database = ColonyDatabase.class)
public class Colony extends BaseModel {

  @PrimaryKey(autoincrement = true)
  long id;

  @Column
  String name;

}
```

</div>

</div>

 

<div class="highlight highlight-source-java">

#### Ant table

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="3150a150-a7aa-43d5-977f-173ab10470e0" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
@Table(database = ColonyDatabase.class)
public class Ant extends BaseModel {

  @PrimaryKey(autoincrement = true)
  long id;

  @Column
  String type;

  @Column
  boolean isMale;

}
```

</div>

</div>

### Relationships

    Colony (1..1) -> Queen (1...many)-> Ants

#### 1-1

we have a `Queen` and `Colony` table, we want to establish a 1-1 relationship. We want the database to care when data is removed, such as if a fire occurs and destroys the `Colony`. When the `Colony` is destroyed, we assume the `Queen` no longer exists, so we want to "kill" the `Queen` for that `Colony` so it no longer exists.

To establish the connection, we will define a Foreign Key that the child, `Queen` uses:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="6fc8a53d-87f8-451d-abfe-2f9ba77f4398" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
@ModelContainer
@Table(database = ColonyDatabase.class)
public class Queen extends BaseModel {

  //...previous code here

  @Column
  @ForeignKey(saveForeignKeyModel = false)
  Colony colony;

}
```

</div>

</div>

<div class="highlight highlight-source-java">

Tips: For performance reasons we use `saveForeignKeyModel=false` to not save the parent `Colony `when the `Queen` object is saved.

#### 1-to-many

Now that we have a `Colony` with a `Queen` that belongs to it, we need some ants to serve her!

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="08bcb13e-1b22-495e-a6e3-87f51bc8bfa7" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
@Table(database = ColonyDatabase.class)

public class Ant extends BaseModel {
 //...previous code here
  @ForeignKey(saveForeignKeyModel = false)
  ForeignKeyContainer<Queen> queenForeignKeyContainer;

  /**
 * Example of setting the model for the queen.
 */
  public void associateQueen(Queen queen) {
    queenForeignKeyContainer = FlowManager.getContainerAdapter(Queen.class).toForeignKeyContainer(queen);
  }
}
```

</div>

</div>

<div class="highlight highlight-source-java">

We use a `ForeignKeyContainer` in this instance, since we can have thousands of ants. For performance reasons this will "lazy-load" the relationship of the `Queen` and only run the query on the DB for the `Queen` when we call `toModel()`.

Since `ModelContainer` usage is not generated by default, we must add the `@ModelContainer` annotation to the `Queen` class in order to use for a `ForeignKeyContainer`.

Next, we establish the 1-to-many relationship by lazy-loading the ants for performance reasons:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="f63d3aa5-9997-4206-b9bd-11b3aa8183b7" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
@ModelContainer
@Table(database = ColonyDatabase.class)
public class Queen extends BaseModel {
  //...

  // needs to be accessible for DELETE
  List<Ant> ants;

  @OneToMany(methods = {OneToMany.Method.SAVE, OneToMany.Method.DELETE}, variableName = "ants")
  public List<Ant> getMyAnts() {
    if (ants == null || ants.isEmpty()) {
            ants = SQLite.select()
                    .from(Ant.class)
                    .where(Ant_Table.queenForeignKeyContainer_id.eq(id))
                    .queryList();
    }
    return ants;
  }
}
```

</div>

</div>

If you wish to lazy-load the relationship yourself, specify `OneToMany.Method.DELETE` and `SAVE` instead of `ALL`. If you wish not to save them whenever the `Queen`'s data changes, specify `DELETE` and `LOAD` only.

### SQL Statements Using the Wrapper Classes

#### SELECT Statements and Retrieval Methods

A `SELECT` statement retrieves data from the database. We retrieve data via 1. Normal `Select` on the main thread 2. Running a `Transaction` using the `TransactionManager` (recommended for large 3. queries).

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="c2cb1919-042f-4504-8c28-9563e8d100bf" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
// Query a List
SQLite.select().from(SomeTable.class).queryList();
SQLite.select().from(SomeTable.class).where(conditions).queryList();

// Query Single Model
SQLite.select().from(SomeTable.class).querySingle();
SQLite.select().from(SomeTable.class).where(conditions).querySingle();

// Query a Table List and Cursor List
SQLite.select().from(SomeTable.class).where(conditions).queryTableList();
SQLite.select().from(SomeTable.class).where(conditions).queryCursorList();

// Query into a ModelContainer!
SQLite.select().from(SomeTable.class).where(conditions).queryModelContainer(new MapModelContainer<>(SomeTable.class));

// SELECT methods
SQLite.select().distinct().from(table).queryList();
SQLite.select().from(table).queryList();
SQLite.select(Method.avg(SomeTable_Table.salary))
  .from(SomeTable.class).queryList();
SQLite.select(Method.max(SomeTable_Table.salary))
  .from(SomeTable.class).queryList();

// Transact a query on the DBTransactionQueue
TransactionManager.getInstance().addTransaction(
  new SelectListTransaction<>(new Select().from(SomeTable.class).where(conditions),
  new TransactionListenerAdapter<List<SomeTable>>() {
    @Override
    public void onResultReceived(List<SomeTable> someObjectList) {
      // retrieved here
});

// Selects Count of Rows for the SELECT statment
long count = SQLite.selectCountOf()
  .where(conditions).count();
```

</div>

</div>

##### <a href="https://github.com/Raizlabs/DBFlow/blob/master/usage/SQLQuery.md#order-by" class="external-link" rel="nofollow" style=""><span class="octicon octicon-link legacy-color-text-default"> </span></a>Order By

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="689787c3-00ae-4b18-8e0f-2211f255e062" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
// true for 'ASC', false for 'DESC'
SQLite.select()
  .from(table)
  .where()
  .orderBy(Customer_Table.customer_id, true)
  .queryList();
```

</div>

</div>

##### <a href="https://github.com/Raizlabs/DBFlow/blob/master/usage/SQLQuery.md#group-by" class="external-link" rel="nofollow" style=""><span class="octicon octicon-link legacy-color-text-default"> </span></a>Group By

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="893da570-c3a1-4c64-80cb-08f9c3f1d49b" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
SQLite.select()
  .from(table)
  .groupBy(Customer_Table.customer_id, Customer_Table.customer_name)
  .queryList();
```

</div>

</div>

##### <a href="https://github.com/Raizlabs/DBFlow/blob/master/usage/SQLQuery.md#having" class="external-link" rel="nofollow" style=""><span class="octicon octicon-link legacy-color-text-default"> </span></a>HAVING

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="c12ba5e2-93d0-46bc-a8c4-c3377f88ff91" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
SQLite.select()
  .from(table)
  .groupBy(Customer_Table.customer_id, Customer_Table.customer_name))
  .having(Customer_Table.customer_id.greaterThan(2))
  .queryList();
```

</div>

</div>

##### <a href="https://github.com/Raizlabs/DBFlow/blob/master/usage/SQLQuery.md#limit--offset" class="external-link" rel="nofollow" style=""><span class="octicon octicon-link legacy-color-text-default"> </span></a>LIMIT + OFFSET

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="55e51313-2a75-4685-97c6-f92c35fa89e1" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
SQLite.select()
  .from(table)
  .limit(3)
  .offset(2)
  .queryList();
```

</div>

</div>

#### <a href="https://github.com/Raizlabs/DBFlow/blob/master/usage/SQLQuery.md#update-statements" class="external-link" rel="nofollow" style=""><span class="octicon octicon-link legacy-color-text-default"> </span></a>UPDATE statements

There are two ways of updating data in the database: 1. Calling `SQLite.update()`or Using the `Update` class 2. Running a`Transaction` using the `TransactionManager` (recommended for thread-safety, however seeing changes are async).

In this section we will describe bulk updating data from the database.

From our earlier example on ants, we want to change all of our current male "worker" ants into "other" ants because they became lazy and do not work anymore.

Using native SQL:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="0dcc72a6-4e86-422c-9f92-58be7e6388ac" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
UPDATE Ant SET type = 'other' WHERE male = 1 AND type = 'worker';

Using DBFlow:
// Native SQL wrapper
Where<Ant> update = SQLite.update(Ant.class)
  .set(Ant_Table.type.eq("other"))
  .where(Ant_Table.type.is("worker"))
    .and(Ant_Table.isMale.is(true));
update.queryClose();


// TransactionManager (more methods similar to this one)
TransactionManager.getInstance().addTransaction(new QueryTransaction(DBTransactionInfo.create(BaseTransaction.PRIORITY_UI), update);
```

</div>

</div>

<div class="highlight highlight-source-java">

     

</div>

#### <a href="https://github.com/Raizlabs/DBFlow/blob/master/usage/SQLQuery.md#delete-statements" class="external-link" rel="nofollow" style=""><span class="octicon octicon-link legacy-color-text-default"> </span></a>DELETE statements

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="1124c7c8-edcb-495d-931d-04c84cc28951" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
// Delete a whole table
Delete.table(MyTable.class, conditions);

// Delete multiple instantly
Delete.tables(MyTable1.class, MyTable2.class);

// Delete using query
SQLite.delete(MyTable.class)
  .where(DeviceObject_Table.carrier.is("T-MOBILE"))
    .and(DeviceObject_Table.device.is("Samsung-Galaxy-S5"))
  .query();
```

</div>

</div>

#### <a href="https://github.com/Raizlabs/DBFlow/blob/master/usage/SQLQuery.md#join-statements" class="external-link" rel="nofollow" style=""><span class="octicon octicon-link legacy-color-text-default"> </span></a>JOIN statements

For reference, (<a href="http://www.tutorialspoint.com/sqlite/sqlite_using_joins.htm" class="external-link" rel="nofollow">JOIN examples</a>).

`JOIN` statements are great for combining many-to-many relationships.

For example we have a table named `Customer` and another named `Reservations`.

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="f03cefcc-0766-4511-bd5c-c3904b75889b" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
SELECT FROM `Customer` AS `C` INNER JOIN `Reservations` AS `R` ON `C`.`customerId`=`R`.`customerId`
```

</div>

</div>

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="240d9981-b709-467b-8c51-6d77f80d2121" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
// use the different QueryModel (instead of Table) if the result cannot be applied to existing Model classes.
List<CustomTable> customers = new Select()   
  .from(Customer.class).as("C")   
  .join(Reservations.class, JoinType.INNER).as("R")    
  .on(Customer_Table.customerId
      .withTable(new NameAlias("C"))
    .eq(Reservations_Table.customerId.withTable("R"))
    .queryCustomList(CustomTable.class);
```

</div>

</div>

The `IProperty.withTable()` method will prepend a `NameAlias` or the `Table` alias to the `IProperty` in the query, convenient for JOIN queries:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="5e28968a-825d-41a9-9340-80029318f67a" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
SELECT EMP_ID, NAME, DEPT FROM COMPANY LEFT OUTER JOIN DEPARTMENT
      ON COMPANY.ID = DEPARTMENT.EMP_ID
```

</div>

</div>

in DBFlow:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="ba1a89a4-f288-4994-ae61-ef69be23d2ac" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
SQLite.select(Company_Table.EMP_ID, Company_Table.DEPT)
  .from(Company.class)
  .leftOuterJoin(Department.class)
  .on(Company_Table.ID.withTable().eq(Department_Table.EMP_ID.withTable()))
  .queryList();
```

</div>

</div>

 

<div class="highlight highlight-source-java">

### Transaction Manager

`TransactionManager` is the main class that manages batch DB interactions. It is useful for retrieving, updating, saving, and deleting lists of items. It wraps around the *native* builder notation for SQL statements and makes it dead-simple to perform database **Transactions**. This class also provides a way to asynchronously execute database operations on the same thread so that data operations do no block the UI thread.

The `TransactionManager` utilizes a `DBTransactionQueue`. This queue is based on the `VolleyRequestQueue` from Volley by using a `PriorityBlockingQueue`. This queue will order our database transactions by priority using the following order (highest to lowest): 1. **UI**: Reserved for only immediate tasks and all forms of fetching that will display on the UI 2. **HIGH**: Reserved for tasks that will influence user interaction, 3. such as displaying data in the UI at some point in the future (not necessarily right away) 4. **NORMAL**: Default priority for a `Transaction`, good when adding transactions that the app does not need to access right away. 5. **LOW**: Low-priority, reserved for non-essential tasks.

`DBTransactionInfo`: Holds information on how to process a `BaseTransaction` on the `DBTransactionQueue`. It contains a name and priority. The name is purely for debugging purposes during runtime and to identify it when executed. The priority corresponds to the previous paragraph on priority.

These priorities are just `int` and you can specify your own higher, or different priorities as needed.

For advance usage, the `TableTransactionManager` or by extending the `TransactionManager` class, you can create and specify its own `DBTransactionQueue`. You will need to now use that manager for any transaction that was intended for it.

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="9daf59eb-c61b-4e2a-af25-74d760d48fa2" macro-name="code" style="border-width: 1px;">

<div class="codeHeader panelHeader pdl" style="border-bottom-width: 1px;">

**Example**

</div>

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
 ProcessModelInfo<SomeModel> processModelInfo = ProcessModelInfo<SomeModel>.withModels(models)
                                                                            .result(resultReceiver)
                                                                            .info(myInfo);

  TransactionManager.getInstance().saveOnSaveQueue(models);

  // or directly to the queue
  TransactionManager.getInstance().addTransaction(new SaveModelTransaction<>(processModelInfo));

  // Updating only updates on the ``DBTransactionQueue``
  TransactionManager.getInstance().addTransaction(new UpdateModelListTransaction(processModelInfo));

  TransactionManager.getInstance().addTransaction(new DeleteModelListTransaction(processModelInfo));
```

</div>

</div>

There are all sorts of methods in `TransactionManager` for performing queries on the `DBTransactionQueue` for fetching, saving, updateing, and deleting. Check it out!

#### <a href="https://github.com/Raizlabs/DBFlow/blob/master/usage/Transactions.md#retrieving-models" class="external-link" rel="nofollow" style=""><span class="octicon octicon-link legacy-color-text-default"> </span></a>Retrieving Models

The `SelectListTransaction` and `SelectSingleModelTransaction` performs the *select* on the `DBTransactionQueue`, and when it completes the `TransactionListener` will be called on the UI thread. A normal `SQLite.select()` will be done on the current thread. While this is OK for simple database interactions, it's much better to perform these operations on the`DBTransactionQueue` so that other operations do not cause a "lock" of the main thread.

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="6fee9fe0-68f9-44ec-9a1b-1126a70fac9f" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
// Just get all items from the table
  // You can even use Select and Where statements instead
  TransactionManager.getInstance().addTransaction(new SelectListTransaction<>(new TransactionListenerAdapter<TestModel.class>() {
     @Override
    public void onResultReceived(List<TestModel> testModels) {
        // on the UI thread, do something here
    }
  }, TestModel.class, condition1, condition2,..);
```

</div>

</div>

#### <a href="https://github.com/Raizlabs/DBFlow/blob/master/usage/Transactions.md#custom-transactions" class="external-link" rel="nofollow" style=""><span class="octicon octicon-link legacy-color-text-default"> </span></a>Custom Transactions

This library makes it very easy to perform custom transactions. Add them to the `TransactionManager` by:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="5482f0b4-25f4-41a7-9a86-12bd7cf94cc7" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
TransactionManager.getInstance().addTransaction(myTransaction);
```

</div>

</div>

There are a few ways to create a specific transaction you wish:

 

- Extending `BaseTransaction` will require you to run something in `onExecute()`.

  <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="88232bf1-abdc-4c13-a575-bfeaee249f99" macro-name="code" style="border-width: 1px;">

  <div class="codeContent panelContent pdl">

  ``` syntaxhighlighter-pre
  BaseTransaction<TestModel1> testModel1BaseTransaction = new BaseTransaction<TestModel1>() {
  @Override
  public TestModel1 onExecute() {
  // do something and return an object
   return testModel;
  }
  };
  ```

  </div>

  </div>

  <div class="highlight highlight-source-java">

       

  </div>

- `BaseResultTransaction` adds a simple `TransactionListener` that enables you to listen for transaction updates.

  <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="9765d82b-266e-4a32-a3e5-3f75fabac70f" macro-name="code" style="border-width: 1px;">

  <div class="codeContent panelContent pdl">

  ``` syntaxhighlighter-pre
  BaseResultTransaction<TestModel1> baseResultTransaction = new BaseResultTransaction<TestModel1>(dbTransactionInfo, transactionListener) {
  @Override
  public TestModel1 onExecute() {
  return testmodel;
  }
  };
  ```

  </div>

  </div>

- `ProcessModelTransaction` takes in a model and enables you to define how to process each individual model in the`DBTransactionQueue`.

  <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="bd35c7ed-e5d3-45ed-baba-c10cb2435cb7" macro-name="code" style="border-width: 1px;">

  <div class="codeContent panelContent pdl">

  ``` syntaxhighlighter-pre
  public class CustomProcessModelTransaction<ModelClass extends Model> extends ProcessModelTransaction<ModelClass> {

  public CustomProcessModelTransaction(ProcessModelInfo<ModelClass> modelInfo) {
     super(modelInfo);
  }

  @Override
  public void processModel(ModelClass model) {
     // process model class here!
  }
  }
  ```

  </div>

  </div>

- `QueryTransaction` has you use a `Queriable` to retrieve a cursor in whatever way you what.

  <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="a40c863c-595d-4fce-8a6d-99d9d9d78826" macro-name="code" style="border-width: 1px;">

  <div class="codeContent panelContent pdl">

  ``` syntaxhighlighter-pre
  // any Where, From, Insert, Set, and StringQuery all are valid parameters
  TransactionManager.getInstance().addTransaction(new QueryTransaction(DBTransactionInfo.create(),
    SQLite.delete().from(MyTable.class).where(MyTable_Table.name.is("Deleters"))));
  ```

  </div>

  </div>

<div class="highlight highlight-source-java">

     

</div>

</div>

</div>

  

</div>

  

</div>

</div>

</div>

</div>

%% ai-graph-start %%

**Related notes:**
- [[Persistence layer implementation]]
- [[How to implement an audit log using Hibernate Envers]]
- [[Kotlin migration plan]]
- [[08_ Fintech - Innovation Coding convention]]
- [[Reporting - Java class configuration]]

%% ai-graph-end %%