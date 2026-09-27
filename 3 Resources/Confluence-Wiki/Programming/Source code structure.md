---
title: "Source code structure"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/GRAVITY/pages/47130218714/Source+code+structure
space: "GRAVITY"
topic: programming
relevance: 0.714
depth: 2.54
updated: 2022-06-17
attachments: 11
tags:
  - confluence
  - programming
  - space/gravity
---

# Source code structure

> [!info] Imported from Confluence
> Space **GRAVITY** · updated 2022-06-17 · [open original](https://axonivy.atlassian.net/wiki/spaces/GRAVITY/pages/47130218714/Source+code+structure)
> Relevance 0.714 · topic `programming`

<div class="toc-macro client-side-toc-macro conf-macro output-block" hasbody="false" headerelements="H1,H2,H3,H4,H5,H6,H7" macro-id="a9abefdc-edbc-4a05-8a45-7abb8f3972cb" macro-name="toc">

</div>

1.   <a href="#" class="unresolved">main package</a>: core source for framework
2.   <a href="#" class="unresolved">test package</a>: more implementations for the specific project (exp: current is Valiant)

  

                                                    

![[47130218714-srcAssociation.PNG]]



  

  

# Main Package

Includes 4 sub-packages:

- [managers](#Sourcecodestructure-managerssub-package)
- [elements](#Sourcecodestructure-elementssub-package)
- [enums](#Sourcecodestructure-enumssub-package)
- [utilities](#Sourcecodestructure-utilitiessub-package)

                                         

![[47130218714-mainPackage1.PNG]]



## managers sub-package

                                                                               

![[47130218714-mainPackage_managers.PNG]]



## utilities sub-package

                                                 

![[47130218714-mainPackage_utilities.PNG]]



  

                                                     

![[47130218714-mainPackage_utilities_dataReader.PNG]]



## enums sub-package

                                                               

![[47130218714-mainPackage_enums.PNG]]



## elements sub-package

  

                                                                  

![[47130218714-mainPackage_elements.png]]



  

# Test Package

 Including:<a href="#" class="unresolved">4753211411</a> and resources sub-packages

                                                                                      

![[47130218714-testPackage.PNG]]



  

## java sub-packages

 - **myRunner** includes all files that written to execute automated testscripts matched "@CucumberOptions()"

                                                                                    

![[47130218714-testPackage_java_myRunner.PNG]]



  

 - **models** stores all defined objects relate to business which are used in automation as: AdditionalSecurity, Dossier, ...

                                                       

![[47130218714-testPackage_java_models_1.PNG]]



  

                                                       

![[47130218714-testPackage_java_models_3.PNG]]



  

 - **pages** stores all the locators, page and functions of web element
