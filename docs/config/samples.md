# Sample Configuration Artefacts

While we are working to add more graphical editors to Skryv Studio, you can use the snippets below to get started more quickly.

## Dossier types (.dosdef)

Copy the snippet below in the dossier type editor and change it to your usecase (e.g. names, labels, reference to the process messages and milestones, ...).

### Simple Dossier Type with a front-office

This is an example of a dossier type for a process that starts with a request button in the front office (e.g. "Start subsidy request").

```json
{
  "name": "simple_dossier_type_front_office",
  "label": "Simple Dossier Type with Front-Office",
  "description": "Simple Dossier Type with Front-Office",
  "creationModes": [
    {
      "key": "eLoket",
      "label": "Request via Front-office",
      "labelProviderConfiguration": {
        "type": "counter",
        "counterLabel": "DEMO",
        "includeYearPrefix": true,
        "counterDigits": 5
      },
      "grantRole": "requester",
      "processMessagesOnStartup": [
        "demo-process"
      ]
    }
  ]
}
```

### Simple Dossier Type with a front-office and a progress bar

This is an example of a dossier type for a process that starts with a request button in the front office (e.g. "Start subsidy request"). The progress bar will be displayed on the dossierpage and depends on the milestones (signal event id) in the workflow.

```json
{
  "name": "simple_dossier_type_front_office_steps",
  "label": "Simple Dossier Type with Front-Office and Process Steps",
  "description": "Simple Dossier Type with Front-Office and Process Steps",
  "creationModes": [
    {
      "key": "eLoket",
      "label": "Request via Front-office",
      "labelProviderConfiguration": {
        "type": "counter",
        "counterLabel": "DEMO",
        "includeYearPrefix": true,
        "counterDigits": 5
      },
      "grantRole": "requester",
      "processMessagesOnStartup": [
        "demo-process"
      ]
    }
  ],
  "dossierProgressSteps": [
    {
      "name": "Start",
      "description": "Go ahead, it is a simple form :)",
      "relatedMilestoneKey": "start"
    },
    {
      "name": "In progress",
      "description": "We are working on it, you'll hear from us soon!",
      "relatedMilestoneKey": "in_progress"
    },
    {
      "name": "Done",
      "description": "You have been served. It was nice interacting with you",
      "relatedMilestoneKey": "done"
    }
  ]
}
```