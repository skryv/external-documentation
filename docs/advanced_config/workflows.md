## BPMN


## Task Types

### A yes/no task

A Yes-No task is very simple type of task that allows the user to check off an action.
Simply Create a process or a case and add a user task to the latter. Use Workflow modeler to edit the workflow.

### A yes/no task


## Fluent API

The Fluent API allows to interact with other configuration artefacts from a Camunda process. Examples when you want to use the fluent API is for example when you want to fetch the information a user filled in in the request to decide which path in the process you will follow.

The Fluent API is typically used in Camunda listeners or service tasks, to retrieve specific data from dossiers or to trigger specific operations on specific arterfacts from the Camunda process.

The base principle of the Fluent API is that you can chain one operation after the other, passing the output of the operation on the left to the next one. 

A simple example to explain this principle is `${skryv.dossierFromScope(execution).getLabel()}`. This executes the following logic:
* creates the context of Skryv
* fetches the dossier
* gets the label

The following sections contain the reference documentation for the Fluent API.


### Accessing a dossier

The most common need is to get full access to the current dossier:

`${skryv.dossierFromScope(execution)}`

Alternatively, a specific dossier can be retrieved based on its id, i.e. the business key:

`${skryv.dossier(dossierId)}`

### Operations on a dossier

#### Dossier label

To get the human readable dossier label:

`${skryv.dossierFromScope(execution).getLabel()}`

?> It is a best practice to store the dossier label in a Camunda variable `dossierLabel`. This makes it possible to retrieve dossiers in the Camunda cockpit based on the dossier id.

`${execution.setVariable('dossierLabel', skryv.dossierFromScope(execution).getLabel())}`

The dossier label can also be recalculated from the Camunda process, based on a specific label provider or not:

`${skryv.dossierFromScope(execution).recalculateLabel(labelProviderName)}`

`${skryv.dossierFromScope(execution).recalculateLabel()}`

?> It is a best practice to set the Camunda variable `dossierLabel` again whenever the dossier label is recalculated.

For more information on label providers, please refer to the article “[How to set up a label provider?](https://skryvdev.atlassian.net/wiki/spaces/SP/pages/1560543242)”.

#### Dossier export

`${skryv.dossierFromScope(execution).exportDossierWithAttachments(folderName)}`

`${skryv.dossierFromScope(execution).exportSkryvDossier(folderName)}`

#### Dossier access

`${skryv.dossierFromScope(execution).grantAccess().forTeamOfUser(userSub)}`

`${skryv.dossierFromScope(execution).revokeAccess().forTeamOfUser(userSub)}`

#### Other dossier operations

`${skryv.dossierFromScope(execution).takeSnapshot(snapshotLabel)}`

`${skryv.dossierFromScope(execution).deactivate()}`

### Accessing a document

The most commonly used way to get access to a document on the current dossier is by using the getOrCreate method, which will create or - if it exists already - return the dossier scoped document for a specific definitionKey, i.e. docdef:

`${skryv.dossierFromScope(execution).getOrCreateDocumentByDefinitionKey(definitionKey)}`

Self-explanatory alternatives to retrieve a document are:

`${skryv.dossierFromScope(execution).getDocumentByDefinitionKey(definitionKey)}`

`${skryv.dossierFromScope(execution).findDocumentByDefinitionKey(definitionKey)}`

Accessing process scoped documents can be done as follows:

`${skryv.execution(execution)..getOrCreateDocumentByDefinitionKey(definitionKey)}`

### Operations on a document

`getField(String fieldPath)`

`setField(String fieldPath, Object value)`

`markReadOnly()`

`markEditable()`

`reset()`

`isValid()`

`getDocument()`

`getRawDocumentValue()`

`getRawDocumentValueAs(Class<T> classOfT)`

`updateFromWorksheet(Map<String, Object> worksheetData)`

`updateFromVariableScope(VariableScope scope)`

`updateWithMapping(T input, MutableSkryvDocMapper<T> mapper)`

`clearAndUpdateWithMapping(T input, MutableSkryvDocMapper<T> mapper)`


### Accessing a process - delegateExecution

`skryv.execution(execution)`

### Accessing a communication

`skryv.communication(communicationId)`