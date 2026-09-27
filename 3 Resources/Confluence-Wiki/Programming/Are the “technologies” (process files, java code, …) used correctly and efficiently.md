---
title: "Are the “technologies” (process files, java code, …) used correctly and efficiently?"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47098168564/Are+the+technologies+process+files+java+code+used+correctly+and+efficiently
space: "LUZ"
topic: programming
relevance: 0.746
depth: 2.57
updated: 2022-05-10
attachments: 0
tags:
  - confluence
  - programming
  - space/luz
---

# Are the “technologies” (process files, java code, …) used correctly and efficiently?

> [!info] Imported from Confluence
> Space **LUZ** · updated 2022-05-10 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47098168564/Are+the+technologies+process+files+java+code+used+correctly+and+efficiently)
> Relevance 0.746 · topic `programming`

### 1. Ivy Signal

When the user makes the change, we use ivy signal to fire event in order to synchronize data to another database, or to 3rd.

And because luz_components is like a “central“, there are a lot of signal event processes.

The example is: changing company information then synchronize the change to customer of Klara AG, synchronize data to Hubspot system

### Solution

This should be moved to backend side if possible.

### 2. Possible God Class

Possible God Classes are the hint for us to detect the Ivy problems. Is it because we implemented too many business logic on frontend? Or to find out another reason why the logic on front end so complicated.

There are two many classes in luz_components, luz_xhrm_processes, luz_web, …

They are:

- models: RedirectionOrder, CreditCardInformation, OpenedPositionDisplay, Vat, Address, SalaryStatementConfig, Contract, SalaryItemBasicInfo, EmployeeInformation, SellableArticle, InstitutionSalaryDeclaration…

- controller: TaxAtSourceController2021, CompanyHouseHoldRegistrationInsuranceController, SalaryPaymentController, EpostComponentViewHandler, AuthenticationLetterService, CompanyWorkplaceController, BankConnectionHandler, EpostComponentViewHandler…

- beans: TaxAtSourceBean, AddressInformationBean, BankConnetionSetUpManagedBean…

- … or even an utility class to detect the data change, or to work with Date…: TaxAtSourceChangeDetector, DateUtil

The questions are:

- Why do models violate Possible God Class Rule? for instance why are they too big or too complicated? …

- Bean classes play as the model and controller of MVC.

  - Is it a good idea to merge “model” and “controller” into one class?

  - Why are the controller so big and complicated? Is it because of we implement business logic on front end?

### Problem

- Business code in pure Java class: VAT Amount?

- hardcode in xhtml. Then, the xhtml might need to be changed more frequently.

- Not follow best practice as implementing MVC model E.g. the implementation of Primefaces. Then, the related code need to be changed more frequently.  
  Bean handles both functionality of controller and service

### Solution

- Try to use the dedicated method/api instead of hardcoding. Then, in case the logic for this change the xhtml doesn’t need to be changed.

- Follow strictly the MVC model. Please refer to the implementation of Primefaces as an example.

### 3. Working with File

Normally, on Ivy side, we call Rest APIs to gather data, then generate file.

The big disadvantage of this approach is we need a lot of memory.

### Solution/Suggestion

We should consider the alternative approach, that we stream the file from backend to end user directly.
