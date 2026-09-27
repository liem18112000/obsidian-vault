---
title: "02_30 Pull request and review code orally transmitted secrets"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/GRAVITY/pages/47134409478/02_30+Pull+request+and+review+code+orally+transmitted+secrets
space: "GRAVITY"
topic: programming
relevance: 0.703
depth: 2.38
updated: 2022-06-24
attachments: 0
tags:
  - confluence
  - programming
  - space/gravity
---

# 02_30 Pull request and review code orally transmitted secrets

> [!info] Imported from Confluence
> Space **GRAVITY** · updated 2022-06-24 · [open original](https://axonivy.atlassian.net/wiki/spaces/GRAVITY/pages/47134409478/02_30+Pull+request+and+review+code+orally+transmitted+secrets)
> Relevance 0.703 · topic `programming`

# **Pull Request**

### <span class="placeholder-inline-tasks">1. Put "why" code comment as a reason inside class’s existence</span>

When you write a new feature, you have a lot of information about it: requirements, limitations of 3rd-party systems, interactions with legacy codebase — you write code which takes all of it into account.

However, when somebody reads that code, they don’t have that context, so they ask “Why is it here?” and “Why did you pick this approach?”.

Give them the answer to that “why” in advance, by adding explanatory comments.

Example:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="16e6dc1a-42fd-427f-8281-a52c2a080f39" macro-name="code" style="border-width: 1px;">

<div class="codeHeader panelHeader pdl" style="border-bottom-width: 1px;">

**pr-1**

</div>

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
/**
 * First Crew Dragon launch was postponed due to bad weather, 
 * and now we need an event for the "second" first launch. 
 * Hence the stupid name.
 */
public class SecondFirstCrewDragonLaunch {
    // your code...
}
```

</div>

</div>

### 2. Make Your PRs Small

Small PRs are:

- Reviewed more thoroughly.
- Reviewed more quickly.
- Easier to merge (frequent merges lead to fewer conflicts).
- Less wasted work if rejected.

Some ideas to make it easier to make small PRs:

- Extract refactoring to a separate PR.
- Split big features into parts.
- Using  `git add --patch` and `git rebase --interactive`
- In case of long-running feature branches, set that branch as a target for your PRs instead of master.

### 3. Make a Clear Description

- A link to the ticket.
- A summary of what has been done (if it’s not obvious from PR’s title).
- Links to related pull requests (for example, related changes in another service).

### 4. Comment Your Own Pull Request

Consider commenting your own pull request to give additional context to reviewers

### 5. Discuss the Overall Approach Before Implementing the Whole Feature

This is a huge time-saver. When you’re about to start work on<span class="legacy-color-text-red2"> **a larger refactoring or feature**</span>, discuss your approach with colleagues beforehand.

Create a chat with several fellow developers, explain the task and your idea. They might approve your approach, or suggest a better one.

### 6. Rebase Onto Fresh Master Before Creating a PR (<span class="legacy-color-text-red2">still</span> <span class="legacy-color-text-red2">non-official, considering...</span>)

- Tests might pass in your local branch, but fail with latest updates applied.
- You would be able to use recently added functionality (a new utility class, for example).
- Reviewers could be confused if they don’t find recent changes.

# **Code Reviews**

### 1. Dependencies

- When was it last updated?
- Who maintains it?
- What are others experiences?

### 2. Organization

- What design patterns do they use?
- Is the application design consistent?
- Is the system easily testable? i.e. Controllable & Observable components

### 3. Recommendations

- How can they improve the reliability?
- How can they improve the maintainability and efficiency?
- What is my overall assessment of the project?
