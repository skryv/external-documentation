# Configuring Templates

!> This documentation will be updated once the wysiwig editor is integrated in Skryv Studio

## Handlebars templates

For template handling, we use the Handlebars templating engine. 
For more info: please consult the [hbs website.](https://handlebarsjs.com/guide/#what-is-handlebars)

## Using information from the dossier in the template

The base idea is that all information from the dossier can be used in template.
This works through expression like `{{docs.name_of_docdef.path_of_field}}`: 
* `docs` indicates this is information from an information model (~ form)
* `name_of_docdef` is the technical name of the form
* `path_of_field` is the reference to this field.

## Common Functions
Below you’ll find examples of functions we often use in a handlebars template. 


| **Function**              | **Explanation**                                                                                  | **Example**                                                                                                                             |
|---------------------------|--------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------|
| Dossier label             | Show the assigned dossier label                                                                  | `{{dossier._source.dossier.label}}`                                                                                                     |
| Show the value of a field | Show .hbs the way to the docdef and the field you want to show                                   | `{{docs.name_of_docdef.path_of_field}}`                                                                                                 |
| If .. else                | Check if a variable is defined and fill content depending on whether the result is true or false | `{{#if docs.name_of_docdef.path_of_field}} {{docs.name_of_docdef.path_of_field}}{{else}} {{docs.name_of_docdef.path_of_field2}}{{/if}}` |
| ifCond                    | Compare the value of a variable retrieved via hbs and fill content if comparison returns true    | `{{#ifCond VAR operator=”=” expected=“VALUE“}} {{/ifCond}}`                                                                             |
| each                      | Repeat the following for each element of the list                                                | `{{#each docs.path_to_list.elements}} {{/each}}`                                                                                        |
| .value                    | You need to add value if you want to show the value of a multiline field                         | `{{docs.name_of_docdef.path_of_field.value}}`                                                                                           |
| userDetails               | Get details on the user who initiated a task (ex: name & e-mail)                                 | `{{#userDetails assignee}}{{username}} / {{email}} {{/userDetails}}`                                                                    |
| date                      | Fill in & format date information                                                                | `{{date now format="dd-MM-yyyy" add="P1y"}}`                                                                                            |
| number                    | Fill & format number information                                                                 | `{{number VAR decimalLength='4' thousandsSep=' ' decimalSep=','}}`<br><br>Default `decimalLength` = 2                                   |
| index                    | Show the index of a list item                                                                 | `{{index @index}}`                                                 |
| latestTask                | Compare the name of the latest task(s) in the process with target task(s) name                   | `{{#latestTask dossier.task names="Some Task, Another Task"}} {{else}} {{/latestTask}}`                                                 |
| latestMilestone           | Compare the name of the latest milestone in the process with target milestone name               | `{{#latestMilestone dossier.milestones keys="milestone_1, milestone_2"}} {{/latestMilestone}}`                                          |
| dossierUrl                | Fill in hyperlink URL for FO or BO dossierpages                                                  | `{{dossierUrl dossier._source.dossier.id type=""}}`<br><br>`{{dossierUrl dossier._source.dossier.id type="front"}}`                     |

Other tips concerning Skryv PDFS/communications:

| **Topic**       | **Description**                                                                                                                                                                                                                                                                                 |
|-----------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| To add an image | Add an image by converting the image to a base64 url (e.g via [https://www.base64-image.de/](https://www.base64-image.de/))                                                                                                                                                                     |
| Font            | *   The font size can be different for certain environments. Often a backend change is necessary to tweak this<br>    <br>*   To use for example Calibri as a font, be aware that not all systems support that font. Add a “sans-serif” to cover all cases. `font-family: calibri, sans-serif;` |
