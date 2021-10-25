# Configuration Tutorials

These video tutorials give step-by-step instructions on how to do some common basic configurations to build your process.


## I want to create a request form for the citizen

In this tutorial, we will build a basic process so a citizen can submit a request form for a subsidy via the front-office.

1. Start by creating a dossier type for the subsidy request process (not shown on video)
2. Create a subsidy workflow with the start message event set with the same message name as specified in the dossier type and a user task
3. Create a request form and add the fields
4. In the workflow, link the user task to the form via the Form template
5. Open the front-office preview and verify that everything is correct


<iframe width="784" height="490" src="https://www.youtube.com/embed/RskdZx1Mo10" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## I want to add automatic validations/rules to my form

The following tutorials show some ways to make digital forms smarter and more efficient.

#### I want to show a field only in some conditions

In this example, a citizen is only required to attach an invoice in case the amount they request is greater than 1000€.

1. Add the attachment field for the invoice
2. In the logic tab, create a new computed expression that checks whether the value of the Amount field is greater than 1000
3. Select the attachment field and select the expression from the dropdown
4. This field will now only be shown if an amount greater than 1000 is entered

<iframe width="784" height="490" src="https://www.youtube.com/embed/ji8oCzBPOYc" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>


#### I want to set a field based on other fields

In this example, a subsidy is paid in two instances and we want to automatically calculate how much a citizen will receive in the first and second payment.

1. Add the payment fields
2. In the logic tab, create a new computed expression that calculates 60% of the Amount field and another expression that calculates 40% of the Amount field
3. Select the First payment field, set it to read-only, and select the first payment calculation from the dropdown
4. Select the Second payment field, set it to read-only,  and select the second payment calculation from the dropdown
5. These two fields will now automatically show the 60% and 40% payments of the amount filled in by the citizen

<iframe width="784" height="490" src="https://www.youtube.com/embed/F02ad0Fzwnc" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## I want to use a value from a form in the BPMN process

#### I want to adjust workflow steps based on content from a form

This tutorial shows how to create different paths in your workflow using the content from a form.
In the example of a subsidy process, the back-office task depends on the type of subsidy requested by the citizen.

1. In the subsidy workflow, add a X-OR gateway and create two branches
2. Set the branch for Task A as the default branch
3. Set an expression on the branch for Task B so it is only taken when the selected subsidy type in the request form is B. This is a custom fluent expression that retrieves information from forms. 
4. The back-office casemanager sees Task B only when subsidy type B is chosen. In all other cases, they see Task A.

For more information on how fluent expressions can be used to manipulate data from forms, see the [workflow reference documentation](/config-reference/workflows.md##fluent-api).

<iframe width="784" height="490" src="https://www.youtube.com/embed/-Vumx4Eer2E" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## I want to follow my dossier status

#### I want to add a milestone

1. In your workflow, add a signal event
2. Name the event and fill in the signal name using the prefix `DS_`
3. The milestone is now automatically displayed in the history tab in the back-office

Milestones are also useful in other places:
- They can be used in the dossier type properties to define the progress bar in the front-office
- They can be used in templates

<iframe width="784" height="490" src="https://www.youtube.com/embed/mc_0aAWNaUs" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## I want to configure task execution

#### I want to assign a task to users with a specific role

!>Note: Configuration of users and user roles is not yet available via Skryv Studio. We currently provide a few default users (i.e. `anna`, `jimmy`) and user roles (i.e. `Requester`, `Casemanager`).

This tutorial shows how to assign a task to a back-office user with the role `Casemanager` to process a subsidy request. This means that this task will not be available to other users who do not have that role (e.g. citizen requesters).

1. First make sure that the user Jimmy is given the relevant role to execute the task via the admin page of the back-office.
2. In the workflow, set the candidate group of the back-office task to `casemanager`
3. This task will now be automatically assigned to Jimmy

<iframe width="784" height="490" src="https://www.youtube.com/embed/XSOB1YlvZGo" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>


#### I want to set a due date on a task

Due dates are useful to make sure that back-office tasks are executed in the correct timeframe. Tasks are automatically sorted according to due date in the Skryv Platform back-office.

1. In your workflow, add a due date to your back-office task. Here we are setting it to a period of 10 days after the task is active 
2. The final due date is now automatically shown on the back-office task list for the casemanager

<iframe width="784" height="490" src="https://www.youtube.com/embed/T_fg89ww9fw" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>


## I want use communication templates

The following tutorials explain how communication templates can be used to send various types of messages (letters or e-mails) to citizens or various stakeholders.

#### I want to generate a letter based on a template

In this example, we want to generate a letter for the citizen that we want to print manually and send via post.

1. Create a new communication template
2. Write the body of the letter and copy the placeholders for the relevant form fields you want to use
3. In your workflow, add a user task with the Communication template and link to the letter template you just made
4. This letter is now automatically filled with the contents of the request form

Tip: You can edit the template and refresh the communication task to preview the changes without having to start a new dossier.

<iframe width="784" height="490" src="https://www.youtube.com/embed/9hiPCpoQ5fc" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>


#### I want to send automatic e-mail notifications

This tutorial shows how to send an automatic e-mail notification to the citizen after they have submitted a request form.

1. In the request form, add a field for the citizen to fill in their e-mail
2. Create a new communication template for the e-mail
3. Write the body of the e-mail and copy placeholders for the relevant form fields you want to use
4. In your workflow, add a service task with the Mail Task template and fill the parameters so that the e-mail is sent to the e-mail address the citizen gave in the request form (see [I want to use a value from a form in the BPMN process](##i-want-to-use-a-value-from-a-form-in-the-BPMN-process) and [Accessing a document](/config-reference/workflows.md###accessing-a-document) )

<iframe width="784" height="490" src="https://www.youtube.com/embed/lrYgUrOGT2U" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## I want to add a payment step to my workflow

The following tutorial explains how to add a payment step to your workflow. This can be used to request a payment from the requestor in the Front Office.

!>Note: A short onboarding needs to happen before you can use this feature in your environment. Please contact support@skryv.com to start the onboarding.

1. At the desired place in your workflow add a call activity.
2. From the Element Templates select eGovFlowPayment
3. Enter the amount that needs to be paid. This can be a number `50`, `${12,3}` or a variable `${amountToBePaid}` or a calculation `${5*numberOfClubMembers}`
4. The preview environments will be connected to the test services of the payment provider so no actual payments will happen. To test use accountnumber `4100000000000000000`, security code `123` and an expiration date that is in the future.

<iframe width="784" height="490" src="https://www.youtube.com/embed/l-LZQ7RwZE4" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>