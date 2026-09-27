---
title: "How to define an external interface"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/PT/pages/25328820262/How+to+define+an+external+interface
space: "PT"
topic: programming
relevance: 0.832
depth: 3
updated: 2019-04-29
attachments: 4
tags:
  - confluence
  - programming
  - space/pt
---

# How to define an external interface

> [!info] Imported from Confluence
> Space **PT** · updated 2019-04-29 · [open original](https://axonivy.atlassian.net/wiki/spaces/PT/pages/25328820262/How+to+define+an+external+interface)
> Relevance 0.832 · topic `programming`

![[25328820262-Interface definition.png]]



<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="3c5a9569-82f3-4ca4-a91c-5bbb76838e1f" macro-name="code" style="border-width: 1px;">

<div class="codeHeader panelHeader pdl" style="border-bottom-width: 1px;">

**ExternalInterfaceConfigurationLoader**

</div>

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
public static Injector loadInjector() {
        /*Get injector from file. In case there is no initial cache, it will read the config file which is defined by ${ch_axonivy_fintech_standard_configuration_path}/services.xml. 
        * Services.xml defines a list of interface binding, each interface binding contains 2 value: interface name and it's implementation. After this file load successful, the loader will create a google guice injector and put it to ivy cache in order to avoid reading file many times
        */      
    }
```

</div>

</div>

  

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="439b76ee-e55b-48ac-9542-7ec8efe0e454" macro-name="code" style="border-width: 1px;">

<div class="codeHeader panelHeader pdl" style="border-bottom-width: 1px;">

**services.xml**

</div>

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
<?xml version="1.0" encoding="UTF-8"?>
<externalInterfaceWrapper>    
    <interface>
        <name>ch.axonivy.fintech.standard.external.address.AddressCheckerService</name>
        <implementation>ch.axonivy.fintech.standard.external.address.MockAddressCheckerService</implementation>
    </interface> 
</externalInterfaceWrapper>
```

</div>

</div>

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="478d9c79-6323-46ca-8755-b6d6ecee6520" macro-name="code" style="border-width: 1px;">

<div class="codeHeader panelHeader pdl" style="border-bottom-width: 1px;">

**ExternalInterfaceService**

</div>

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
getInstance(Class<T> clazz){
    //Get the injector from ExternalInterfaceConfigurationLoader and initialize the instance for the input interface.
    //The external interface must extend this class in order to retrieve the bindinginstance.
}
```

</div>

</div>

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="71cb0844-f4ed-49f4-b33a-ecadec1a5de1" macro-name="code" style="border-width: 1px;">

<div class="codeHeader panelHeader pdl" style="border-bottom-width: 1px;">

**AddressCheckerService**

</div>

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
@ImplementedBy(DefaultAddressCheckerService.class)//This annotation allow us to get the default instance for this service in case of missing services.xml or services.xml doesn't define the binding
public abstract class AddressCheckerService extends ExternalInterfaceService {


    public static AddressCheckerService getInstance() {
        return getInstance(AddressCheckerService.class);
    }
    public Responce perform(){
    }




}
```

</div>

</div>

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="7545ad3f-e848-4d85-9438-4a7a1e235ece" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
How to call external interface:
AddressCheckerService service = AddressCheckerService.getInstance();//The returned instances depends on our configuration

```

</div>

</div>

## Remaining issue:


![[25328820262-image2019-4-25_14-54-30.png]]



  

After extract the current implementation for AddressCheck into abstract. we spot an issue that the abstract still depend on 2 dependencies which is specific for one implementation. We still need to refactor it, make the abstract interface as much as generic and doesn't depend on any specific implementation.
