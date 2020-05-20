---
layout: page
title: Alert Raw
permalink: /alert/raw
---

On detection of an alert, detected using a build in algorithm stored on the device, an initial message is sent to the platform, followed by a series of messages containing the sensor data.

The device records sensor data at 50Hz across both the accelerometer and gyroscope.

These messages can be pushed to a third party via a webhook, with the system able to an ID returned in the response for later communications relating to this alert.

```graphql
type Alert Message 1 {
  AccidentTimestamp: String
  TripStartTimestamp: String
  Latitude: float64
  Longitude: float64
  Speed: int //Knots
  AccMaxX: int
  AccMaxY: int
  AccMaxZ: int
}
```

```graphql
type Alert Detailed Data - 500 points at 50Hz[{
  Latitude: float64
  Longitude: float64
  Speed: int //Knots
  AccX: int
  AccY: int
  AccZ: int
  GyroX: int
  GyroY: int
  GyroZ: int
}]
```

<h2>Sample data</h2>

<h3>Raw data collected from devices sensors</h3>

<img src="/img/rawalert.png"/>

```
{% for row in site.data.rawcrashsensordata %}
{{ row }}
{% endfor %}
```