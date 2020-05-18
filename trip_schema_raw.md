---
layout: page
title: Trip Schema - Raw
permalink: /trip-schema/raw
---

This schema matches exactly what comes from the device post de-compression. 

```go
type Data struct {
    DeviceID  string  `codec:"-" validate:"nonzero"`
    TripStart int     `validate:"nonzero"`
    Points    []Point `validate:"nonzero"`
 }
  
 //Point is raw data collected by a little theo device
 type Point struct {
    Timestamp         int     `codec:"timestamp" validate:"nonzero"`
    BatteryVoltage    int     `codec:"batteryVoltage"`
    BatteryPercentage int     `codec:"batteryPercentage"`
    Ax                float64 `codec:"ax"`
    Ay                float64 `codec:"ay"`
    Az                float64 `codec:"az"`
    MinAx             float64 `codec:"minAx"`
    MinAy             float64 `codec:"minAy"`
    MinAz             float64 `codec:"minAz"`
    MaxAx             float64 `codec:"maxAx"`
    MaxAy             float64 `codec:"maxAy"`
    MaxAz             float64 `codec:"maxAz"`
    Lng               float64 `codec:"lng"`
    Lat               float64 `codec:"lat"`
    Heading           float64 `codec:"heading"`
    Speed             float64 `codec:"speed"` // Knots
    HDOP              float64 `codec:"hdop"`
    Quality           int     `codec:"quality"`
    Satellites        int     `codec:"satellites" validate:"nonzero"`
 }
```


You can find the source code for trip data post enrichment at:
[Enriched Trip Data]({{ site.baseurl }}{% link trip-schema/enriched.html %}) /
