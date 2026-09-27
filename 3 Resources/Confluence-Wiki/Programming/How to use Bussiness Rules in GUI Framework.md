---
title: "How to use Bussiness Rules in GUI Framework"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/PT/pages/25287335453/How+to+use+Bussiness+Rules+in+GUI+Framework
space: "PT"
topic: programming
relevance: 0.731
depth: 2.73
updated: 2017-03-27
attachments: 8
tags:
  - confluence
  - programming
  - space/pt
---

# How to use Bussiness Rules in GUI Framework

> [!info] Imported from Confluence
> Space **PT** · updated 2017-03-27 · [open original](https://axonivy.atlassian.net/wiki/spaces/PT/pages/25287335453/How+to+use+Bussiness+Rules+in+GUI+Framework)
> Relevance 0.731 · topic `programming`

## Require: How to use bussiness rule(Gui framework) in Fintech projects 

## Solution  you must follow exactly these steps:

#### Step 1: Create **guiframework-config.xml**<span style="font-size: 14.0px;"> under </span>**/webContent**<span style="font-size: 14.0px;"> folder:</span>

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="c54bbac6-0c59-434f-9071-db4403de714d" macro-name="code" style="border-width: 1px;">

<div class="codeHeader panelHeader pdl" style="border-bottom-width: 1px;">

**guiframework-config**

</div>

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<config>
    <debugger>
        <showingRuleKeyOnView>true</showingRuleKeyOnView>
    </debugger>
    <dataObserver enable="true">
        <autoResetChangeFlag>true</autoResetChangeFlag>
        <notifyChange>
            <listeners>
                <listener
                    class="ch.axonaviy.guidemo.services.WorkingDateValueChangeListener"
                    for="ContextBasedRules.dg.accountHolder.person.workingDay" type="PreRenderView">
                    <params>
                        <param key="RULE_NAMESPACE" value="BusinessRules.person" />
                    </params>
                </listener>
            </listeners>
        </notifyChange>
    </dataObserver>
</config>
```

</div>

</div>

In this file, we defined one listener listens on **ContextBasedRules.dg.accountHolder.person.workingDay** (**WorkingDateValueChangeListener** was executed whenever the user changes value for **ContextBasedRules.dg.accountHolder.person.workingDay**), and we pass a package rule as a param as well

#### Step 2: Create  a class **WorkingDateValueChangeListener** implements **NotifyValueChangeEventListener**

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="57d38c80-39db-48dd-9062-9c8d4116a60e" macro-name="code" style="border-width: 1px;">

<div class="codeHeader panelHeader pdl" style="border-bottom-width: 1px;">

**WorkingDateValueChangeListener**

</div>

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
package ch.axonaviy.guidemo.services;

import gui.framework.Dossier;

import java.util.List;
import java.util.Map;

import javax.faces.context.FacesContext;
import javax.faces.event.ValueChangeEvent;

import org.primefaces.context.RequestContext;

import ch.axonivy.fintech.standard.guiframework.bean.BusinessRuleDto;
import ch.axonivy.fintech.standard.guiframework.changedobserver.eventlistener.NotifyValueChangeEventListener;
import ch.axonivy.fintech.standard.guiframework.converter.BusinessRuleDtoConverter;
import ch.axonivy.fintech.standard.guiframework.eventlistener.helper.ValueChangeEventListenerHelper;
import ch.axonivy.fintech.standard.guiframework.util.UIComponentUtil;
import ch.axonivy.fintech.standard.guiframework.workflow.BaseGuiWorkflow;
import ch.ivyteam.ivy.environment.Ivy;

public class WorkingDateValueChangeListener implements NotifyValueChangeEventListener {

    private static final String RULE_NAMESPACE = "RULE_NAMESPACE";

    @Override
    public void execute(ValueChangeEvent event, Map<String, Object> params)
            throws Exception {
        Dossier dossier =  (Dossier) params.get(ValueChangeEventListenerHelper.PARAM_DOSSIER_DATA);
        BaseGuiWorkflow baseGuiWorkflow = BaseGuiWorkflow.getInstanceToExecuteBusinessRuleWithDataModel();
        List<BusinessRuleDto> businessRuleDtos = BusinessRuleDtoConverter.getInstance().toDtosWithScanningListInside(dossier);
        String ruleNameSpace = (String) params.get(RULE_NAMESPACE);
        //businessRuleDtos.forEach(item -> Ivy.log().debug(item.getPropertyName() +"-" + item.getValue()));
        baseGuiWorkflow.proceedBusinessRuleWithDataModel(dossier, businessRuleDtos, ruleNameSpace, true);
        
        RequestContext.getCurrentInstance().update(UIComponentUtil.getFullId("personSalary", FacesContext.getCurrentInstance()));
    }
            
}
```

</div>

</div>

1.  - We can get our bussiness data model by params.get(ValueChangeEventListenerHelper.PARAM_DOSSIER_DATA)
    - Get rule name space by params.get(RULE_NAMESPACE)  (we put that value inside guiframework-config.xml)

Note :

1.  - We should update the element that we want rule effect on (Ex : In this case it would be "personSalary"). Because Business Rule doesn't effect on UI elements, so we should update it manually.  
        

#### Step 3: Create Rule drl :

1.  <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="c21b5190-13d0-4f8c-ad02-e0254f72b5a5" macro-name="code" style="border-width: 1px;">

    <div class="codeHeader panelHeader pdl" style="border-bottom-width: 1px;">

    **default.drl**

    </div>

    <div class="codeContent panelContent pdl">

    ``` syntaxhighlighter-pre
    package ch.axonivy.fintech.basic.component.showcase.address

    import ch.axonivy.fintech.standard.guiframework.bean.BusinessRuleDto;
    import ch.ivyteam.ivy.environment.Ivy;


    rule "workingDay > 10"
      when
      $workingDaty:BusinessRuleDto( propertyName == "BusinessRules.accountHolder.person.workingDay",value >10)
          $salary: BusinessRuleDto( propertyName == "BusinessRules.accountHolder.person.salary")
      then
      $salary.setValue(100D);
    end
    rule "workingDay > 30"
      when
      $workingDaty:BusinessRuleDto( propertyName == "BusinessRules.accountHolder.person.workingDay",value >30)
          $salary: BusinessRuleDto( propertyName == "BusinessRules.accountHolder.person.salary")
      then
      $salary.setValue(300D);
    end
    rule "workingDay > 40"
      when
      $workingDaty:BusinessRuleDto( propertyName == "BusinessRules.accountHolder.person.workingDay",value >40)
          $salary: BusinessRuleDto( propertyName == "BusinessRules.accountHolder.person.salary")
      then
      Ivy.log().debug("RUn > 40");
      $salary.setValue(400D);
    end

    ```

    </div>

    </div>

      
    In this rule, we have some differences from Gui rule:

- Using BusinessRuleDto instead of BaseMetaDto
- Using value instead of viewValue
- Structor for propertyName is BusinessRules.{propertyInDataModel}... We also can list some properties are supported in bussiness rule by "businessRuleDtos.forEach(item -\> Ivy.log().debug(item.getPropertyName() +"-" + item.getValue()));" in your listener

  
  
Thanks for any comment!
