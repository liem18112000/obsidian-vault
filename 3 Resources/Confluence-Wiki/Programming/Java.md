---
title: "Java"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/TS/pages/47097249927/Java
space: "TS"
topic: programming
relevance: 0.894
depth: 3
updated: 2022-04-21
attachments: 3
tags:
  - confluence
  - programming
  - space/ts
---

# Java

> [!info] Imported from Confluence
> Space **TS** · updated 2022-04-21 · [open original](https://axonivy.atlassian.net/wiki/spaces/TS/pages/47097249927/Java)
> Relevance 0.894 · topic `programming`

<div id="expander-1192838666" class="expand-container conf-macro output-block" hasbody="true" macro-id="4565e846-fec5-4200-b3e6-07f1508b86db" macro-name="expand">

<div id="expander-control-1192838666" class="expand-control">

<span class="expand-control-icon icon"> </span><span class="expand-control-text">Q: When & why we need to create public Constructor explicitly?</span>

</div>

<div id="expander-content-1192838666" class="expand-content expand-hidden">

## When?

- Java classes will be serialize, deserialize for transferring data between FE & BE.

## Why?

- Classes that are meant for serialization and deserialization should have a no-argument constructor (doesn't matter whether public or private).


![[47097249927-image-20220420-095441.png]]



### References:

- <a href="https://sites.google.com/site/gson/gson-user-guide" class="external-link" data-card-appearance="inline" rel="nofollow">https://sites.google.com/site/gson/gson-user-guide</a>

</div>

</div>

<div id="expander-1856478892" class="expand-container conf-macro output-block" hasbody="true" macro-id="083e35d1-52a0-44b7-9d04-22b3478db7cb" macro-name="expand">

<div id="expander-control-1856478892" class="expand-control">

<span class="expand-control-icon icon"> </span><span class="expand-control-text">Q: When & why we need create a private constructor?</span>

</div>

<div id="expander-content-1856478892" class="expand-content expand-hidden">

## When?

- We create classes that simply contain a collection of static methods. They are often **utils-classes** (Ex: UserUtils, StringUtils,…)

- These classes contain a couple of static utility methods and can't be instantiated due to the private constructor.

## Why?

- There's no need to allow object instantiation since static methods don't require an object instance to be used.


![[47097249927-image-20220421-031259.png]]

![[47097249927-image-20220421-074803.png]]



References:

- <a href="https://www.baeldung.com/java-private-constructors#:~:text=Private%20constructors%20allow%20us%20to,is%20known%20as%20constructor%20delegation." class="external-link" data-card-appearance="inline" rel="nofollow">https://www.baeldung.com/java-private-constructors#:~:text=Private%20constructors%20allow%20us%20to,is%20known%20as%20constructor%20delegation.</a>

</div>

</div>
