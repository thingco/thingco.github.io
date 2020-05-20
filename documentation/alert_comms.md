---
layout: page
title: Alert Communication
permalink: /alert/comms
---

In the event of an alert, the platform contacts the device using it's SMARTFNOL solution, to communicate with the customer to determine the circumstances and confirm or deny if an accident has occured. 

This information is then sent to the relevant third party via a webhook, referencing either the AccidentID or an IncidentRef supplied in the response to the initial webhook.

<iframe width="560" height="315" src="https://www.youtube.com/embed/C7Y55k8A4VY" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>


```graphql
type CustomerResponse {
  AccidentID string
  IncidentRef string
  CustomerContacted bool
  AccidentConfirmed bool
}
```

<h2>Sample data</h2>


```graphql
{
  AccidentID: 03ab6839-d165-4c19-8164-5a3ae4b7d5c2,
  CustomerContacted: true,
  AccidentConfirmed: false
}
```