---
layout: page
title: Alert Summary
permalink: /alert/summary
---

Once an alert is recieved, and analysed, the summary is stored in the format below.

```graphql
type Accident {
  AccidentID: ID!
  AccidentTimestamp: String
  AccidentImpactType: String
  AccidentIsAcknowledged: Boolean
  AccidentIsConfirmed: Boolean
  AccidentLocation: Location
  AccidentPlacename: String
  AccidentSpeed: Int
  AccidentSeverity: Int
  TripID: ID
}

```

<h2>Sample data</h2>