---
title: "ePost AI Solution Concept"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/AI/pages/47075754264/ePost+AI+Solution+Concept
space: "AI"
topic: architecture
relevance: 0.769
depth: 2.79
updated: 2022-03-28
attachments: 2
tags:
  - confluence
  - architecture
  - space/ai
---

# ePost AI Solution Concept

> [!info] Imported from Confluence
> Space **AI** · updated 2022-03-28 · [open original](https://axonivy.atlassian.net/wiki/spaces/AI/pages/47075754264/ePost+AI+Solution+Concept)
> Relevance 0.769 · topic `architecture`

<div class="contentLayout2">

<div class="columnLayout two-right-sidebar">

<div class="cell normal" data-type="normal">

<div class="innerCell">

# Management Summary

Artificial Intelligence (AI) powers some of the most magical experiences in the ePost product. I<span class="inline-comment-marker" ref="ea832f36-68fe-4f31-a078-6ffea18acf85">t helps organize mail, assists taking necessary actions, and much more.</span> AI learns these skills by studying vast numbers of examples. The more examples it has, the better it gets.

With ePost, mail is kept private. It meets the highest Swiss Post IT and data security standards and can only be read by the person who owns it.

But how can we gain data to let AI learn skills and still keep the data private?

## Crowdsourcing

Our customers knows their data best. We therefore let our users annotate their own data and keep the data private all the time.

We see two variants of contribution. An implicit way, that lets the user contribute as a side effect of its actions, and by an opt-in for explicit contribution through gamification.

<div class="panel conf-macro output-block" hasbody="true" macro-id="" macro-name="panel" style="background-color: #EAE6FF;border-color: #998DD9;border-width: 1px;">

<div class="panelContent" style="background-color: #EAE6FF;">

The app guides the users to achieve a specific goal and as a side effect the data becomes annotated.

</div>

</div>

> E.g. The app tells the user that it was not able to identify the title of a document. If the user wants, he gets guided to select the title in the document. Afterwards we document has got a title, and the app knows even the exact location.

<div class="panel conf-macro output-block" hasbody="true" macro-id="" macro-name="panel" style="background-color: #EAE6FF;border-color: #998DD9;border-width: 1px;">

<div class="panelContent" style="background-color: #EAE6FF;">

The user can opt-in to help improve ePost even more. They then receive simple questions about their mails, and earn badges and level up. This functionality could be provided in a separate app.

</div>

</div>

</div>

</div>

<div class="cell aside" data-type="aside">

<div class="innerCell">

<div class="toc-macro client-side-toc-macro conf-macro output-block" hasbody="false" headerelements="H1,H2" macro-id="e69e554a0b0a3ba4d4f1250e6cde9d2f" macro-name="toc">

</div>

</div>

</div>

</div>

<div class="columnLayout fixed-width">

<div class="cell normal" data-type="normal">

<div class="innerCell">

## Keep data private

Due to privacy reasons we need explicit permission to process sensitive customer data.

<span class="inline-comment-marker" ref="9813b177-6fb3-44cc-a2d1-d3be02b93b81">To bootstrap a new AI skill </span>**<span class="inline-comment-marker" ref="9813b177-6fb3-44cc-a2d1-d3be02b93b81">we need access to data for research purposes</span>**<span class="inline-comment-marker" ref="9813b177-6fb3-44cc-a2d1-d3be02b93b81">. We must be able to explore, examine, and visualize data in order to specify the requirements and to crowdsource data labeling. We also need feedback from users in combination with raw data to examine problems</span>.

ePost App is allowed to send data to Analyze API. We therefore assume that the App is allowed to send data to another API for model training purposes too. We must be able to **organize data in different logical collections** to make model training reproducible. We also need the ability to **<span class="inline-comment-marker" ref="f48c1cf6-0ca5-482c-a9e0-dc005f432aae">collect and store</span>** customer data in order to **increase reachability**.

<div class="panel conf-macro output-block" hasbody="true" macro-id="" macro-name="panel" style="background-color: #EAE6FF;border-color: #998DD9;border-width: 1px;">

<div class="panelContent" style="background-color: #EAE6FF;">

Register a script that runs triggered by a specific event (exactly once on login, on update of field x). That script is allowed to extract, transform and load data to a target system.

</div>

</div>

> E.g. If user changes the title, then send corresponding page with annotated title to the target system.

For research purposes, data must be in raw format.

Later, exported data could be a vectorized representation.

In future, we might be able to build models directly on the basis of encrypted data.

The ability to deploy a script must be secured by an approval and signing flow.

## Proof feasibility of data

We must be able to **validate** that enough data exists that fulfills the requirements. The same capability allows us to **analyze** the **data landscape** to support the process of product feature development and to proof business strategy.

<div class="panel conf-macro output-block" hasbody="true" macro-id="" macro-name="panel" style="background-color: #EAE6FF;border-color: #998DD9;border-width: 1px;">

<div class="panelContent" style="background-color: #EAE6FF;">

Register a script that runs triggered by a specific event (exactly once on login, on update of field x). That script runs on behalf on the user is allowed to query it’s own data and to send results to a target system.

</div>

</div>

> E.g. How many documents are paid invoices?

The ability to deploy a script must be secured by an approval and signing flow.

# Visualization

Abstract illustration of needed data flow for<span class="inline-comment-marker" ref="6d93bc79-1ff8-4485-b446-c409803a0eec"> building a static mode</span>l.


![[47075754264-process-overview.png]]



# Ideas of “AI skills”

Below you can find an incomplete catalog of potential use cases.

**Document title** – Extract title of a document.

**Abstract** – Generate a summary of a document.

**Document tags** – Tag a document with meaningful keywords. Every user should own it’s own model, trained based on his input. Similar to a spam filters and mobile keyboard input suggestions.

**Follow-up reminders** – Suggest a date to remind user to complete a task. The model might need to learn the event date (e.g. payment due date) and the date when the notification should appear with respect to the event date (e.g. one day before).

**Document type** – Whether the document is a receipt, a bill, a contract, etc.

# “AI Skill” Life Cycle Model

## Concept definition

The first step is understanding the business requirements and the problem we're trying to solve. The goals should be related to the business objectives and <span class="inline-comment-marker" ref="f2d8859a-f647-4125-855f-eef10a3aabb5">not </span>to machine learning.

### Business requirements

- Who is the actor?

- What is the (final) outcome?

- What steps are needed to achieve the goal?

- Who (else) has an interest in the outcome?

- What are the triggers?

- Are there any preconditions?

- What happens if things go wrong?

### Model requirements

- What steps are cognitive / AI model based?

- What are the success criteria?

- Are there any special requirements for transparency, explainability, or bias reduction?

- What are the acceptable parameters for accuracy, precision, recall, etc?

- What are the expected inputs to the model and the expected outputs?

- What are the characteristics of the problem being solved (classification, regression, clustering, etc)?

- What is the "heuristic" -- the quick-and-dirty approach to solving the problem that doesn't require machine learning? <span class="inline-comment-marker" ref="dc90fcc9-4085-4f42-a0f0-62084816e091">How much better than the heuristic does the model need to be?</span>

### System requirements

- How will the model be trained?

  - online, as data comes in

  - offline, having a static data set (in iterations with versions of it deployed periodically)

  - federated (collaborative), across multiple decentralized systems

- How will the model operate?

  - used offline

  - operate in batch mode on data that's fed in and processed asynchronously

  - <span class="inline-comment-marker" ref="8f362ac5-6636-4118-bd11-992b523d14c8">used in real time,</span> operating with high-performance requirements to provide instant results

- Where will the model be trained or used?

  - On powerful servers

  - On consumer device

## Identify data needs

Identify our data needs and determine whether the data is in proper shape for the machine learning project. The focus should be on data identification, initial collection, requirements, quality identification, insights and potentially interesting aspects that are worth further investigation.

- What kind of annotations and labels (features) are needed?

- How can we identify relevant, accurate, legitimate, reliable, and clean data?

- How can we ensure maximum variety of the data?

- What quantity of data is needed to train (and validate and test) the system?

- What is the current quantity and quality of the training data?

## Collect data

Collecting and preparing data will take a substantial amount of time. Since machine learning models need to learn from data, the amount of time spent on collecting, labeling, preparing, and cleansing is well worth it.

**Data labeling** is the process of enriching raw data with metadata so that it can be used to learn an AI skill.

**Data preparation and cleansing** is the process of making the data suitable for machine learning. High quality data is needed for get a high quality model. The use of unverified data is one of the most common mistakes machine learning engineers do in AI developments.

- <span class="inline-comment-marker" ref="a40ed573-7513-4c1c-b9dc-5e55417f79b5">Remove extraneous information and de-duplicate</span>

- Remove irrelevant data from training to improve results

- Reduce noise and remove ambiguity

- Remove incorrect data

- Enhance data

- Balance data to not be biased, e.g. by specific users, senders, etc

- Select features that identify the most important dimensions and, if necessary, reduce dimensions

- Split data into training, test and validation sets

## Proof feasibility

Validate that the available data can support the requirements.

- What is the **current quantity and quality** of the training data?

**Establish a baseline model** or heuristic.

## Build

This phase requires model technique selection and application, model **train**ing, model hyper-parameter setting and adjustment, model validation, ensemble model development and **test**ing, algorithm selection, and model optimization.

**Evaluate** the model’s performance and **compare** the result to the baseline model (or heuristic).

## Deploy

As soon as the model can work in the real world, it's time to see how it actually operates.

We need to be able to deploy the model within a closed, controlled group. This includes the capability to do staging in development and production environments.

We need to be able to **continually measure and monitor** a model’s performance.

## Improve

Start small, think big and iterate often.

Find solutions to "model drift" or "data drift," which can cause changes in performance due to changes in real-world data.

# Glossary

<div>

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr>
<th><p><strong>Term</strong></p></th>
<th><p><strong>Description</strong></p></th>
</tr>
&#10;<tr>
<td><p>Annotated data</p></td>
<td><p>Raw data enriched (labeled) with metadata so that the machine can recognize, understand and memorize it.</p>
<p>Examples:</p>
<ul>
<li><p>highlighted text with the meaning ‘title’ added</p></li>
<li><p>a document with added tag ‘Contract’</p></li>
</ul></td>
</tr>
<tr>
<td><p>Bootstrapping</p></td>
<td><p>To build a first valuable model (AI skill).</p></td>
</tr>
<tr>
<td><p>Ground truth</p></td>
<td><p><code>Annotated data</code> that is used to train, validate and test a model.</p></td>
</tr>
<tr>
<td><p>Human in the Loop (HITL)</p></td>
<td><p>Constant supervision and validation of the AI model’s results by a <span class="inline-comment-marker" data-ref="6ad35d35-6cbf-4ed0-a7b7-508d5e71a08d">human</span>.</p>
<p>There are two main ways in which humans become part of the machine learning loop:</p>
<ol>
<li><p>Labeling training data</p></li>
<li><p>Validating predictions</p></li>
</ol>
<p>Examples:</p>
<ul>
<li><p>User is asked whether or not the extracted title is correct</p></li>
</ul></td>
</tr>
<tr>
<td><p>Training data</p></td>
<td><p><code>Annotated data</code> that has been collected to be fed to a machine learning model to help the model learn more about the data.</p></td>
</tr>
</tbody>
</table>

</div>

# Privacy

How do the big five handle it?

## Microsoft

<a href="https://privacy.microsoft.com/en-us/privacystatement" class="external-link" data-card-appearance="inline" rel="nofollow">https://privacy.microsoft.com/en-us/privacystatement</a>

> **Our processing of personal data for these purposes includes both automated and manual (human) methods of processing. Our automated methods often are related to and supported by our manual methods.** \[…\] This manual review may be conducted by Microsoft employees or vendors who are working on Microsoft’s behalf.

## Google

<a href="https://policies.google.com/privacy?hl=en" class="external-link" data-card-appearance="inline" rel="nofollow">https://policies.google.com/privacy?hl=en</a>

> We also collect the content you create, upload, or receive from others when using our services. This includes things like email you write and receive, photos and videos you save, docs and spreadsheets you create, and comments you make on YouTube videos.
>
> \[…\] We use the information we collect in existing services to help us develop new ones. For example, understanding how people organized their photos in Picasa, Google’s first photos app, helped us design and launch Google Photos.

## Amazon

<a href="https://www.amazon.com/gp/help/customer/display.html?nodeId=GX7NJQ4ZB8MHFRNJ" class="external-link" rel="nofollow">https://www.amazon.com/gp/help/customer/display.html?nodeId=GX7NJQ4ZB8MHFRNJ</a>

> We use your personal information to recommend features, products, and services that might be of interest to you, identify your preferences, and personalize your experience \[…\]
>
> \[…\] When you use our voice, image and camera services, we use your voice input, images, videos, and other personal information to respond to your requests, provide the requested service to you, and improve our services.

# Future Topics

## Homomorphic Encryption

Homomorphic Encryption provides the ability to compute on data while the data is encrypted. This ground-breaking technology has enabled industry and government to provide never-before enabled capabilities for outsourced computation securely.

More information can be found on <a href="https://homomorphicencryption.org/" class="external-link" data-card-appearance="inline" rel="nofollow">https://homomorphicencryption.org/</a>.

</div>

</div>

</div>

</div>
