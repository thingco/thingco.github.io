array (len: 6) [
int32: Timestamp
uint16: Battery Voltage (millivolts)
uint8: Battery Charge Level (percent)
int16: Accelerometer X axis reading (in 4096ths of 1G) int16: As above, for Y axis
int16: As above, for Z axis
]

---
layout: page
title: Tamper Alerts
permalink: /tamper
---

If a device is removed from the windscreen, a tamper alert is sent through to the platform. Due to the device's independance, both from a power and connectivity perspective, the alert is always sent. 

```graphql
  
type Tamper {
  TamperTimestamp: String
  BatteryVoltage    int
  BatteryPercentage int
  X int //Accelerometer X axis reading (in 4096ths of 1G)
  Y int //Accelerometer Y axis reading (in 4096ths of 1G)
  Z int //Accelerometer Z axis reading (in 4096ths of 1G)
}

```

<h2>Sample data</h2>

```
1589961563 4182 99 -8 12 4
```