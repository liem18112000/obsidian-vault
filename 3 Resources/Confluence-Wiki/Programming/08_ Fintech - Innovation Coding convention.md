---
title: "08_[Fintech - Innovation] Coding convention"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/GRAVITY/pages/47134411670/08_+Fintech+-+Innovation+Coding+convention
space: "GRAVITY"
topic: programming
relevance: 0.738
depth: 2.65
updated: 2022-06-24
attachments: 2
tags:
  - confluence
  - programming
  - space/gravity
---

# 08_[Fintech - Innovation] Coding convention

> [!info] Imported from Confluence
> Space **GRAVITY** · updated 2022-06-24 · [open original](https://axonivy.atlassian.net/wiki/spaces/GRAVITY/pages/47134411670/08_+Fintech+-+Innovation+Coding+convention)
> Relevance 0.738 · topic `programming`

Convention based on:[/wiki/spaces/COF/pages/37974943764](https://axonivy.atlassian.net/wiki/spaces/COF/pages/37974943764)

## Override points:

## I. Java convention

### **<u>Formatter:</u>**

- Import to Eclipse (Java/Code Style/Organize import - Java/Code Style/Formatter)

<span class="confluence-embedded-file-wrapper conf-macro output-inline" hasbody="false" macro-id="87004e61-03c5-4597-a7f4-022a5e935031" macro-name="view-file"><a href="../_attachments/47134411670-formatter.xml" class="confluence-embedded-file" data-nice-type="XML File" data-file-src="/wiki/download/attachments/47134411670/formatter.xml?version=1&amp;modificationDate=1656070594345&amp;cacheVersion=1&amp;api=v2" data-mime-type="text/xml" data-has-thumbnail="true">

![[47134411670-formatter.xml]]

</a></span><span class="confluence-embedded-file-wrapper conf-macro output-inline" hasbody="false" macro-id="755f568c-5337-4e5f-8e05-cee429f69c1a" macro-name="view-file"><a href="../_attachments/47134411670-example.importorder" class="confluence-embedded-file" data-nice-type="null" data-file-src="/wiki/download/attachments/47134411670/example.importorder?version=1&amp;modificationDate=1656070594253&amp;cacheVersion=1&amp;api=v2" data-mime-type="application/octet-stream" data-has-thumbnail="true">

![[47134411670-example.importorder]]

</a></span>

### Package

- The package name should be  in lowercase letter

- Java classes should be packaged base on components.

- Name should follow \[<a href="http://com.axonfintech.fi" class="external-link" rel="nofollow">com.axonfintech.fi</a>\].sericername.specific reponsibility Ex: **com.axonfintech.fi.addressverification.service.api**

- Should not use plural word in package name.  
  **com.axonfintech.fi.addressverification.service.constants =\>com.axonfintech.fi.addressverification.service.constant**

## Additional points

#### Method content:

- Every method should have the comment java doc

#### Java doc

- @param, @Throws must have a description

- The head of each java class has to be

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="255e51f2-a610-4375-92ef-adefa9aac5e2" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
/*
class-name.java

Copyright by Axon Ivy (Lucerne), all rights reserved. (should be line 4)
*/
```

</div>

</div>

#### Specification:

- Should include all default error code

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="201c23f2-dc0b-4da8-b2c3-aed43ff2ef37" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
        '300':
          description: 'Moved: The resource has moved.'
        '400':
          description: 'Bad Request: The request could not be understood by the server due to malformed syntax. Something like Domain validation errors, missing data, invalid input, etc.'
        '401':
          description: 'Unauthorized: The caller is not authorized.'
        '403':
          description: 'Forbidden: The caller has no permission.'
        '404':
          description: 'Not found: The server has not found anything matching the Request-URI.'
        '408':
          description: 'Timeout: The request timeout.'
```

</div>

</div>

- Use Bear Authentication

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="936f4470-f743-4c7f-9a88-ac51c5aeac08" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
security:
  - BearerAuth: []
components:
  securitySchemes:
    BearerAuth:
      type: http
      scheme: bearer
      bearerFormat: JWT
```

</div>

</div>

- Always include tenantId parameter, It should be located at the bottom of components:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="ec09c041-547d-4136-bd04-fcf5d03f6e3f" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
parameters:
    tenantId:
      in: header
      name: tenantId
      required: true
      description: 'The tenant identifier.'
      schema:
        type: string
        maxLength: 64
        example: customer1
```

</div>

</div>

- Property should always have a proper description

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="79f2e6f0-afb3-44b8-8c68-2d68775b3759" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
        birthDate:
          type: string
          format: date-time
          description: "The birth date, the date format as defined by date - [RFC3339](https://tools.ietf.org/html/rfc3339)."
          example: "2000-01-29T00:00:00.00Z"
```

</div>

</div>

#### Implementation of spec:

- Always set root package when init a new project

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="c43c261b-9223-4089-ac8a-e6249e847c3c" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
E.g.
project name: fi-credit-rating-service
root pacckage should be: (gradle.properties)
projectRootPackageName   = com.axonfintech.fi.credit.rating.service
```

</div>

</div>

- Edit base URL after init project:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="060e00ea-f0b7-4ec7-be6e-a94b38b5fea8" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
# define base url of the service (gradle.properties)
kubernetesUrlPath        = /api/credit-ratings
```

</div>

</div>

- Avoid using reflection, in the case have to use we have to confirm with CTO (Patrick)

- Return 401 if authentication failed inside our service:

E.g. In case a 401 occurs inside our service stack then we will propagate 401 because we are based on the same authentication.

- Return 503 if can not reach remote client:

E.g. About the authentication / access error of a remote resource (post, crif…) we have to return 503. As shared in the document attached API design we have defined the 503. In case we have remote service access we must include it.
