---
layout: page
title: Person
permalink: /person
---

All driving and accident data is related to a person.

```graphql
type Person {
  # Details
  Email: String
  AddressLine1: String
  AddressLine2: String
  City: String
  Country: String
  FirstName: String
  LastName: String
  PhoneNumber: String
  Postcode: String
  # Box
  BoxHasCamera: Boolean,
  BoxSerial: String
  BoxSignupCode: String
  BoxSimMSISDN: String
  BoxType: String
  # Downloads
  DownloadedCount: Int
  DownloadsAvailable: Int
  DownloadsLimit: Int
  ExpireTimestamp: Int
  # Stats
  StatCompletedTripCount: Int
	StatPerfectTripCount: Int
	StatPerfectTripStreak: Int
	StatRewardsClaimedCount: Int
	StatRewardsEarnedCount: Int
	StatTotalDistance: Float
	StatTotalDuration: Float
	StatTotalEventCount: Int
  # Connections
  Accidents(id: ID!, limit: Int, nextToken: String): AccidentConnection
  Trips(id: ID!, limit: Int, nextToken: String): TripConnection
}

```

<h2>Sample data</h2>

```
{
  PersonID: "67e2f9dc-3e75-4478-974b-79a919b308d1",
  FirstName: "Jonathon",
  LastName: "Valentine",
  BoxSerial: "89430301721911987477",
  BoxType: "LittleTheo",
}
```