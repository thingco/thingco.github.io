---
layout: page
title: Trip Schema - Enriched
permalink: /trip-schema/enriched
---

Once a trip is normalised and mapped, the following schema is used. 

Mapping and road information is supplied by HERE. 

```graphql
type Trip {
  TripID: ID!
  TripDistance: Float
  TripDuration: Float
  TripEventCount: Int
  TripIsPerfect: Boolean
  TripStartTimestamp: String
  TripStartPlacename: String
  TripStartLocation: Location
  TripEndPlacename: String
  TripEndLocation: Location
  TripPolylineSegments: AWSJSON
}

type TripDetails {
  Events(id: ID! tripId: ID!): [Event]
}

type Event {
  EventID: ID!
  EventLocation: Location
  EventMessage: String
  EventName: String
}

```

<h2>Sample data</h2>

<h3>Enriched trip from raw data</h3>

<img src="/img/app1.png" class="app" />
<img src="/img/app2.png" class="app" />
<img src="/img/app3.png" class="app" />

```
{
  TripID: "a7dc4b3b-b4d4-3b88-badb-4017fedb1836"
  TripDistance: 28.5
  TripDuration: 46.44
  TripEventCount: 6
  TripIsPerfect: false
  TripStartTimestamp: "1589636527000"
  TripStartPlacename: "Warkworth"
  TripStartLocation: {
    Lng: -1.63081837,
    Lat: 55.02681351
  },
  TripEndPlacename: "Newcastle upon Tyne"
  TripEndLocation: {
    Lng: -1.6118183135986328,
    Lat: 55.34747314453125
  }
}
```


You can find the source code for the raw trip data supplied from the device at:
<a href="/trip-schema/raw.html">Trip data - raw</a>
