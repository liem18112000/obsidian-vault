---
title: "Swagger UI"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20474827711/Swagger+UI
space: "LUZ"
topic: programming
relevance: 0.753
depth: 2.49
updated: 2021-09-06
attachments: 7
tags:
  - confluence
  - programming
  - space/luz
---

# Swagger UI

> [!info] Imported from Confluence
> Space **LUZ** · updated 2021-09-06 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20474827711/Swagger+UI)
> Relevance 0.753 · topic `programming`

## Problems

At the moment, each JEE module has to implement itself the Swagger-UI in order to display the Rest API on GUI.

This is not good because it just for developer only, but we have to include them to the package and deploy on the server. There are a lot of hardware resource hungry.

Andit is not good also when the developers copy and paste the Swagger-UI for all JEE modules. 

## Idea

Move the Swagger-UI to a separated module. The developers can explore the Rest APIs of any JEE module they want.

  

## Step-by-step guide

1.  check-out the project luz_api_explore and deploy it. (link bitbucket: <a href="https://bitbucket.org/axonivy-prod/luz_api_explore/src/master/" class="external-link" rel="nofollow">https://bitbucket.org/axonivy-prod/luz_api_explore/src/master/</a>)
2.  enter the url of the JEE module that you want to explore, and the token if needed. You can explore from any host.

  


![[20474827711-luz_api_explore.png]]



  

## Next step

Remove the swagger-ui stuff in all JEE projects. **Need some short actions from each teams\***.

What we remove:


![[20474827711-remove_swagger_ui.png]]



## Explore APIs on local by api-forwarder

### Generate your token

Import this to your local postman: 

1.  Running json: [[20474827711-dev.tech.postman_collection.json|dev.tech.postman_collection.json]],  
    

![[20474827711-image2021-9-6_20-11-16.png]]

  
      
2.  Environment: [[20474827711-dev.tech.postman_environment.json|dev.tech.postman_environment.json]]  
    Change your username as email and password.  
      
3.  Run from step 1 to 4 and copy the last token  
    

![[20474827711-image2021-9-6_19-48-10.png]]

  
      

### Run port-forward all

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="e5262492-a04f-4cbc-b2ba-fcdaed0daf6a" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
kubectl port-forward services/api-forwarder 8080:8080 -n dev-vn
```

</div>

</div>

### Get access


![[20474827711-image2021-9-6_20-16-10.png]]
