---
title: "WIP: Recipe: How to adapt Unit Test to be able to run with Junit 5"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47142830245/WIP+Recipe+How+to+adapt+Unit+Test+to+be+able+to+run+with+Junit+5
space: "LUZ"
topic: testing
relevance: 0.76
depth: 2.74
updated: 2022-07-20
attachments: 0
tags:
  - confluence
  - testing
  - space/luz
---

# WIP: Recipe: How to adapt Unit Test to be able to run with Junit 5

> [!info] Imported from Confluence
> Space **LUZ** · updated 2022-07-20 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47142830245/WIP+Recipe+How+to+adapt+Unit+Test+to+be+able+to+run+with+Junit+5)
> Relevance 0.76 · topic `testing`

This page will list out points should be adapted to be able to run unit tests in JUnit 5.

## Dependencies Update

In order to use JUnit 5, we need to include this dependency:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="28b6f116-aec4-44b2-bcd1-1a7b8b9c38c6" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
<dependency>
    <groupId>org.junit.jupiter</groupId>
    <artifactId>junit-jupiter-engine</artifactId>
    <version>5.6.0</version>
    <scope>test</scope>
</dependency>
```

</div>

</div>

## Unit Test adaptations

### Use new package `org.junit.jupiter.api.*` instead of `org.junit.*`

In order to avoid misleading JUnit 4 and JUnit 5, the JUnit 5 provides a separately package for using it. So in test classes which are using JUnit 4, it is required to change code to use new package. Below is an example:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="1fc98099-02e6-4ae9-86f6-af30e4596f67" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
// JUnit 4
import org.junit.Test;
import static org.junit.Assert.assertTrue;
 
class JUnitExampleTest {
     
     @Test
     public void testMethodX_shouldReturnTrue_WhenItIsCalled() {
        assertTrue(new JUnitExample().methodX());
     }
}

# JUnit 5
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.assertTrue;
 
class JUnitExampleTest {
     
     @Test
     public void testMethodX_shouldReturnTrue_WhenItIsCalled() {
        assertTrue(new JUnitExample().methodX());
     }
}
```

</div>

</div>

### Expectation inside @Test annotation is no longer support.

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="3ebf281f-2387-4765-a378-f1e747156e11" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
// JUnit 4
@Test(expected = Exception.class)
public void shouldRaiseAnException() throws Exception {
    // ...
}

// JUnit 5
public void shouldRaiseAnException() throws Exception {
    Assertions.assertThrows(Exception.class, () -> {
        //...
    });
}
```

</div>

</div>

## Ivy project adaptations (under consideration)

When we lift Ivy projects to support JUnit 5, we are able to execute test classes with Ivy environment without need of mocking so therefore we can get rid of using PowerMockRunner which runs heavily and slowly. This can be done by using @IvyTest annotation and AppFixture class. Please follow this page to know more how to use this: <a href="https://developer.axonivy.com/doc/9.3/concepts/testing/unit-testing.html#set-up-the-ivy-environment" class="external-link" data-card-appearance="inline" rel="nofollow">https://developer.axonivy.com/doc/9.3/concepts/testing/unit-testing.html#set-up-the-ivy-environment</a>.

If we consider using JUnit 5 and @IvyTest in our test classes, there things below should be considered and adapted:

### CMS content using for test cases

Since we are able to access to Ivy in the test runtime so we don’t need to mock the behavior of mocking like below:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="29e9b1fb-1b2f-43ba-9e7e-e4dfab998188" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
PowerMockito.when(CMSEscapeBean.co("/ch/klara/luz/fin/accounting/creditCard/overview/label/employeeHolderTypeTitle")).thenReturn("Employee");
```

</div>

</div>

Instead, we can use CMS from CMS were defined in business code.

Please be aware that CMS content which will be used in test is also available in the current project or its parent projects. Otherwise you have to prepare it.
