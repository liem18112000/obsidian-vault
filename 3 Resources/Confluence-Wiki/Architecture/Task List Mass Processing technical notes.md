---
ai_hash: 9762983c448c89f1
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 2
depth: 2.51
entities: []
relevance: 0.701
source: https://axonivy.atlassian.net/wiki/spaces/X4/pages/47312601311/Task+List+Mass+Processing+technical+notes
space: X4
status: reference
tags:
- confluence
- architecture
- space/x4
title: Task List Mass Processing technical notes
topic: architecture
type: source
updated: 2023-03-21
---

# Task List Mass Processing technical notes

> [!info] Imported from Confluence
> Space **X4** · updated 2023-03-21 · [open original](https://axonivy.atlassian.net/wiki/spaces/X4/pages/47312601311/Task+List+Mass+Processing+technical+notes)
> Relevance 0.701 · topic `architecture`

#### 1. How to execute Ivy task and change status to DONE

Ivy task cannot make DONE by public API, so we have to destroy it

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="b89171c6-32df-4fd1-adb7-3536fd85af9b" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
 TaskDestroyUtil.processDoneTaskInDestroyedState(iTask);
```

</div>

</div>

#### 2. Proposal Architecture design

Overview:


![[47312601311-Overview class.PNG]]



On TaskListBean, build command executor:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="1b548b45-bff7-4c49-b9db-3c37a1e70df7" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
@ELCaller
public void onExecuteMassProcessing(String action) {
  List<CommandHolder> commandHolders = new ArrayList<>();
  commandHolders.add(new MassExecutionDialogHolderFactory(defaultTaskNavigationData, massExecutionAction).buildDialogHolder());
  commandHolders.add(MassExecutionProcessingHolderFactory(this.selectedTasks, massExecutionAction, workflow, currentStep, this.taskNavigationBean)
  .buildProcessingHolder());
}
```

</div>

</div>

Mass execution Process command:


![[47312601311-MassExecutionProcessorCommand.PNG]]



On **MassExecutionProcessCommand**, loop on each TaskItemModel, use `MassExecutionFinishTaskCommandExecutorFactory` to build and execute task processing.

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="9dfe4c42-ad68-45c2-954e-a728663df225" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
for (TaskItemModel taskItemModel : this.selectedTasks) {
      TaskUtil.getTask(taskItemModel.getTaskId()).ifPresent(taskItem -> {
          CommandExecutor commandExecutor = buildSingleTaskCommandExecutor(taskItemModel, taskItem);
          commandExecutor.execute();
          
          if (commandExecutor.isFailed()) {
              failureExecutors.add(commandExecutor);
              TaskAdditionalPropertiesUtil.setMassExecutionErrorState(taskItem.getUserTask(), Boolean.TRUE.toString());
          }
      });
}

private CommandExecutor buildSingleTaskCommandExecutor(TaskItemModel taskItemModel, Task taskItem) {
      TaskNavigationData taskNavigationData = MassExecutionTaskUtil.buildTaskNavigationData(this.workflow, this.currentStep, taskItemModel);
      switch (this.action) {
      case FINISH_TASK:
          commandExecutor = new MassExecutionFinishTaskCommandExecutorFactory(taskNavigationData, taskItem.getUserTask()).buildExecutor();
          break;
          ...
      }
}
```

</div>

</div>

After commandExecutor has been executed, if status of executor is FAILED then put mass execution error to ItemHead entity and sync task to Elasticsearch. This flag will be using for displayed failed tasks on Task List.

%% ai-graph-start %%

**Related notes:**
- [[Research on bulk removal of access class]]
- [[Task]]
- [[Run Re-Index ivy database]]
- [[Enhancements for API Delete and Restore]]
- [[Batching Design]]

%% ai-graph-end %%