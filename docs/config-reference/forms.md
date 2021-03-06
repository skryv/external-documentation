# Configuring Forms (and the corresponding information model)

With the form modeler, you can configure forms that, when linked to a user task in the workflow, can be filled in by a user.
Forms can be viewed in a webpage, but are also available to be downloaded with a default PDF visualisation.
Additionally, an API (REST) and export (CSV) can be made available as well. Finally, this configuration will automatically propagate to the search index for easy search & reporting in a later stage.

The form structure can be created by dragging & dropping the relevant fields into the structure.

![image](../_media/studio-form.png)

For each of the structural elements or fields, properties are available when you click on the individual field.

![image](../_media/studio-form-field-details.png)

Behind the scences, this is translated into a json file that describes the information model of the form, which can be accessed in the raw format.

!> This raw format is for power users only and no backward compatibility guarantees are given.

![image](../_media/studio-forms-raw.png)

## Field types

*field type not yet available via the Skryv Studio UI

| **Field type**   | **Explanation**                                                                                                                                                                                                        | **Available properties**                                                                                                              |
|------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------|
| text             | \-                                                                                                                                                                                                                     | `onlyWhen`, `conditions` , `read-only`, `required` ,`default`, `computedDefault`, `help`, `helpInLine` , `computedWith`, `maskConfig` |
| number           | \-                                                                                                                                                                                                                     | `onlyWhen`, `conditions` , `read-only`, `required` ,`default`, `computedDefault`, `help`, `helpInLine` , `computedWith`,              |
| boolean          | A yes/no field                                                                                                                                                                                                         | `onlyWhen`, `conditions` , `read-only`, `required` , `default`, `computedDefault`, `help`, `helpInLine`                               |
| multiline        | Similar to a text field but allows for several lines of text                                                                                                                                                           | `onlyWhen`, `conditions` , `read-only`, `required` , `help`, `helpInLine`                                                             |
| date             | \-                                                                                                                                                                                                                     | `onlyWhen`, `conditions` , `read-only`, `required` ,`default`, `computedDefault`, `help`, `helpInLine`                                |
| information      | When you want to give extra information to the user without expecting an answer in return                                                                                                                              | `onlyWhen`, `help`, `helpInLine`                                                                                                      |
| list             | A list to which users can dynamically add/remove a set of fields.                                                                                                                                                      | `onlyWhen`, `conditions` , `read-only`, `required` ,`visualisation`, `initialLength`, `labelForAdd`, `labelForDelete`                 |
| attachment2     | When you expect the user to upload an attachment.<br><br>The allowed formats are defined in the `application.properties` file of the applications. For example: `skryv.attachments.allowed-mime-types=application/pdf` | `onlyWhen` , `required`                                                                                                               |
| choice           | \-                                                                                                                                                                                                                     | `onlyWhen` , `read-only`, `required` ,`default`, `computedDefault`, `help`, `helpInLine` , `computedWith`, `displayOptions`           |
| multichoice      | \-                                                                                                                                                                                                                     | `onlyWhen` , `read-only`, `required` ,`default`, `computedDefault`, `help`, `helpInLine` , `computedWith`, `displayOptions`           |
| option           | An option for a `choice` or `multichoice` field                                                                                                                                                                        | `onlyWhen`                                                                                                                            |
| email            | A predefined text field that validates if the format of the email is correct.                                                                                                                                          | `onlyWhen` , `read-only`, `required` ,`default`, `computedDefault`, `help`, `helpInLine` , `computedWith`, `inputsize`                |
| section          | Used to structure your form<br><br>! important: a section is never part of a field’s path within the docdef                                                                                                            | `conditions`, `help`, `helpInLine`, `isOpenByDefault`                                                                                 |
| fieldset         | Used to group different fields                                                                                                                                                                                         | `onlyWhen`, `conditions` , `read-only` , `help`, `helpInLine`                                                                         |
| address fieldset * | A predefined fieldset we use that contains all address fields                                                                                                                                                          | `onlyWhen` , `conditions` , `read-only` , `required`,`customComponentName`,`autocomplete`, `visualisation`                            |
| object *           |                                                                                                                                                                                                                        |                                                                                                                                       |


## Properties

*property not yet available via the Skryv Studio UI

| **Property**          | **Explanation**                                                                                                                                                                                                                           | **Available values**                                                                                       | **Example value**                                                                                                                                                                     | 
|-----------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------                                                                                                                                                                                       |
| label                 | Display name of the field shown on the actual form<br><br>→ obligatory                                                                                                                                                                    |                                                                                                            |                                                                                                                                                                                       |
| name                  | Technical name (key) of the field to be referenced during configuration<br><br>→ obligatory                                                                                                                                               |                                                                                                            |                                                                                                                                                                                       |
| type                  | Type of field<br><br>→ obligatory                                                                                                                                                                                                         |                                                                                                            |                                                                                                                                                                                       |
| values                | Value of a field                                                                                                                                                                                                                          |                                                                                                            |                                                                                                                                                                                       |
| onlyWhen              | Define when this field is being shown<br><br>Note: choice or multichoice options that are “not shown” are actually visualised as disabled                                                                                                 | Refer to one or multiple computedExpressions                                                               | “onlyWhen”: “name\_computed\_expression“                                                                                                                                              |
| conditions             | By adding a condition you can give a warning or error notification to the user.                                                                                                                                                           | `error`: prevents you from submitting a task<br><br>`warning`: does not prevent you from submitting a task | "conditions": \[  <br>{  <br>"name": "dummy",  <br>"level": "error",  <br>"expression": "$$.computedExpressions.dummy\_expression",  <br>"errorMessage": "dummy\_text"  <br>}, <br>{  <br>"name": "nrn",  <br>"level": "error",  <br>"expression": "typeof $ === 'string' && $.length > 0 ? 97 - $.substring(0,9) % 97 === Number($.substring(9)):true",  <br>"errorMessage": "Please enter a valid national registry number."  <br>}  <br>\] |
| read-only             | The value of the field can not be modified                                                                                                                                                                                                | `true`, `false` or refer to one or multiple `computedExpressions`                                          | “read-only“: “name\_computedExpression“                                                                                                                                               |
| required              | The field needs to be filled in before you can submit the task                                                                                                                                                                            | `true`, `false` or refer to one or multiple `computedExpressions`                                          | “required”: true                                                                                                                                                                      |
| computedWith          | The value of this field is computed with a computedExpression                                                                                                                                                                             | Refer to a `computedExpression`                                                                            | “computedWith“ : “name\_computedExpression“                                                                                                                                           |
| computedDefault *       | The default value of the field is computed with a computedExpression                                                                                                                                                                      | Refer to a `computedExpression`                                                                            | “computedDefault“: “name\_computedExpression“                                                                                                                                         |
| default               | A default value of the field                                                                                                                                                                                                              | `string`                                                                                                   | “default”: “dummy text”                                                                                                                                                               |
| autocomplete *         | Used with an address fieldset in the front-office. Based on the search the address fields are autocompleted                                                                                                                               | `true`, `false`                                                                                            | “autocomplete“: true                                                                                                                                                                  |
| customComponentName *   |                                                                                                                                                                                                                                           | `skrAddressInput` , `skrHiddenComponent`, `skrAttachmentsList`                                             | "customComponentName": "skrHiddenComponent"                                                                                                                                           |
| maskConfig            | Show the value of the field in the following structure | `ssin`, `kbo`, `kboWithoutPrefix`,  `iban`, `currency`, `date`, `time`                                                                                             | "maskConfig": {  <br>"predefinedMask": "ssin"  <br>}                                                                                                                                  |
| visualisation         | For some field types we support different visualisations                                                                                                                                                                                  | Address fieldset, list: `compact`<br><br>Boolean field:`checkbox`<br><br>Information field: `callout-info` | "visualisation": "callout-info"                                                                                                                                                       |
| help                  | Help text for the user                                                                                                                                                                                                                    | `string`                                                                                                   | “help“: “dummy text“                                                                                                                                                                  |
| helpInLine            | Should the help text be hidden after a “?” or can it be shown by default                                                                                                                                                                  | `true`, `false`                                                                                            | "helpInLine": true                                                                                                                                                                    |
| isOpenByDefault       | Only used with sections. When you start the form, is the section collapsed or uncollapsed.                                                                                                                                                | `true`, `false`                                                                                            | “isOpenByDefault“: true                                                                                                                                                               |
| displayOptions        | How are the options of the (multi)choice field visualised                                                                                                                                                                                 | (multi)choice field: `list` ,`dropdown`                                                                    | "displayOptions": "dropdown"                                                                                                                                                          |
| computedWhen           |                                                                                                                                                                                                                                           |                                                                                                            |                                                                                                                                                                                       |
| referencelist *        |                                                                                                                                                                                                                                           |                                                                                                            |                                                                                                                                                                                       |
| referencelistKey *     |                                                                                                                                                                                                                                           |                                                                                                            |                                                                                                                                                                                       |
| binding *               |                                                                                                                                                                                                                                           |                                                                                                            |                                                                                                                                                                                       |
| resolvedReferencelist * |                                                                                                                                                                                                                                           |                                                                                                            |                                                                                                                                                                                       |
| initialLength         | How many items are there in a list field at the start                                                                                                                                                                                     | `number`                                                                                                   |                                                                                                                                                                                       |
| labelForDelete        | The label that should be used on the button to add a new item to the list                                                                                                                                                                 | `string`                                                                                                   |                                                                                                                                                                                       |
| labelForDelete        | The label that should be used on the button to remove an item from a list                                                                                                                                                                 | `string`                                                                                                   |                                                                                                                                                                                       |

## Computed expressions

Computed expressions allow to add logic to the information model, such as only showing certain fields depending on the value of another field.

![image](../_media/studio-form-only-when.png)

Computed expressions are small scripts (javascript), which can be added under the `Logic` tab of the form definition.

![image](../_media/studio-form-logic.png)

![image](../_media/studio-form-logic-detail.png)

Some of the computedExpressions we often use

| **What**                                              | **Computed expression**                                                                                                                                                                                                                                                                                                                                      |
|-------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| To check what the current task is                     | `"currentTask.taskDefinitionKey === 'id_of_the_task'"`                                                                                                                                                                                  |
| To check if the postcode of the address is in Flanders | `"typeof $ === 'string' ? (1500 <= Number($) && Number($) <= 3999) \| (8000 <= Number($) && Number($) <= 9999) : true"`                                                                                                                        |
| To check the IBAN number                              | `“typeof $ === 'string' && $.length > 0? ($.substring(0,2) === 'BE' ? ((($.substring(4,16) + '1114') % 97 + $.substring(2,4)) % 97 === 1) : ((($.substring(2,14) + '1114') % 97 + $.substring(0,2)) % 97 === 1)):true”` |
| To check the KBO                             | `"typeof $ === 'string' && $.length > 0 ? (97 - $.substring(0,8) % 97) === Number($.substring(8)) : true”` |
| To check the national registry number                              | `"typeof $ === 'string' && $.length > 0 ? 97 - $.substring(0,9) % 97 === Number($.substring(9)):true"` |
| Based on the option one selected in a choice field    | `"$.path_to_choice_field !== undefined? $.path_to_choice_field.selectedOption === 'selected_value': false"`                                                                                                                                                                                                                                                  |
| Calculate with dates                                  | Use `moment()`                                                                                                                                                                                                                                                                                                                                               |

Computed expressions on a list:

| **What**                                              | **Computed expression**                                                                                                                                                                                                                                                                                                                                      |
|-------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| listItemTitle | ```"element": {"computedExpressions": { "listItemTitle": "$.field1.selectedOption === undefined ? 'Field1' : $.field1.selectedOption + ' ' + ($.field2 \| '')"}}``` |
| onlyWhen | ```"element": {"computedExpressions": { "fieldSelectedOption": "$.field.selectedOption === 'option1'"}}``` |

## Address custom component

An address fieldset can use the custom behaviour of the custom component `skrAddressInput` if it contains the following fields:

*   Street
    
*   House number
    
*   Box number
    
*   Zip code
    
*   \[OPT\] NIS code
    
*   Municipality
    
*   \[OPT\] Country
    

![image](../_media/studio-form-skrAddresInput.png)

Example:

```
{
  "type": "fieldset",
  "label": "Address",
  "name": "address",
  "customComponentName": "skrAddressInput",
  "autocomplete": true,
  "fields": [
    {
      "type": "text",
      "label": "Street",
      "name": "street"
    },
    {
      "type": "text",
      "label": "Housenumber",
      "name": "housenumber"
    },
    {
      "type": "text",
      "label": "Boxnumber",
      "name": "boxnumber"
    },
    {
      "type": "text",
      "label": "Zipcode",
      "name": "zipcode"
    },
    {
      "type": "text",
      "label": "Municipality",
      "name": "municipality"
    }
  ]
}
```

For more information about these advanced configurations, please contact [one of our consultants](mailto:support@skryv.com) to give you an introduction.
