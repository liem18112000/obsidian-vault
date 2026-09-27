---
title: "Research on Delete Access class"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/TP2020/pages/47108654097/Research+on+Delete+Access+class
space: "TP2020"
topic: programming
relevance: 0.863
depth: 3
updated: 2022-05-12
attachments: 1
tags:
  - confluence
  - programming
  - space/tp2020
---

# Research on Delete Access class

> [!info] Imported from Confluence
> Space **TP2020** · updated 2022-05-12 · [open original](https://axonivy.atlassian.net/wiki/spaces/TP2020/pages/47108654097/Research+on+Delete+Access+class)
> Relevance 0.863 · topic `programming`

<div class="toc-macro client-side-toc-macro conf-macro output-block" cssliststyle="none" hasbody="false" headerelements="H1,H2,H3,H4,H5,H6,H7" macro-id="57b18fcf055a97238bf8701c0d7baa95" macro-name="toc">

</div>

## I. Discussion

### 1. The impact of deleting security classes

#### 1.1. To other modules

Security classes are managed centrally by `luztenant_service` so that many different modules besides `luz_docs` could reuse the APIs.

=\> If we implement a simple solution to delete the security class, other modules will not aware of the deletion, then an error will occur.  
  
Example of error: A security class is marked deleted. Some letters still have that class, but cannot query detail info (e.g. assigned persons) =\> not found error.

Pubsub document: <a href="https://cloud.google.com/pubsub/docs/reference/libraries#windows" class="external-link" data-card-appearance="inline" rel="nofollow">https://cloud.google.com/pubsub/docs/reference/libraries#windows</a> , <a href="https://github.com/googleapis/java-pubsub" class="external-link" data-card-appearance="inline" rel="nofollow">https://github.com/googleapis/java-pubsub</a>

Implemented team: Kepler (luz_docs, luz_audit), Helios, Wow

**Potential solutions:**

1.  Publish the deletion topic in `luztenant_service`, every module that depends on it would subscribe to that topic.  
    Then the modules can trigger a background process to adapt to the deletion (soft delete).

    1.  Advantages:

        1.  The user just deletes the class, and background processes will handle the remaining

        2.  `luztenant_service` need not be updated when a new module starts using the security class

    2.  Disadvantages:

        1.  T.B.D.

2.  Check existing usage of Security Class in `luz_docs` and other modules:

    1.  Advantage: Ensure no further error because all dependencies are resolved.

    2.  Disadvantages

        1.  user need to remove access class usage manually

        2.  in `luztenant_service`, have to check existing usage of security classes

        3.  need to update `luztenant_service` checking everytime there is a new place using the Security Class such as `folder class` (<a href="https://www.figma.com/file/kMudzCqJjtw5gRsgpmRtxB/eArchive?node-id=171%3A74725" class="external-link" data-card-appearance="inline" rel="nofollow">https://www.figma.com/file/kMudzCqJjtw5gRsgpmRtxB/eArchive?node-id=171%3A74725</a>) or `luz_accounting`

3.  ~~Check on UI, then remove not found classes~~:  
    (not applicable because this solution makes every UI in the future need to check if a security class is valid)

4.  ~~Besides that, we can think of some workarounds to remove the assigned classes from letters before deleting them.~~  
    (not applicable because this idea has a drawback in that other systems will not be aware of the deletion)

#### 1.2. For auditing purposes

They identified the following features as essential:

- History: They needed the ability to tie every action taken with a specific person in order to illustrate accountability to auditors.

- Compliance: The solution would need auditing and reporting capabilities to prove an existing secure access methodology and adherence to multiple countries’ privacy laws.

- Centralized management: The solution would need to centralize discovery, management, and user administration.

- Least Privilege: The solution would need to provide just enough privilege, granted just-in-time, and for a limited time only.

### 2. Performance

#### What can make it slow?

- **The number of target items** (Number of letters having assigned classes could be large)

  -  there could be hundreds or thousands of assigned letters

 

- **The number of actions** (Number of API calls could be large)

  - there could be hundreds of API requests if there was no single API that can handle <u>multiple patches</u>

  - the cost of trying and retrying in case of many failures during the update

 

- **The time is taken for each action** (Complexity of implementation)

  - The flow

    - Accessing database / other API multiple times (ideally 1 time each action)

    - Rest calls from far containers might take more time than REST calls from the nearby containers

    - Implementing rollback / undo / resume mechanism

    - Redundant calls

  - The algorithm

    - Query none indexed item (letters that contain a particular security class)

#### How long the actors could bear the slowness?

- User: 3s -\> 10s

- Services (a few hundreds of seconds before request timeout exception occurs)

  - luz_docs_view_controller

  - luz_docs

  - luztenant_service

### 3. Consistency while we delete the security class

- Do we need to block UI when deleting the security class?

- The process will unassign a security class and then delete this security class in the DB. Is it the best way?

- when unassign security class fails:

  - Do we need to continue to delete this security class in DB?

  - How about the metadata of letters that have been unassigned security class?

- During this time user deletes a security class “SC“ and another user assigns this security class “SC” into the letter. What happens?

### 4. Data integrity

### 5. Future usage of Security class

Folder class: <a href="https://www.figma.com/file/kMudzCqJjtw5gRsgpmRtxB/eArchive?node-id=171%3A74725" class="external-link" data-card-appearance="inline" rel="nofollow">https://www.figma.com/file/kMudzCqJjtw5gRsgpmRtxB/eArchive?node-id=171%3A74725</a>


![[47108654097-image-20220512-042339.png]]



## II. Solution
