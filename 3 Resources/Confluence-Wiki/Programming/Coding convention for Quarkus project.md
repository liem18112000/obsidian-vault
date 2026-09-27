---
title: "Coding convention for Quarkus project"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/GRAVITY/pages/48197271777/Coding+convention+for+Quarkus+project
space: "GRAVITY"
topic: programming
relevance: 0.753
depth: 2.49
updated: 2025-06-06
attachments: 18
tags:
  - confluence
  - programming
  - space/gravity
---

# Coding convention for Quarkus project

> [!info] Imported from Confluence
> Space **GRAVITY** · updated 2025-06-06 · [open original](https://axonivy.atlassian.net/wiki/spaces/GRAVITY/pages/48197271777/Coding+convention+for+Quarkus+project)
> Relevance 0.753 · topic `programming`

<div class="toc-macro client-side-toc-macro conf-macro output-block" cssliststyle="decimal" hasbody="false" headerelements="H1,H2,H3,H4,H5,H6" macro-id="983d4984-c3f6-4e99-aaa2-5ec7dcaa8f54" macro-name="toc" numberedoutline="false" structure="list">

</div>

# Java convention

## Package

When structuring a Java package for a microservice, it's important to follow conventions that reflect the microservice's purpose, promote modularity, and make it easier to maintain and understand. Below is a commonly used package structure for Java microservices:

### Base Package

The base package usually follows the reverse domain name convention:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="dd9d9446-9ecb-41aa-8a60-6231d87b9b3b" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
com.<organization>.<project>.<service>
```

</div>

</div>

For example, if your organization's `example`, project is `orders`, and the service is `inventory`, the base package could be:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="cb15c287-5361-4464-a292-bd03ef1eca9e" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
com.example.orders.inventory
```

</div>

</div>

### Common Sub-packages

Below is a typical structure for a microservice:

1.  **controller**: Contains REST API controllers or endpoints.  
    Ex: `com.example.orders.inventory.controller`

2.  **service**: Contains business logic.  
    Ex: `com.example.orders.inventory.service`

3.  **repository**: Contains classes for interacting with the database (e.g., JPA repositories).  
    Ex: `com.example.orders.inventory.repository`

4.  **entity**: Contains entity classes that represent database tables.  
    Ex: `com.example.orders.inventory.entity`

5.  **dto**: Contains Data Transfer Objects for API requests and responses.  
    Ex: `com.example.orders.inventory.dto`

6.  **exception**: Contains custom exception classes and exception handling.  
    Ex: `com.example.orders.inventory.exception`

7.  **config**: Contains configuration classes (e.g., Spring Beans, properties).  
    Ex: `com.example.orders.inventory.config`

8.  **util**: Contains utility classes (e.g., common functions).  
    Ex: `com.example.orders.inventory.util`

9.  **mapper**: (Optional) Contains mappers for converting between DTOs and entities.  
    Ex: `com.example.orders.inventory.mapper`

10. **security**: (Optional) Contains classes related to authentication and authorization.  
    Ex: `com.example.orders.inventory.security`

### Additional Conventions

1.  The package name should be in lowercase letters

2.  Java classes should be packaged based on components.

3.  Do not use plural words in package names.

4.  Keep the package focused: Avoid creating large, generic packages like common unless necessary.

5.  Feature-specific sub-packages: If a service has distinct features, create sub-packages for each feature:

    <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="0a0ac86a-5480-40fc-a193-55beba030fb7" macro-name="code" style="border-width: 1px;">

    <div class="codeContent panelContent pdl">

    ``` syntaxhighlighter-pre
    com.example.orders.inventory.featureA.controller
    com.example.orders.inventory.featureA.service
    com.example.orders.inventory.featureB.repository
    ```

    </div>

    </div>

6.  Keep third-party integrations separate: If the service integrates with external APIs, create a package for integration-related code.

    <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="8750b052-a03b-4b61-901e-2b780cb05dcb" macro-name="code" style="border-width: 1px;">

    <div class="codeContent panelContent pdl">

    ``` syntaxhighlighter-pre
    com.example.orders.inventory.integration
    ```

    </div>

    </div>

## Class

1.  Don't use the Data Model class abstract

2.  Logging uses the common Logger

3.  No \*util \*helper classes anymore so we apply the single responsibility pattern

4.  Do not add new methods to deprecated classes

5.  Avoid using synchronized singleton patterns to construct instances.

6.  Service: Apply singleton when possible

7.  Remove unused classes and methods.

8.  The class name should be:

    1.  Meaningful

    2.  Start with an uppercase letter

    3.  Be a NOUN

    4.  Camel case

    5.  Single-responsibility

    6.  Avoid using a plural noun

    7.  Avoid using the noisy word

        1.  Ex: a, an, the,…

    8.  Abstract classes start with "Abstract"

    9.  Avoid meaningless prefixes/suffixes like "data", "object", etc.  
        EX: data (employeeData), object (personObject), obj, str...

    10. Avoid using the preceding (“I” for interface,...).  
        Ex: IWorkflowProcessor

## Method

1.  Method name should be:

    1.  Meaningful

    2.  Start with a lowercase letter

    3.  Be a VERB

    4.  Camel case

    5.  Single-responsibility

    6.  The name should be in the Object's context.  
        EX: To name a method that does something with a student object, this method in StudentService, we just name it: delete, or deleteById, or deleteByCode, readList, readListByBirthYear...

2.  Method parameters should be

    1.  Maximum 4 parameters

    2.  Avoid using Boolean or Flag in the parameter

3.  Method content:

    1.  CONST should be all uppercase

    2.  The indent should be Tab

    3.  Avoid using negative conditions  
        Ex: if (!A())

    4.  Avoid using Magic Numbers and magic texts → Use meaningful Constants

    5.  Single responsibility.

    6.  Do not use static for public methods → apply for new methods

    7.  The Variable Name should be based on the usage purpose  
        EX: deletedProduct, selectedStudent, firstPersonInDatabase...

## Unit Test

1.  Unit test method name: methodName_stateUnderTest_expectedBehavior  
    Example:  
    - isAdult_ageLessThan18_false  
    - withdrawMoney_invalidAccount_throwException  
    - withdrawMoney_invalidAccountAndNoCard_throwException  
    - admitStudent_missingMandatoryFields_failToAdmit

## Best practices

We encourage the code to follow the best practices here: [/wiki/spaces/COF/pages/37974943794](https://axonivy.atlassian.net/wiki/spaces/COF/pages/37974943794)

# API implementation

Based on the AXON Fintech API Specifications documentation we will follow its best practices when designing and implementing APIs for status codes, operations, and URL naming…

Documentation:

<span class="confluence-embedded-file-wrapper conf-macro output-inline" hasbody="false" macro-id="936f98a8-7fe1-4af6-bd18-3b64fd664ff6" macro-name="view-file"><a href="../_attachments/48197271777-20210326-AXON-Fintech-API-Specifictations.pdf" class="confluence-embedded-file" data-nice-type="PDF Document" data-file-src="/wiki/download/attachments/48197271777/20210326-AXON-Fintech-API-Specifictations.pdf?version=3&amp;modificationDate=1733469776948&amp;cacheVersion=1&amp;api=v2" data-mime-type="application/pdf" data-has-thumbnail="true">

![[48197271777-20210326-AXON-Fintech-API-Specifictations.pdf]]

</a></span>

# Changelog format

1.  There are three different kinds of subsections:

    1.  Added: for new features.

    2.  Changed: for changes in existing functionality.

    3.  Fixed: for any bug fixes.

2.  The release number will be defined as major.minor.revision numbers, e.g. 1.4.2.

    1.  The version number follows the semantic version number concept, see <a href="https://semver.org" class="external-link" rel="nofollow">https://semver.org</a>!

    2.  The major number is given by the release planning.

    3.  The minor number is increased when new features are added

    4.  The revision number will be increased when new bugs are fixed.

    5.  During the Sprint we have SNAPSHOT releases, e.g. 1.4.2-SNAPSHOT (for internal automated builds)

Documentation:

<span class="confluence-embedded-file-wrapper conf-macro output-inline" hasbody="false" macro-id="299f760e-a908-4837-a5d2-987567ffeae3" macro-name="view-file"><a href="../_attachments/48197271777-20210506-AXON-Fintech-Release &amp; PO&#39;s Responsibility.pdf" class="confluence-embedded-file" data-nice-type="PDF Document" data-file-src="/wiki/download/attachments/48197271777/20210506-AXON-Fintech-Release%20%26%20PO%27s%20Responsibility.pdf?version=1&amp;modificationDate=1733470994938&amp;cacheVersion=1&amp;api=v2" data-mime-type="application/pdf" data-has-thumbnail="true">

![[48197271777-20210506-AXON-Fintech-Release & PO's Responsibility.pdf]]

</a></span>

# CB build checkstyles

The project **MUST** be built successfully using the **CB Tool**. Make sure that the project doesn’t violate any conventions or checkstyles of the CB Tool.

Moreover, the **CHANGELOG.md** file must accomplish the conventions of the CB Tool to build success.

The **BUILD SUCCESSFUL** will be shown similar to the image below:


![[48197271777-image-20241220-043717.png]]



# Import code format to IntelliJ

The developer must format code by the IntelliJ IDE before committing the code. The standard format for IntelliJ is attached below. Here are the steps to import the code format:

1.  Download the format file

2.  Open IntelliJ

3.  Go to Settings → Editor → Code Style

4.  Look for the Scheme setting and click on the 3 dots button and a context menu will show

5.  Select Import Scheme → IntelliJ IDEA code style XML

6.  Select the file downloaded in step 1

7.  When import is successful it should be similar to this image:  

    

![[48197271777-image-20241220-070517.png]]



**Axonfintech format scheme:**

<span class="confluence-embedded-file-wrapper conf-macro output-inline" hasbody="false" macro-id="440bfb62-8921-4302-a644-b0de1f97af2d" macro-name="view-file"><a href="../_attachments/48197271777-axonfintech-format.intellij.xml" class="confluence-embedded-file" data-nice-type="XML File" data-file-src="/wiki/download/attachments/48197271777/axonfintech-format.intellij.xml?version=1&amp;modificationDate=1733469749297&amp;cacheVersion=1&amp;api=v2" data-mime-type="text/xml" data-has-thumbnail="true">

![[48197271777-axonfintech-format.intellij.xml]]

</a></span>

# Setup Copyright in IntelliJ

When running the CB Tool it will require each Java file must have a copyright at the top as shown in the image here:


![[48197271777-image-20241220-064457.png]]



If we don’t have the copyright for the Java file we will not be able to build the project successfully and we will receive an error as shown below whenever we run the CB build:


![[48197271777-image-20241220-064726.png]]



To help us create copyright easier whenever we create a new Java file, we will set up the copyright for our project by IntelliJ by the following steps:

1.  Go to Settings → Editor → Copyright → Copyright Profiles

2.  Click on the **Add Profile** button

3.  Enter the **axonfintech** for the Profile Name

4.  Put this content in the Copyright text input

    <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="4fb754c2-34d8-4e34-9ff1-e0e900354367" macro-name="code" style="border-width: 1px;">

    <div class="codeContent panelContent pdl">

    ``` syntaxhighlighter-pre
    $file.fileName

    Copyright by Axon Fintech AG, all rights reserved.
    ```

    </div>

    </div>

    The result should look like the image here:

    

![[48197271777-image-20241220-070205.png]]



5.  Go back to the Copyright setting

6.  In the Default project copyright select the **axonfintech** which we created in step 4

7.  The result will be like the image here:

    

![[48197271777-image-20241220-070941.png]]



# Setup SonarQube in IntelliJ

SonarQube for IntelliJ is a plugin that directly integrates SonarQube's code quality and security analysis capabilities into the IntelliJ IDEA development environment. This plugin helps developers identify and fix coding issues in real time, much like a spell-checker for code. It provides contextual guidance to help developers understand why a problem exists, assess its risk, and learn how to fix it. We will set up the SonarQube for our IntelliJ by the following steps:

1.  Go to Settings → Plugins

2.  Search for **"SonarQube"** in the **Marketplace** tab. Click Install and restart IntelliJ IDEA to activate the plugin

    

![[48197271777-image-20250106-031637.png]]



3.  After restarting, go to Settings → Tools → SonarQube for IDE

4.  In the Settings tab, click on the `+` sign to create a new SonarQube connection

5.  Provide a connection name **axongroupio**

6.  Choose **SonarQube Server** as the connection type

7.  Enter the **SonarQube Server URL**: <a href="https://sonar.axongroupio.ch/" class="external-link" rel="nofollow">https://sonar.axongroupio.ch</a>, then click **Next**

8.  Click on the **Create token** button, and it will open the <a href="https://sonar.axongroupio.ch/" class="external-link" rel="nofollow">sonar.axongroupio.ch</a> page

    

![[48197271777-image-20250106-034644.png]]



9.  Login to the AxonGroup SonarQube by your account

10. Click on the **Allow connection** button

11. Open the IDE and finish, the result will look like the image below

    

![[48197271777-image-20250106-035201.png]]



12. Go to Settings → Tools → SonarQube for IDE → Project Settings

13. In the **Project Settings** tab, select the **SonarQube** connection you created.

14. Click **Bind project to SonarQube**

15. Select the **axongroupio** for the **Connection**

16. Click on the **Search in list…** button, type **Team_COB_Sonar_1,** and choose it

17. Click on the **Apply** button to finish the setup, and the result will look like below

    

![[48197271777-image-20250106-035849.png]]



# Setup the plugin JavaDoc for IntelliJ

When running the CB Tool it will require our methods and classes must have the Javadoc. To help us create Javadoc faster we will use the plugin **JavaDoc** by **Sergey Timofiychuk** that generates Javadoc on Java class elements, like field, method, etc.


![[48197271777-image-20250106-040736.png]]



To install the JavaDoc plugin by Sergey in IntelliJ, follow these steps:

1.  Go to Settings → Plugins

2.  In the **Marketplace** tab, type **"JavaDoc"** in the search bar. Look for the plugin named "JavaDoc" by **Sergey Timofiychuk**

3.  Click the Install button next to the JavaDoc plugin. Once the installation is complete, restart IntelliJ IDEA to activate the plugin

4.  After restarting, you can verify the installation by going to Settings → Plugins and checking if the JavaDoc plugin is listed under Installed

5.  The CB Tool also requires the javadoc for **private methods** so we need to check the **Private** in the **Visibility** option. We can uncheck the **Level Type** and **Field** because they are not required.

    

![[48197271777-image-20250106-041148.png]]
