---
ai_hash: e7a9503c988cc15a
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 3
depth: 2.73
entities: []
relevance: 0.731
source: https://axonivy.atlassian.net/wiki/spaces/GFT/pages/14280394549/2+-+Notes+for+merging+code+from+master+to+branch+render-list
space: GFT
status: reference
tags:
- confluence
- programming
- space/gft
title: 2 - Notes for merging code from master to branch render-list
topic: programming
type: source
updated: 2018-08-31
---

# 2 - Notes for merging code from master to branch render-list

> [!info] Imported from Confluence
> Space **GFT** · updated 2018-08-31 · [open original](https://axonivy.atlassian.net/wiki/spaces/GFT/pages/14280394549/2+-+Notes+for+merging+code+from+master+to+branch+render-list)
> Relevance 0.731 · topic `programming`

- **UIOutputPanelHandler**  
  - Remove function updateElement(). In branch render-list, it is not necessary to wrap component.
- **DefaultTooltipBuilder**  
  - function build tooltip **build(OutputLabel outputLabel)** & **build(CommandLink commandLink)**
    - Remove the line **replaceComponentWithPanel()**. In branch master, it is necessary to wrap the component with an outputpanel and replace the outputpanel to the position of component. However, in branch render-list, the component have the wrapper by default.
    - Remove the line **wrapOutputLabelPanel()**.

    

![[14280394549-tooltip-builder.png]]

  
      
  - function **buildPanel()**
    - In branch render-list, it is not necessary to add the component to panel, cause panel contained the component. In addition, add the tooltip and tooltip icon below component and above ui-message.  
      

![[14280394549-build-panel.png]]

%% ai-graph-start %%

**Related notes:**
- _(none above threshold)_

%% ai-graph-end %%