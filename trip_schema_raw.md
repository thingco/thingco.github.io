---
layout: page
title: Trip Schema - Raw
permalink: /trip-schema/raw
---

This schema matches exactly what comes from the device post de-compression. 

```
type Data struct {
    DeviceID  string
    TripStart int
    Points    []Point
 }
  
 //Point is raw data collected by a little theo device
 type Point struct {
    Timestamp         int
    BatteryVoltage    int
    BatteryPercentage int
    Ax                float64
    Ay                float64
    Az                float64
    MinAx             float64
    MinAy             float64
    MinAz             float64
    MaxAx             float64
    MaxAy             float64
    MaxAz             float64
    Lng               float64
    Lat               float64
    Heading           float64 
    Speed             float64  // Knots
    HDOP              float64 
    Quality           int     
    Satellites        int     
 }
```


You can find the source code for trip data post enrichment at:
[Enriched Trip Data]({{ site.baseurl }}{% link trip-schema/enriched.html %}) /
