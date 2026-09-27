---
ai_hash: bb900b58c9d93977
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 6
depth: 2.68
entities: []
relevance: 0.796
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20496026835/iOS-RegioApp+Code+Architecture
space: LUZ
status: reference
tags:
- confluence
- programming
- space/luz
title: iOS-RegioApp Code Architecture
topic: programming
type: source
updated: 2019-09-19
---

# iOS-RegioApp Code Architecture

> [!info] Imported from Confluence
> Space **LUZ** · updated 2019-09-19 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20496026835/iOS-RegioApp+Code+Architecture)
> Relevance 0.796 · topic `programming`

RegioApp iOS is completely developed on Apple Native Framework for iOS, which is called **cocoa framework**. We are following MVC Architecture in the code to communicate with website and display data in to application and same for reverse. To start with detail explanation, lets us give you some overview of MVC architecture.  
  
**Model-View-Controller**

The Model-View-Controller (MVC) design pattern assigns objects in an application one of three roles: model, view, or controller. The pattern defines not only the roles objects play in the application, it defines the way objects communicate with each other. Each of the three types of objects is separated from the others by abstract boundaries and communicates with objects of the other types across those boundaries. The collection of objects of a certain MVC type in an application is sometimes referred to as a*layer*—for example, model layer.

MVC is central to a good design for a Cocoa application. The benefits of adopting this pattern are numerous. Many objects in these applications tend to be more reusable, and their interfaces tend to be better defined. Applications having an MVC design are also more easily extensible than other applications. Moreover, many Cocoa technologies and architectures are based on MVC and require that your custom objects play one of the MVC roles.

  


![[20496026835-iOS-MVC Explanation Diagram.png]]



  

**Project Folder Structures**

To begin with explanation, we would like to show you about the folder structure which we are following in to application. Here is the diagram for this:

  


![[20496026835-iOS-RegioApp Code Architecture.png]]



  

Now you have knowledge of project folder structure.  In the next step, we would like to take your attention to the next level of deeper diagram about how our classes are communicate with each other.  
  
**Classes Communication Diagram  **


![[20496026835-iOS Classes Connection Diagram.png]]

%% ai-graph-start %%

**Related notes:**
- _(none above threshold)_

%% ai-graph-end %%