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
4. Select the Second payment field, set it to read-only, and select the second payment calculation from the dropdown
5. These two fields will now automatically show the 60% and 40% payments of the amount filled in by the citizen

<iframe width="784" height="490" src="https://www.youtube.com/embed/F02ad0Fzwnc" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## I want to use a value from a form in the BPMN process

#### I want to adjust workflow steps based on content from a form

This tutorial shows how to create different paths in your workflow using the content from a form.
In the example of a subsidy process, the back-office task depends on the type of subsidy requested by the citizen.

1. In the subsidy workflow, add a XOR gateway and create two branches
2. Set the branch for Task A as the default branch
3. Set an expression on the branch for Task B so it is only taken when the selected subsidy type in the request form is B. This is a custom fluent expression that retrieves information from forms.
4. The back-office casemanager sees Task B only when subsidy type B is chosen. In all other cases, they see Task A.

For more information on how fluent expressions can be used to manipulate data from forms, see the [workflow reference documentation](/config-reference/workflows.md##fluent-api).

<iframe width="784" height="490" src="https://www.youtube.com/embed/-Vumx4Eer2E" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## I want to show or hide list elements using a computed expression

Using the a computed expression to show or hide elements in a form is fairly straigforward using the `Studio`. Applying this filters on the elements of a list is not supported by`Studio`, yet this can be achieved by editing the raw `json` of the form.

##### Example

Lets say we have a list of vehicles. Every list element has the type of the vehicle and a checkbox wether a drivers license is required or not.

<table>
<tr>
<td>

![image](../_media/Field_List_computed_show.png)

</td>
<td>

```json                                       
{                                             
  "label": "Vehicle request",                 
  "name": "vehicle-request",                  
  "fields": [                                 
    {                                         
      "name": "vehicleList",                  
      "label": "vehicleList",                 
      "type": "list",                         
      "fields": [                             
        {                                     
          "name": "verhicleType",             
          "label": "verhicleType",            
          "type": "text"                      
        },                                    
        {                                     
          "name": "driversLicenseRequired",   
          "label": "driversLicenseRequired",  
          "type": "boolean"                   
        }                                     
      ]                                       
    }                                         
  ]                                           
}                                             
```                                           

</td>
</tr>
</table>

Now we want to add a checkbox to the form to hide or show the vehicles with a drivers license requirement. There are 2 things we need to do:

1. Add a checkbox

```json
}
  "fields": [
      {
        "name": "driversLicenseOnly",
        "label": "driversLicenseOnly",
        "type": "boolean"
      }
}
```

2. Add a computed expression and apply it to the `computedShow` of the list.

The  following expression will check if the checkbox in the list item, has the same value as the checkbox added in step 1.

```json
{
  "element": {
    "computedShow": "checkDriversLicenseRequired",
    "computedExpressions": {
      "checkDriversLicenseRequired": "$.driversLicenseRequired === undefined || $$.parent.parent.propertyManipulators.driversLicenseOnly.value === $.driversLicenseRequired"
    }
  }
}
```

Putting it al toghether gives the following result.

<table>
<tr>
<td>

![image](../_media/Field_List_computed_show.gif)

</td>
<td>

```json                                       
{                                             
  "label": "Vehicle request",                 
  "name": "vehicle-request",                  
  "fields": [  
    {
        "name": "driversLicenseOnly",
        "label": "driversLicenseOnly",
        "type": "boolean"
    },                               
    {                                         
      "name": "vehicleList",                  
      "label": "vehicleList",                 
      "type": "list", 
      "element": {
        "computedShow": "checkDriversLicenseRequired",
        "computedExpressions": {
          "checkDriversLicenseRequired": "see above"
        }
      },                        
      "fields": [                             
        {                                     
          "name": "verhicleType",             
          "label": "verhicleType",            
          "type": "text"                      
        },                                    
        {                                     
          "name": "driversLicenseRequired",   
          "label": "driversLicenseRequired",  
          "type": "boolean"                   
        }                                     
      ]                                       
    }                                         
  ]                                           
}                                             
```                                           

</td>
</tr>
</table>



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

!>Configuration of users and user roles is not yet available via Skryv Studio. We currently provide a few default users (i.e. `anna`, `jimmy`) and user roles (i.e. `Requester`, `Casemanager`).

This tutorial shows how to assign a task to a back-office user with the role `Casemanager` to process a subsidy request. This means that this task will not be available to other users who do not have that role (e.g. citizen requesters).

1. First make sure that the user Jimmy is given the relevant role to execute the task via the admin page of the back-office.
2. In the workflow, set the candidate group of the back-office task to `Casemanager`
3. This task will now be automatically assigned to Jimmy

<iframe width="784" height="490" src="https://www.youtube.com/embed/XSOB1YlvZGo" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

#### I want to set a due date on a task

Due dates are useful to make sure that back-office tasks are executed in the correct timeframe. Tasks are automatically sorted according to due date in the Skryv Platform back-office.

1. In your workflow, add a due date to your back-office task. Here we are setting it to a period of 10 days after the task is active
2. The final due date is now automatically shown on the back-office task list for the casemanager

<iframe width="784" height="490" src="https://www.youtube.com/embed/T_fg89ww9fw" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## I want to use communication templates

The following tutorials explain how communication templates can be used to send various types of messages (letters or e-mails) to citizens or various stakeholders.

#### I want to generate a letter based on a template

In this example, we want to generate a letter for the citizen that we want to print manually and send by post.

1. Create a new communication template
2. Write the body of the letter and copy the placeholders for the relevant form fields you want to use
3. In your workflow, add a user task with the Communication template and link to the letter template you just made
4. This letter is now automatically filled with the contents of the request form

?> You can edit the template and refresh the communication task to preview the changes without having to start a new dossier.

<iframe width="784" height="490" src="https://www.youtube.com/embed/9hiPCpoQ5fc" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

#### I want to send automatic e-mail notifications

This tutorial shows how to send an automatic e-mail notification to the citizen after they have submitted a request form.

1. In the request form, add a field for the citizen to fill in their e-mail address
2. Create a new communication template for the e-mail
3. Write the body of the e-mail and copy placeholders for the relevant form fields you want to use
4. In your workflow, add a service task with the Mail Task template and fill the parameters so that the e-mail is sent to the e-mail address the citizen gave in the request form (see [I want to use a value from a form in the BPMN process](##i-want-to-use-a-value-from-a-form-in-the-BPMN-process) and [Accessing a document](/config-reference/workflows.md###accessing-a-document) )

<iframe width="784" height="490" src="https://www.youtube.com/embed/lrYgUrOGT2U" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## I want to add a payment step to my workflow

The following tutorial explains how to add a payment step to your workflow. This can be used to request a payment from the requestor in the Front Office.

!>A short onboarding needs to happen before you can use this feature in your environment. Please contact support@skryv.com to start the onboarding.

1. At the desired place in your workflow add a call activity.
2. From the Element Templates select eGovFlowPayment
3. Enter the amount that needs to be paid. This can be a number `50`, `${12,3}` or a variable `${amountToBePaid}` or a calculation `${5*numberOfClubMembers}`
4. The preview environments will be connected to the test services of the payment provider so no actual payments will be made. To test use accountnumber `4100000000000000000`, security code `123` and an expiration date that is in the future.

<iframe width="784" height="490" src="https://www.youtube.com/embed/l-LZQ7RwZE4" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## I want to send status notifications via DOSIS

The following tutorial explains how to add a DOSIS notification to your workflow.
Briefly, a DOSIS connection allows you to send standard status updates of a dossier to a citizen's _Mijn Burgerprofiel_ or a company's inbox in eLoket Ondernemers. Please see the [official website](https://overheid.vlaanderen.be/informatie-vlaanderen/producten-diensten/dossierstatusinformatiesysteem-dosis) for more detailed information.

!>A short onboarding needs to happen before you can use this feature in your environment. Please contact support@skryv.com to start the onboarding.

1. Add a Service Task in your workflow at the place where a dossier changes status (e.g. after submission)
2. Select "Connector - Dosis - Dossierstatusupdate" from the Element Template menu
3. Enter the minimally required information to send via DOSIS
   1. For the agent identification, we recommend you use a fictional National Registry Number or KBO-number to test the configuration (this can be filled in directly in the element template, or retrieved from a form using a fluent expression)
   2. Important! Make sure you enter a valid product id, you will receive this information during your onboarding
4. Test your workflow in the preview-environment and check the successful dispatch of the DOSIS notification on the relevant website
   - [Mijn Burgerprofiel](https://beta.frontend.burgerprofiel.dev-vlaanderen.be/) if a notification was sent to a citizen
     - Click on "Meld u aan"
     - Go to the Tab "INSZ invullen" and fill in the INSZ with the National Registry Number you used for your test
     - After you log in with this test user, you should see the status update that was just sent
   - [eLoket Ondernemers](https://beta.dosis.dev-vlaanderen.be/dossier/controlerapport) if a notification was sent to a company
     - You should see a successful execution report if the status update happened correctly

<iframe width="784" height="490" src="https://www.youtube.com/embed/L-2dwow48oA" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## I want to register events in GIPOD

Onderstaande tutorial legt uit hoe er geïntegreerd kan worden met GIPOD om evenementen te registeren.

!>Er moet een onboarding doorlopen worden vooralleer deze feature bruikbaar is op uw applicatie. Contacteer support@skryv.com om de onboarding op te starten.

### Gegevens

Volgende informatie is nodig om de GIPOD connector te kunnen aanroepen:

- Ingetekend evenment gebied: voeg hiervoor de kaart component toe aan uw formulier (verderop meer over de configuratie van de kaart component)
- Startdatum evenement (Moet in de toekomst liggen minstens één dag later)
- Einddatum evenement (Mag niet voor de startdatum liggen)
- E-mail organisator evenement
- Naam evenement

### GIPOD connecties

#### Controleer conflicten (Gipod – public occupancies):

Deze connectie wordt gebruikt om te controleren of er voor de aangevraagd periode op de aangevraagde plaats geen conflicten zijn. Indien er conflicten zijn worden deze automatisch bij op de kaart ingetekend na het oproepen van deze connectie.

##### Configuratie

- Form key: Hier plaatst u de key van het formulier waar de gegevens van het evenement uit opgehaald kunnen worden (typisch is dit het aanvraag formulier)
- Map field path: het pad naar het kaart componentje in het formulier
- Start date path: het pad naar de startdatum van het evenement in het formulier
- End date path: het pad naar de einddatum van het evenment in het formulier.
- Result variable: De connectie geeft ook een boolean waarde (true/false) terug na het oproepen, deze geeft aan of er conflicten zijn (true) of niet (false). Deze variabele kan door de configurator gebruikt worden om een speciaal pad in de workflow te starten. Bv. De aanvraag wordt automatisch afgewezen of moet terug naar de aanvrager om aanpassingen te doen.

![Image](../_media/gipod-check-for-conflicts.png)

#### Registreer evenement (Gipod – register event):

Dit is de eerste connectie gebruikt moet worden om een nieuw evenement in GIPOD te registreren. Deze connectie maakt een nieuw evenement aan in GIPOD met de voorlopige status ” Niet-concreet gepland”. Dit zorgt er voor dat een evenment alvast geregistreerd is zodat er tijdens het behandelen van de aanvraag geen nieuwe conflicten ontstaan.

##### Configuratie

- Form key: Hier plaatst u de key van het formulier waar de gegevens van het evenement uit opgehaald kunnen worden (typisch is dit het aanvraag formulier)
- Map field path: het pad naar het kaart componentje in het formulier
- Email path: het pad naar het email adres van de organisator in het formulier
- Description path: het pad naar de naam/beschrijving van het evenement in het formulier
- Start date path: het pad naar de startdatum van het evenement in het formulier
- End date path: het pad naar de einddatum van het evenment in het formulier.
- Result variable: Deze connectie geeft als resultaat de id van het evenement in GIPOD. Het is belangrijk deze bij te houden in een variabele aangezien deze voor de volgende connecties nodig is.

![Image](../_media/gipod-register-event.png)

#### Bevestig evenement (Gipod – Confirm event):

Deze connectie bevestigd het evenement in GIPOD, de status wijzigt dan van “Niet-concreet gepland” naar “Concreet gepland”. Deze connectie gebeurd typisch eens de aanvraag volledig is goedgekeurd. Opgelet: Voordat deze connectie opgeroepen kan worden moet Registreer evenement reeds gebeurd zijn.

##### Configuratie

- Event id: geef hier de naam van de variabele met de event id in. Deze komt als resultaat uit de “Registreer evenement” connectie.

![Image](../_media/gipod-confirm-event.png)

#### Verwijder evenement (Gipod – Delete event):

Deze connectie verwijderd het evenement uit GIPOD. Deze connectie gebeurd typisch indien de aanvraag werd afgekeurd of als er wijzigingen aan het evenement nodig zijn, in dat geval wordt het evenement eerst verwijderd om dan een nieuw evenement te registreren. Opgelet: Voordat deze connectie opgeroepen kan worden moet Registreer evenement reeds gebeurd zijn.

##### Configuratie

- Event id: geef hier de naam van de variabele met de event id in. Deze komt als resultaat uit de “Registreer evenement” connectie.

![Image](../_media/gipod-delete-event.png)

### Voorbeeld workflow

![Image](../_media/gipod-example-flow.png)
