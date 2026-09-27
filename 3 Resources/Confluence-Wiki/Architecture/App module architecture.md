---
title: "App module architecture"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20519724033/App+module+architecture
space: "LUZ"
topic: architecture
relevance: 0.855
depth: 3
updated: 2021-01-22
attachments: 11
tags:
  - confluence
  - architecture
  - space/luz
---

# App module architecture

> [!info] Imported from Confluence
> Space **LUZ** · updated 2021-01-22 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20519724033/App+module+architecture)
> Relevance 0.855 · topic `architecture`

## Problem description

The myLife and ePost app have similar features. The core behavior of the features do not differ. The features can differ in their appearance or some configuration settings on each app. Therefore, the architecture of the two apps must assure, that they are built on a modular and scalable approach. So that features are only developed once and not twice in parallel for each app.

  

For example. Below, there are two screenshoots of the menu structure of the two apps. There we see, that the module Digital letterbox and Archive are present on both apps. If we dive deeper into the two figma prototypes, we do see, that the functionalities and behavior are similar. The only difference is the style of the UI (e.g. color, shades, font, etc.). With this in mind we can conclude, that both apps will use the same modules. The goal of this document is to provide an architecture design and solution for this modular approach.

<div>

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr>
<th>ePost</th>
<th>KLARA myLife</th>
</tr>
&#10;<tr>
<td><div class="content-wrapper">

![[20519724033-image2020-12-15_14-45-2.png]]


</div></td>
<td><div class="content-wrapper">

![[20519724033-image2020-12-15_14-45-30.png]]


</div></td>
</tr>
</tbody>
</table>

</div>

  

## Problem solution

In order to solve the problem described above, we should consider the following requirements our architecture should have in order to have a solid modular approach:

- A module is defined as a complete feature. E.g. a module could be a scanner feature or a digital letterbox feature. Modules can differ in their size of code.
- A module must be easily added or removed from the ePost or myLife app.
- A module must have dedicated unit and integration tests. So that their business logic can be verified.
- A module must be able to connect to other modules to exchange data. E.g. a *Scanner* module must be able to store data in an *Archive* module.
- Dependencies between modules should be omitted as far as possible.
- Cycle dependencies between modules must be omitted.
- A module has a UI, which consists out of design components. Those components are based on a module specific configuration. E.g. design components might differ in color, style, size, etc. for each module.

#### Modules

- Registration / Login / Profile
- Digital letterbox
- Archive
- Scanner
- ... etc.

### Swift - iOS

From iOS side, while investigating module based architecture, we would like to inform you that it's possible and we can take benefits of Swift Package Manager(SPM) from Apple. Apple is having native tool for the use of custom libraries or combination of common used functions.It is called Swift Package Manger(SPM). With the help of SPM we can add our business logic & design system.

Here is our plan for modularisation:

Design system and module should be separate packages hosted on Bitbucket repo.

- **Design System:** Where all UI Components added with custom styling according to klara design. Its only one Package for all the native projects, which is compulsory to add in project before starting development.  
    
- **Module Based:** In this package we would like to cover all the business logic, input validation, RestAPI classes. eg:  if we talk about Login module, keycloak apis, textfield validation will be cover in module. 


![[20519724033-Design and Module separate.png]]



Source: <a href="https://swift.org/package-manager/" class="external-link" rel="nofollow">https://swift.org/package-manager/</a>

**Updating Library: **If we require any changes into the library then we can easily do that without too much hassle via SPM. You can add SPM as a local package into any current application and also able to check/test the changes just by simply running current application. Once you have completed changes then you have to commit & release a new version tag on repository. Other apps will take advantage of current library changes just by updating SPM version, that's it.

#### Benefits of Swift Package Manager:

\- We can connect multiple project with same logic dependency on single file.  
- SPM have a versioning system.So developer knows if it is old version or not in current project.  
- Developer have control over updating to latest version from xcode.  
- SPM gives us benefit to consume space of the code. In overall source project compare to old coding pattern.  
- On a base of safety standard no one can easily implement that without having Credentials(security key & Id) .  
- SPM also supports multiple platform like macOS, WatchOS, iPadOS & tvOS. So,In future we will have an advantage(as in not to built from scratch,We can use them easily).  
- We can add modules as per project requirement in a single project.

### **Kotlin - Android  **

- In Android we use MVVM Architecture. This architecture allows us to separate UI and business logic. So we can leverage this architecture and add all our business logic i.e. ViewModel classes in an library, so any changes in logic will be updated in library and by updating the library those changes will be reflected in both the apps.
- For eg if at first we have a filter logic where we can filter list and in future if we want to add a sorting functionality, remove certain filters or change how certain filters behave we just have to add, remove or change logic in the ViewModel class from our library and it will automatically reflect in both the apps when we update the library in those projects.
- For UI part we can make components styles in the library and when we use them in our app, we can override those styles in the individual projects as per our requirements.
- For eg: we can create a component for a button to have certain height, width, shadow, color, etc. and we can directly call the button in our apps from the library but if we have a case such that in mylife app we want color of buttons to be aubergine and for epost we want it to be yellow in this case we can override the style in their respective projects and change color, once that is done the button will take all other attributes from the library and color will be taken from local project


![[20519724033-approach1.png]]



  

- <span class="legacy-color-text-default">The shared library is a part of MyLife app. Since they both a part of same project, both are stored on axonivy-prod Bitbucket as shown in diagram above.</span>  
- <span class="legacy-color-text-default">ePost will implement the shared library as a Maven dependency.</span>
- <span class="legacy-color-text-default">CI/CD implementation will be implemented with minor changes.</span>

<span class="legacy-color-text-default">Another approach suggested was <a href="https://stackoverflow.com/a/31366602" class="external-link" rel="nofollow" style="text-decoration: none;text-align: left;">https://stackoverflow.com/a/31366602</a> . We are working for CI/CD integration.</span>
