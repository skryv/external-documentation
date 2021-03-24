# Configuration Tutorials

These video tutorials give step-by-step instructions on how to do some common basic configurations to build your process.


## I want to create a subsidy request form for the citizen

In this tutorial, we will build a basic process so a citizen can submit a request form for a subsidy via the front-office.

1. Start by creating a dossier type for the subsidy request process (not shown on video)
2. Create a subsidy workflow with the start message event set with the same message name as specified in the dossier type and a user task
3. Create a request form and add the fields
4. In the workflow, link the user task to the form via the Form template
5. Open the front-office preview and verify that everything is correct


<iframe width="560" height="490" src="https://www.youtube.com/embed/RskdZx1Mo10" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## I want to add automatic validations/rules to my form

The following tutorials show some ways to make digital forms smarter and more efficient.

#### I want to show a field only in some conditions

In this example, a citizen is only required to attach an invoice in case the amount they request is greater than 1000€.

1. Add the attachment field for the invoice
2. In the logic tab, create a new computed expression that checks whether the value of the Amount field is greater than 1000
3. Select the attachment field and select the expression from the dropdown
4. This field will now only be shown if an amount greater than 1000 is entered

<iframe width="560" height="490" src="https://www.youtube.com/embed/ji8oCzBPOYc" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>


#### I want to set a field based on other fields

In this example, a subsidy is paid in two instances and we want to automatically calculate how much a citizen will receive in the first and second payment.

1. Add the payment fields
2. In the logic tab, create a new computed expression that calculates 60% of the Amount field and another expression that calculates 40% of the Amount field
3. Select the First payment field, set it to read-only, and select the first payment calculation from the dropdown
4. Select the Second payment field, set it to read-only,  and select the second payment calculation from the dropdown
5. These two fields will now automatically show the 60% and 40% payments of the amount filled in by the citizen

<iframe width="560" height="490" src="https://www.youtube.com/embed/F02ad0Fzwnc" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## I want to use a value from a form in the BPMN process

#### I want to adjust workflow steps based on content from a form

This tutorial shows how to create different paths in your workflow using the content from a form.
In the example of a subsidy process, the back-office task depends on the type of subsidy requested by the citizen.

1. In the subsidy workflow, add a X-OR gateway and create two branches
2. Set the branch for Task A as the default branch
3. Set an expression on the branch for Task B so it is only taken when the selected subsidy type in the request form is B. This is a custom fluent expression that retrieves information from forms. 
4. The back-office casemanager sees Task B only when subsidy type B is chosen. In all other cases, they see Task A.

For more information on how fluent expressions can be used to manipulate data from forms, see the workflow reference documentation.

<iframe width="560" height="490" src="https://www.youtube.com/embed/-Vumx4Eer2E" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## I want to follow my dossier status

#### I want to add a milestone

1. Add a signal event
2. Name the event and fill in the signal name using the prefix `DS_`
3. The milestone is now automatically displayed in the history tab in the back-office

Milestones are also useful in other places:
- They can be used in the dossier type properties to define the progress bar in the front-office
- They can be used in templates

<iframe width="560" height="490" src="https://www.youtube.com/embed/mc_0aAWNaUs" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
