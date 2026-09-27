---
ai_hash: 70345f7878bafad8
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 23
depth: 2.69
entities: []
relevance: 0.721
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47627468825/Recipe+Github+copilot
space: LUZ
status: reference
tags:
- confluence
- ai-ml
- space/luz
title: 'Recipe: Github copilot'
topic: ai_ml
type: source
updated: 2024-10-24
---

# Recipe: Github copilot

> [!info] Imported from Confluence
> Space **LUZ** · updated 2024-10-24 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47627468825/Recipe+Github+copilot)
> Relevance 0.721 · topic `ai_ml`

<div class="toc-macro client-side-toc-macro non-printable conf-macro output-block" hasbody="false" headerelements="H1,H2,H3,H4,H5,H6" macro-id="9ffecde0-4e71-4b49-ac73-76e6735db66a" macro-name="toc" numberedoutline="false" structure="list">

</div>

# Sign up for Github copilot

1\. Go to <a href="https://github.com/join" class="external-link" rel="nofollow">https://github.com/join</a> to create a github account using your company email (@axonactive.com, or @epostservice.ch, or @avdevs.com)

For the username, please follow this format: \<your-full-name\>-\<your-team\>-\<your-company\>  
Examples: NguyenVanA-wow-aavn, shama-optimus-avdevs, JohnMoser-invisible-klara

If you made a mistake in your username, change it in the settings after you have created the account:


![[47627468825-image_2024_01_05T08_46_14_611Z.png]]



Note: If you’re failed at the step of verification. Then, you should try to use **VPN** because maybe something has been blocked.

2\. After creating a Github account using your company email, send an email to <a href="mailto:nam.nguyen@axonactive.com" class="external-link" rel="nofollow">nam.nguyen@axonactive.com</a> requesting to join use Github copilot.

3\. You should then receive an email to join KLARA organization on Github. Check carefully the link and click to join.


![[47627468825-image-20240105-054146.png]]



4\. After joining the KLARA organization, inform <a href="mailto:nam.nguyen@axonactive.com" class="external-link" rel="nofollow">nam.nguyen@axonactive.com</a> so that he can enable Github copilot for you.  
If Github copilot is enabled for you, you should receive these emails:

<div>

|  |  |  |
|----|----|----|
| 

![[47627468825-image-20240105-054110.png]]

 | 

![[47627468825-image-20240105-054121.png]]

 | 

![[47627468825-image-20240105-054132.png]]

 |

</div>

# Integrate Github copilot into IDEs

**This confluence is showing how to integrate Github copilot in Visual studio code and Intellij.**   
**If you are able to integrate Github copilot into your favorite IDEs, feel free to update them here.**

## For Visual studio code

<a href="https://docs.github.com/en/copilot/using-github-copilot/getting-started-with-github-copilot?tool=vscode" class="external-link" data-card-appearance="inline" rel="nofollow">https://docs.github.com/en/copilot/using-github-copilot/getting-started-with-github-copilot?tool=vscode</a>

Press F1 and type **Extensions: Focus on Extensions View**, or click on the search bar and type **\>Extensions: Focus on Extensions View.**  
Then search for Github copilot.

<div>

|  |  |
|----|----|
| **F1** | **Search bar** |
| <span class="confluence-embedded-file-wrapper image-center-wrapper confluence-embedded-manual-size">[[47627468825-f1.mp4|f1.mp4]]</span> | <span class="confluence-embedded-file-wrapper image-center-wrapper confluence-embedded-manual-size">[[47627468825-search-bar.mp4|search-bar.mp4]]</span> |

</div>

After installing the extension, authorized your github login for the extension:

<span class="confluence-embedded-file-wrapper image-left-wrapper confluence-embedded-manual-size">[[47627468825-vscode.mp4|vscode.mp4]]</span>

## For Intellij

<a href="https://docs.github.com/en/copilot/using-github-copilot/getting-started-with-github-copilot?tool=jetbrains#prerequisites" class="external-link" data-card-appearance="inline" rel="nofollow">https://docs.github.com/en/copilot/using-github-copilot/getting-started-with-github-copilot?tool=jetbrains#prerequisites</a>

<span class="confluence-embedded-file-wrapper image-left-wrapper confluence-embedded-manual-size">[[47627468825-intellij-plugin-install.mp4|intellij-plugin-install.mp4]]</span>

# Features

Below are features that we think are probably most useful to you in your daily work.

## Copilot chat

**~~For now, Intellij IDEs do not support Copilot chat for organization yet, only for individual subscription.~~**  
~~This page will be updated and informed when organization subscription copilot chat is supported for Intellij IDEs.~~


![[47627468825-image-20240118-022234.png]]



**~~To use Copilot chat, for now it is recommended to use Visual Studio code.~~**

Copilot chat is available for Visual studio code and Intellij.

You can use github copilot chat to do most of the functions that github copilot offer like code generation, explain code, refactor/simplify code, generate tests,  
and especially asking questions about a topic that you are researching about.

<div>

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr>
<th><p><strong>Features</strong></p></th>
<th><p><strong>How to</strong></p></th>
</tr>
&#10;<tr>
<td><p>Manage chat history</p></td>
<td><span class="confluence-embedded-file-wrapper image-left-wrapper confluence-embedded-manual-size"><a href="../_attachments/47627468825-chat-history.mp4">chat-history.mp4</a></span></td>
</tr>
<tr>
<td><p>Code simplify/refactor by referencing a code block</p></td>
<td><span class="confluence-embedded-file-wrapper image-left-wrapper confluence-embedded-manual-size"><a href="../_attachments/47627468825-npe-refactor.mp4">npe-refactor.mp4</a></span>
<span class="confluence-embedded-file-wrapper image-left-wrapper confluence-embedded-manual-size"><a href="../_attachments/47627468825-code-block-referencing.mp4">code-block-referencing.mp4</a></span></td>
</tr>
<tr>
<td><p>Understanding a project by referencing the opened workspace</p></td>
<td><span class="confluence-embedded-file-wrapper image-left-wrapper confluence-embedded-manual-size"><a href="../_attachments/47627468825-workspace-referencing.mp4">workspace-referencing.mp4</a></span></td>
</tr>
<tr>
<td><p>Ask a question about a topic that you want to research</p></td>
<td><span class="confluence-embedded-file-wrapper image-left-wrapper confluence-embedded-manual-size"><a href="../_attachments/47627468825-ask-topic-question.mp4">ask-topic-question.mp4</a></span></td>
</tr>
<tr>
<td><p>Generate tests</p></td>
<td><span class="confluence-embedded-file-wrapper image-left-wrapper confluence-embedded-manual-size"><a href="../_attachments/47627468825-chat-generate-tests.mp4">chat-generate-tests.mp4</a></span></td>
</tr>
</tbody>
</table>

</div>

### Tips for Copilot chat

**Tip: Start a new chat session frequently**

Keep using a chat session for too long with too many chats/questions, might result in copilot providing bad quality suggestions.  
So start a new chat session if you want to switch the topic, or want to restart the conversation.

**Tip: Take time to construct your questions/prompts**

The more detailed your question/prompt is, the higher chance that copilot will produce a high quality response.  
Here is an example format that you can follow.  
**Context**: "I'm a software developer"  
**Specific** Information: "working on a Python project"  
**Intent/Goal**: "Can you explain how to implement exception handling in Python?"  
**Response Format** (if needed): Write it in a simple paragraph or list.

**Perfect Prompt**: "I'm a software developer working on a Python project. Can you explain how to implement exception handling in Python? Write it in a simple paragraph or list.

### Inline chat

It is possible to use the copilot chat feature while you are in the editor, **but in our experience, it is not as powerful as the full copilot chat panel**.

<span class="confluence-embedded-file-wrapper image-left-wrapper confluence-embedded-manual-size">[[47627468825-vscode-inline-chat.mp4|vscode-inline-chat.mp4]]</span>

## Code suggestions

If you do not want to use copilot chat, maybe you are coding in the editor and don’t want to move your cursor around and select copilot chat,  
then you can command copilot what to do by adding a comment saying your requirements.  
Copilot will also try to predict and suggest your next lines of code.

This feature works both for Visual studio code and Intellij IDEA.

<div>

|  |  |
|----|----|
| **Visual studio code** | **IntelliJ** |
| <span class="confluence-embedded-file-wrapper image-left-wrapper confluence-embedded-manual-size">[[47627468825-code-suggestion.mp4|code-suggestion.mp4]]</span> | <span class="confluence-embedded-file-wrapper image-left-wrapper confluence-embedded-manual-size">[[47627468825-intellij-code-suggestion.mp4|intellij-code-suggestion.mp4]]</span> |

</div>

## Generate tests

You can use this feature by selecting the code block that you need to generate tests for.

<span class="confluence-embedded-file-wrapper image-left-wrapper confluence-embedded-manual-size">[[47627468825-generate-test.mp4|generate-test.mp4]]</span>

**But this feature is limited and we do not recommend to use it to generate test.**  
**Because it does not allow you to define what are your inputs, and what are the test expectations.**

If you want to generate test, just reference the code block/file in copilot chat,  
then you can define your inputs, and your result expectations.

<span class="confluence-embedded-file-wrapper image-left-wrapper confluence-embedded-manual-size">[[47627468825-chat-generate-tests.mp4|chat-generate-tests.mp4]]</span>

If you still want to learn more, visit <a href="https://docs.github.com/en/copilot/using-github-copilot/getting-started-with-github-copilot" class="external-link" data-card-appearance="inline" rel="nofollow">https://docs.github.com/en/copilot/using-github-copilot/getting-started-with-github-copilot</a>

# FAQs

**Q: Visual studio code is not my main IDE, I use IntelliJ.**  
**A: For now, we think it is hard to switch between Visual studio code and other IDE if you want to use copilot chat and other features.**  
Below is an example of a workflow for IntelliJ as main IDE and VS code.  
The scenario is writing tests for a method before implementing the production code.

<span class="confluence-embedded-file-wrapper image-left-wrapper confluence-embedded-manual-size">[[47627468825-workflow.mp4|workflow.mp4]]</span>

**Q: I am developing a feature on AxonIvy designer. Is it possible to make use of Github Copilot?**  
**A: Yes, you can open the AxonIvy project that you are working on in Visual studio code. The workflow should look just like the example above.**

**Q: There are some features described here that I cannot see in my machine, why is that?**  
**A: Make sure your IDEs and plugins are up-to-date. If the issues persist, contact team Future.**

**Q: How can I switch to another code suggestion?**  
**A:** `Alt + [` **or** `Alt + ]`

%% ai-graph-start %%

**Related notes:**
- [[Setup VS Code - Github Copilot - MCP Server]]
- [[MCP Servers — Installation and Configuration Reference]]
- [[Create a work space]]
- [[AI Tools overview]]
- [[Run GitHub Copilot CLI in a GitHub Actions workflow]]

%% ai-graph-end %%