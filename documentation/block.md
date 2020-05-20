---
layout: page
title: Driving Block
permalink: /trip-schema/block
---

All trips are added to a block. On completion of the trip that sends the block over 100.00 miles, the block is scored. 

```graphql
type Block {
	PersonID      string
	Metadata      string 
	BlockID       string 
	CreatedAt     string 
	Distance      float64
	Duration      int
	Scored        bool  
	Scores        Scores  
	EventCounts   Scores
	Trips         []string 
}

type Scores {
	DrivingDuration int
	ErraticDriving  int
	Speeding        int
	TimeOfDay       int
	OverallScore    int
}
```

<h2>Sample data</h2>

<img src="/img/app4.png" class="app" />
<img src="/img/app5.png" class="app" />

```
{
	PersonID:      "67e2f9dc-3e75-4478-974b-79a919b308d1"
	BlockID:       "bb593335-89dd-40ec-9e29-03188e63ba7a" 
	CreatedAt:     "1589875511126" 
	Distance:      446975.38219977764
	Scored:        true  
	Scores:        {
		DrivingDuration: 80
		ErraticDriving:  100
		Speeding:        55
		TimeOfDay:       97
		OverallScore:    74
	},  
	EventCounts:   {
		DrivingDuration: 1
		ErraticDriving:  18
		Speeding:        35
		TimeOfDay:       1
	}, 
	Trips: ["1a21aa3b-b553-3c37-be7c-61a6532b4918","2d649574-6602-38fb-9b2f-cf6dc58ef1ec","4a0c84e4-7a0b-3f66-861d-266acf7f3a12","7a507fff-0f0b-306d-b587-79e967f6c3e1","82af1b70-291a-3465-9190-90c8f4835354","a7dc4b3b-b4d4-3b88-badb-4017fedb1836","b8c0b7f0-0c09-3dee-9ccd-b60e63ac08b5","be7b1a8e-8f54-3a9c-95c1-a3a92a0fc8b2","bf3deeb4-4d01-3f88-a5ee-5c858f264636","c4764531-c674-33c8-b079-4c822caeb468","ec039857-6864-32b5-b825-f7a10196fdbf"]
}
```