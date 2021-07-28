---
layout: page
title: Platform Info
permalink: /platform
---

ThingCo Platform
================

ThingCo's platform is build upon an event-driven architecture leveraging AWS SNS as the backbone event bus. This means that a partner is able to subscribe to a range of topics and securely receive messages in real time. This page will outline the available topics along with the message schema that is published.

The ARN of a Topic is constructed as follows `arn:aws:sns:${AWS::Region}:${AWS::AccountId}:SNS_TOPIC_NAME`, and should be used if adding a trigger to a lambda function.

### Stack Details

To ensure data segregation ThingCo deploy each partners stack in a separate AWS account and maintain a strict POLP model for accessing data. Details of the installation are as follows.

##### AccountId: 000000000000
##### Region: eu-west-1

### Accident Voice Received

**Topic Name:** ${AWS::Region}:${AWS::AccountId}-AccidentVoiceReceivedTopic-Topic-${UID}

```json
{
	"AccidentID":            "VALID-ACCIDENT-ID-1",
	"BoxID":                 "VALID-BOX-ID-1",
	"PersonID":              "VALID-PERSON-ID-1",
	"TripID":                "VALID-TRIP-ID-1",
	"AccidentConfirmed":     "Yes", //Yes, No, Unresponsive
	"AccidentLocation":      "52.485832,-1.893705",
	"AccidentPlacename":     "Birmingham",
	"AccidentSpeed":         "63",
	"AccidentTime":          "1608188020000",
	"BoxContactNumber":      "+431234567890123",
	"BoxStatus":             "INSTALLED", //TAMPERED, INSTALLED, UNREGISTERED
	"BoxType":               "LittleTheo",
	"CustomerFirstName":     "Matt",
	"CustomerLastName":      "Masters",
	"CustomerAddressFull":   "Suite 1, neon, Quorum Park, Newcastle, NE12 8BU",
	"CustomerPostcode":      "NE12 8BU",
	"ContactCustomer":       "",
	"ContactNumber":         "1234", // Last 4 digits
	"ContactNumberFull":     "+447777771234",
	"CustomerEmail":         "support@drivetheo.com",
	"PolicyNumber":          "PolicyNumber",
	"PolicyMileage":         "PolicyMileage",
	"VRN":                   "TE57 1AB",
	"VehicleMake":           "Tesla",
	"VehicleModel":          "Model 3",
	"VehicleRegisteredYear": "2020",
	"ResponderLambda":       "Function:ARN"
}
```

### App Open
Posted to each time a user opens the mobile App

**Topic Name:** ${AWS::Region}:${AWS::AccountId}-AppOpenTopic-Topic-${UID}

```json
{
  "personID": "VALID-PERSON-ID-1",
  "timestamp": "1605889095"
}
```

### Battery Data
A devices charge level at the end of a trip.

**Topic Name:** ${AWS::Region}:${AWS::AccountId}-BatteryDataTopic-Topic-${UID}

```json
{
    "battery_percent": 100,
    "battery_voltage": 4400,
    "box_id": "VALID-DEVICE-1",
    "connection_type": 4,
    "location": {
      "lat": 55.000000,
      "lon": -1.500000
    },
    "person_id": "VALID-PERSON-ID-1",
    "timestamp": "1234567890123",
    "trip_id": "VALID-TRIP-ID-1",
    "trip_start": "1234567890123",
    "version": [ "v1-0" ]
}
```

### Block Distance Complete
The user has completed a block ~100 miles, and it is now ready to be scored. A block is made up of a minumium of 100 miles and will be scored as soon as the last trip in the block is completed. This means that if the user has driven 95 miles in there first block, then dirves a 15 mile trip the final block total will be 110 miles. This is to avoid the trip appearing in multiple blocks.

**Topic Name:** ${AWS::Region}:${AWS::AccountId}-BlockDistanceCompleteTopic-Topic-${UID}
***Trigger:*** On Block Complete

```json
{
  "blockID": "VALID-BLOCK-ID-1",
  "createdAt": "1234567890123",
  "distance": 167452.82438520904,
  "duration": 8038,
  "metadata": "BLOCK#COMPLETE#2006-02-01@15:04:05#VALID-BLOCK-ID-1",
  "personID": "VALID-PERSON-ID-1",
  "scored": false,
  "trips": ["VALID-TRIP-ID-1", "VALID-TRIP-ID-2", "VALID-TRIP-ID-3"]
}
```

### Block Score Complete
Published to once the latest completed block has been scored.

**Topic Name:** ${AWS::Region}:${AWS::AccountId}-BlockScoreCompleteTopic-Topic-${UID}
***Trigger:*** On Block Complete

```json
{
  "blockID": "VALID-BLOCK-ID-1",
  "createdAt": "1234567890123",
  "distance": 167452.82438520904,
  "duration": 8038,
  "eventCounts": {
    "DrivingDuration": 99,
    "ErraticDriving": 99,
    "Speeding": 99,
    "TimeOfDay": 99
  },
  "eventDetails": {
    "durationEvents": 0.0,
    "ecoEvents": {
      "sd": 0.19578,
      "mean": -0.0731159
    },
    "speedingEvents": {
      "persistent": 32.0,
      "newSegment": 29.0
    },
    "timeOfDayEvents": {
      "weekdayUnderSpeed": [
        {
          "count": 245.0,
          "from": 0.0,
          "to": 3.0
        },
        {
          "count": 2495.0,
          "from": 12.0,
          "to": 14.0
        }
      ],
      "weekdayOverSpeed": [
        {
          "count": 663.0,
          "from": 0.0,
          "to": 3.0
        }
      ]
    }
  },
  "metadata": "BLOCK#COMPLETE#2006-02-01@15:04:05#VALID-BLOCK-ID-1",
  "personID": "VALID-PERSON-ID-1",
  "scored": true,
  "scores": {
    "DrivingDuration": 100,
    "ErraticDriving": 100,
    "OverallScore": 100,
    "Speeding": 100,
    "TimeOfDay": 100
  },
  "trips": ["VALID-TRIP-ID-1", "VALID-TRIP-ID-2", "VALID-TRIP-ID-3"]
}
```

### Delivery Driver Notification
A trip has been flagged for delivery driver behaviour

**Topic Name:** Please ask your Account Manager 

```json
{
   "PersonID":"VALID-PERSON-1",
   "TripID":"VALID-TRIP-1",
}
```

### Device Allocation
A Device has been allocated and shipped to a customer.

**Topic Name:** ${AWS::Region}:${AWS::AccountId}:DeviceAllocation

```json
{
   "ID":"1234567890", //RoyalMail 1D ID. Can be used for tracking (will expire)
   "Metadata":"FULFILMENT#REQUEST",
   "AccountID":"1234567890",
   "BoxSerial":"VALID-DEVICE-1",
   "Expiry":0,
   "InvoiceID":"1234567890",
   "IsRefurb":false,
   "PersonID":"VALID-PERSON-1",
   "Reference":"POLICY-NUMBER-1",
   "Status":"Shipped",
   "Summary":"The sender has let us know this item will be with us soon.",
   "UniqueID":"1234567890" // Unique package 2D ID. Can be used for tracking (won't expire)
}
```

### Device Connected
When the device completes a trip by having been stationary for 10 minuets. It connects to the platform and sends a message before the remaining trip data.

**Topic Name:** ${AWS::Region}:${AWS::AccountId}-DeviceConnectedTopic-Topic-${UID}

```json
{
  "box_id": "VALID-DEVICE-1",
  "person_id": "VALID-PERSON-1",
  "location": {
    "lon": -1.403091,
    "lat": 55.014623
   },
  "timestamp": "1234567890123",
  "trip_id": "VALID-TRIP-ID-1",
  "trip_start": "1234567890123",
  "version": ["1.0.0"],
  "battery_percent": 95,
  "battery_voltage": 4074,
  "connection_type": 1
}
```

### Device Disconnected
The device has finished sending trip data and is switching to sleep mode.

**Topic Name:** ${AWS::Region}:${AWS::AccountId}-DeviceDisconnectedTopic-Topic-${UID}

```json
{
  "box_id": "VALID-DEVICE-1",
  "person_id": "VALID-PERSON-1",
  "location": {
    "lon": -1.403091,
    "lat": 55.014623
   },
  "timestamp": "1234567890123",
  "trip_id": "VALID-TRIP-ID-1",
  "trip_start": "1234567890123",
  "version": ["1.0.0"],
  "battery_percent": 95,
  "battery_voltage": 4074,
  "connection_type": 1
}
```

### Device Installed
The customer has installed their device.

**Topic Name:** ${AWS::Region}:${AWS::AccountId}-DeviceInstallationTopic-Topic-${UID}

```json
{
  "ax": -0.037109375,
  "ay": -0.056884765625,
  "az": -0.9645580649375916,
  "batteryPercentage": 98,
  "batteryVoltage": 4091,
  "boxID": "VALID-DEVICE-1",
  "personID": "VALID-PERSON-1",
  "timestamp": 1607346410,
  "isFirst":true
}
```

### Device Offline
The customer has not responded to the 7 day chasers, that the device is offline.

Types: `LOW_BATTERY`, `NO_INSTALL`, `TAMPER`

**Topic Name:** ${AWS::Region}:${AWS::AccountId}-DeviceOfflineCanxTopic-Topic-${UID}

```json
{
  "boxID": "VALID-DEVICE-1",
  "personID": "VALID-PERSON-ID-1",
  "eventType": "TAMPER"
}
```

### Device Returned
ThingCo has received the customers device and wether it can be re-used.

**Topic Name:** ${AWS::Region}:${AWS::AccountId}-DeviceReturnedTopic-Topic-${UID}

```json
{
  "boxID": "VALID-DEVICE-1",
  "personID": "VALID-PERSON-ID-1",
  "damaged": false
}
```

### Excessive Speeding
The customer has exceeded the excessive speeding criteria. 48hrs after the initial event, the cancel field will be marked as true.

**Topic Name:** ${AWS::Region}:${AWS::AccountId}-ExcessiveSpeedingTopic-Topic-${UID}
***Trigger:*** On Trip Complete
***Trigger Conditions:*** Excessive speeding campaign conditions are met

```json
{
  "boxID": "VALID-DEVICE-1",
  "personID": "VALID-PERSON-ID-1",
  "cancel": false,
  "location":{"lat": 55.008765, "lon":-1.456833},
  "timestamp": "1597088197000",
  "eventTimestamp": "1597088197000",
  "roadName": "Hawkeys Lane",
  "roadSpeed": 113,
  "roadType": "B",
  "speed": 169
}
```

### Excessive Speeding Events
The customer has been speeding excessively over the course of a trip

**Topic Name:** ${AWS::Region}:${AWS::AccountId}-ExcessiveSpeedingEventsTopic-Topic-${UID}
***Trigger:*** On Trip Complete
***Trigger Conditions:*** At least one excessive speeding event recorded during the trip

```json
{
  "boxID": "VALID-DEVICE-1",
  "personID": "VALID-PERSON-ID-1",
  "tripID": "VALID-TRIP-ID",
  "blockID": "VALID-BLOCK-ID",
  "events": [
    {
      "location": {"lat":55.06653, "lon":-1.586},
      "placename": "A19",
      "timestamp": "1611756533000",
      "speed": 133,
      "roadspeed": 64,
      "points": [
        {
          "Timestamp": "2021-01-27T14:08:53Z",
          "Location": [-1.586, 55.06653],
          "AdjustedDistance": 4.58,
          "Confidence": 1,
          "RoadSpeed": 64,
          "RoadAvgSpeed": 0,
          "Speed": 133,
          "Speeding": true,
          "RoadName": "A19",
          "RoadType": "A-Road",
          "PostCode": "NE23 7"
        },
        {
          "Timestamp": "2021-01-27T14:08:54Z",
          "Location": [-1.58545, 55.06647],
          "AdjustedDistance": 4.58,
          "Confidence": 1,
          "RoadSpeed": 64,
          "RoadAvgSpeed": 0,
          "Speed": 130,
          "Speeding": true,
          "RoadName": "A19",
          "RoadType": "A-Road",
          "PostCode": "NE23 7"
        }
      ]
    }
  ]
}
```

### Genuine Accident
Post processing using the accident sensor data to determain if we think this is a valid alert

**Topic Name:** ${AWS::Region}:${AWS::AccountId}-AccidentGenuineCheckOutputTopic-Topic-${UID}

```json
{
   "AccidentID":"VALID-ACCIDENT-ID",
   "PersonID":"VALID-PERSON-ID",
   "Accident":{
      "personID":"VALID-PERSON-ID",
      "altPK":"ACCIDENT",
      "acknowledgedSource":"VOICE",
      "metadata":"ACCIDENT#VALID-ACCIDENT-ID",
      "boxSerial":"VALID-BOX-ID",
      "accidentID":"VALID-ACCIDENT-ID",
      "acknowledgedAt":"1627466861281",
      "location":{
         "lon":-1.764406681060791,
         "lat":53.8424072265625
      },
      "incidentReference":"VALID-ACCIDENT-ID",
      "isConfirmed":false,
      "isAcknowledged":true,
      "isGenuine":false,
      "isUnresponsive":false,
      "placename":"Baildon",
      "speed":0,
      "timestamp":"1627466758000",
      "tripID":"VALID-TRIP-ID"
   },
   "SensorData":[
      
   ],
   "AbsoluteSensorData":[
      
   ],
   "MovingAverage":[
      
   ],
   "Gradient":[
      
   ],
   "GradientCrashRange":[
      
   ],
   "MaxGForce":{
      "ax":3.198974609375,
      "ay":-3.45751953125,
      "az":4.260009765625
   },
   "StandardDeviation":{
      "ax":0.34949267579944676,
      "ay":0.30384239284541514,
      "az":0.24995502234744554
   },
   "SensorMeanStartEnd":{
      "ax":[
         -0.2362060546875,
         -0.657275390625
      ],
      "ay":[
         0.178955078125,
         -0.11357421875
      ],
      "az":[
         0.097216796875,
         0.128466796875
      ]
   },
   "SensorChangeInMean":{
      "ax":false,
      "ay":false,
      "az":false
   },
   "DeltaV":2.957416519126759,
   "Genuine":false,
   "SpeedBump":false,
   "Roll":false,
   "Swerve":false,
   "Noisy":false,
   "Spikes":2,
   "NumberOfAbsolutePointsOverThreshold":0,
   "LocationInformation":{
      "RoadType":"A-Road",
      "Intersection":"",
      "SlipRoad":false,
      "SpeedCategory":[
         21,
         30
      ],
      "SpeedLimit":48,
      "NumberofLanes":"",
      "Urban":true
   },
   "ImpactDirection":{
      "primarydirection":"ax",
      "situation":"Rear"
   },
   "Tamper":false,
   "Level":2,
   "AlertsInLastTwoWeeks":1,
   "Orientated":true
}
```

### Matched Points
Trip points post ThingCo enrichment.

**Topic Name:** ${AWS::Region}:${AWS::AccountId}-MatchedPointsTopic-Topic-${UID}
***Trigger:*** On Trip Complete

```json
{
  "id": "VALID-TRIP-ID-1",
  "blockID": "VALID-BLOCK-ID-1",
  "boxID": "VALID-DEVICE-1",
  "startPlacename": "East the Water",
  "endPlacename": "Bideford",
  "matched": {
    "points": [
      {
        "Timestamp": "2019-08-08T05:38:24.0045Z",
        "Location": [-4.18974, 51.01756],
        "AdjustedDistance": 18.15,
        "Confidence": 0.28,
        "RoadSpeed": 64,
        "RoadAvgSpeed": 64,
        "Speed": 69,
        "Speeding": false,
        "RoadName": "Manteo Way",
        "RoadType": "UNKNOWN",
        "PostCode": "EX39 4"
      }
    ],
    "Summary": {
      "WarningCount": 0,
      "GroupedWarningCount": {},
      "Warnings": [],
      "TripGraph": {
        "graphData": [
          {
            "loc": [-4.18974, 51.01756],
            "ps": 69,
            "rs": 64
          }
        ]
      }
    }
  },
  "durationMap": {
    "1565242704004": 138
  },
  "distanceMap": {
    "1565242704004": 2035.6107122000003
  },
  "anomaly": false
}
```

### NightTime Speeding
The customer has surpassed the acceptable % of time over the limit and driving at night.
`stage` indicates the number of campaigns (3 consecutive blocks) the customer has met the criteria for

**Topic Name:** ${AWS::Region}:${AWS::AccountId}-NightSpeedingTopic-Topic-${UID}
***Trigger:*** On Block Complete
***Trigger Conditions:*** 3 new blocks completed and NightTime speeding campaign conditions are met

```json
{
  "personID": "VALID-PERSON-ID-1",
  "timestamp": "1597088197000",
  "policyNumber": "VALID-POLICY-NUMBER-1",
  "stage": 1
}
```

### Overnight Parking
The customers policy risk address is different from the vehicles most common overnight parked location.
`distanceFromRisk` in km

**Topic Name:** ${AWS::Region}:${AWS::AccountId}-OvernightParkingTopic-Topic-${UID}
***Trigger:*** On Trip Complete
***Trigger Conditions:*** 30 days since policy start / time last run and Overnight campaign conditions are met

```json
{
  "personID": "VALID-PERSON-ID-1",
  "timestamp": "1597088197000",
  "distanceFromRisk": 7.89,
  "riskLocation": {"lat": 55.02, "lon": -1.57},
  "riskPostcode": "NE1 2NH",
  "overnightLocation": {"lat": 55.17, "lon": -1.53},
  "overnightPostcode": "NE12 XYZ",
  "queryFrom": "2021/03/10",
  "queryTo": "2021/03/18",
  "durationParked": "9h12m0s",
  "results": [{"geohash":"gcvc4ck","count":6,"duration":2628,"breakdown":{"night":1080,"day":1548}}],
  "pctAtMostFrequent": 82.19
}
```

### Persistent Speeding
The customer has not improved their persistent speeding pattern over a number of blocks and should be cancelled.
`cancel` flag will be set to true only after 12 blocks have met the criteria

**Topic Name:** ${AWS::Region}:${AWS::AccountId}-PersistentSpeedingTopic-Topic-${UID}
***Trigger:*** On Block Complete
***Trigger Conditions:*** Minimum of 9 complete blocks have met the Persistent campaign criteria

```json
{
  "boxID": "VALID-DEVICE-1",
  "personID": "VALID-PERSON-ID-1",
  "timestamp": "1597088197000",
  "cancel": true,
  "redEventCount": 4
}
```

### Persistent Speeding Events
The customer has been speeding persistently over the course of a trip

**Topic Name:** ${AWS::Region}:${AWS::AccountId}-PersistentSpeedingEventsTopic-Topic-${UID}
***Trigger:*** On Trip Complete
***Trigger Conditions:*** At least one persistent speeding event recorded during the trip

```json
{
  "boxID": "VALID-DEVICE-1",
  "personID": "VALID-PERSON-ID-1",
  "tripID": "VALID-TRIP-ID",
  "blockID": "VALID-BLOCK-ID",
  "events": [
    {
      "location": {"lat":51.01756, "lon":-4.18974},
      "placename": "A1",
      "timestamp": "1597088197000"
    }
  ]
}
```

### Shipment Created

**Topic Name:** ${AWS::Region}:${AWS::AccountId}-ShipmentCreated
***Trigger:*** On shipment label creation

```json
{
   "ID":"1234567890", //RoyalMail 1D ID. Can be used for tracking
   "Metadata":"FULFILMENT#REQUEST",
   "AccountID":"1234567890",
   "PersonID":"VALID-PERSON-1",
   "Reason":"NEW_BIZ", // NEW_BIZ, COV, REPLACEMENT, WARRANTY, RETURN_PACK, CUSTOMER_RETURN
   "Reference":"POLICY-NUMBER-1",
   "UniqueID":"1234567890", // Unique package 2D ID. Can be used for tracking
   // Fields below here are *Not Used* on this topic but will appear in the message payload.
   "BoxSerial":"",
   "Expiry":0,
   "InvoiceID":"",
   "IsRefurb":false,
   "Status":"",
   "Summary":""
}
```

### Standard Points
Trip points after basic validation.

**Topic Name:** ${AWS::Region}:${AWS::AccountId}-StandardPointsTopic-Topic-${UID}
***Trigger:*** On Trip Complete

```json
{
  "id": "VALID-TRIP-ID-1",
  "blockID": "VALID-BLOCK-ID-1",
  "boxID": "VALID-DEVICE-1",
  "startPlacename": "East the Water",
  "endPlacename": "Bideford",
  "points": [
    {
      "TripID": "VALID-TRIP-ID-1",
      "BoxID": "VALID-DEVICE-1",
      "PointType": 3,
      "EventType": 0,
      "Timestamp": "2019-08-08T05:38:24.0045Z",
      "HDOP": 1.1,
      "NumberOfSats": 5,
      "Location": [-4.18988, 51.01747],
      "Heading": 315,
      "Valid": true,
      "Speed": 69,
      "SpeedLimit": 64,
      "RoadType": "UNKNOWN",
      "RoadName": "Manteo Way",
      "PostCode": "EX39 4",
      "NumberOfLanes": 0,
      "Distance": 0,
      "Sensors": {
        "ax": [-0.115, -0.306, 0.045],
        "ay": [-0.084, -0.242, 0.087],
        "az": [0.986, 0, 1.089],
        "gx": [-0.0004, -0.0084, 0.008],
        "gy": [-0.0018, -0.011, 0.0065],
        "gz": [-0.0001, -0.0044, 0.0049]
      },
      "SignalQuality": -51
    }
  ],
  "durationMap": {
    "1565242704004": 138
  },
  "distanceMap": {
    "1565242704004": 2035.6107122000003
  },
  "anomaly": false
}
```

### Standardised Accident
Intial alert summary message sent as soon as the device detects an impact.

**Topic Name:** ${AWS::Region}:${AWS::AccountId}-StandardisedAccidentTopic-Topic-${UID}

```json
{
  "accidentID": "VALID-ACCIDENT-ID-1",
  "boxSerial": "VALID-BOX-ID-1",
  "metadata": "ACCIDENT#VALID-ACCIDENT-ID-1",
  "personID": "VALID-PERSON-ID-1",
  "timestamp": "1580405492910",
  "placename": "Newbury",
  "tripID": "VALID-TRIP-ID-1",
  "tripStartTime": "1551442483000",
  "location": { "lat": 51.30144, "lon": -1.33661 },
  "speed": 34
}
```

### Standardised Sensor
The sensor data relating to an alert summary. This sent after the summary to ensure the first message is received in low signal scenarios.

**Topic Name:** ${AWS::Region}:${AWS::AccountId}-StandardisedSensorTopic-Topic-${UID}

```json
{
  "accidentID": "VALID-ACCIDENT-ID-1",
  "metadata": "ACCIDENT#VALID-ACCIDENT-ID-1",
  "personID": "VALID-PERSON-ID-1",
  "sensorData": [
    {
      "t": "1588251559000",
      "s": 34,
      "l":{"lat": 55.000, "lon":-1.500},
      "ax": 0.123,
      "ay": 0.456,
      "az": 1.789,
      "gx": 0,
      "gy": 0,
      "gz": 0
    }
  ]
}
```

### Tamper
The device has sent a tamper notification.

**Topic Name:** ${AWS::Region}:${AWS::AccountId}-TamperTopic-Topic-${UID}

```json
{
    "ax": 63,
    "ay": -16,
    "az": -17,
    "batteryPercentage": 49,
    "batteryVoltage": 3777,
    "boxID": "VALID-DEVICE-1",
    "personID": "VALID-PERSON-ID-1",
    "timestamp": 1618762362
}
```

### Tracking Updates
Tracking and delivery updates, for new policy sales and CoV. ID & UniqueID can be used to track a shipment using [Royal Mail Tracking.](https://www.royalmail.com/track-your-item#/) However, ID is a temporary reference and is recycled. UniqueID does not expire.

**Topic Name:** ${AWS::Region}:${AWS::AccountId}:TrackingUpdates

```json
{
  "ID": "VALID-1D-TRACKING-ID",
  "Metadata": "1611843954216115408",
  "AccountID": "AWS-ACCOUNT-ID",
  "BoxSerial": "VALID-DEVICE-1",
  "Expiry": 1643379954,
  "InvoiceID": "1234567890",
  "IsRefurb": false,
  "PersonID": "VALID-PERSON-ID-1",
  "Reference": "POLICY-NUMBER",
  "Status": "In Transit",
  "Summary": "We have your item at Tyneside MC and it's on its way. ",
  "UniqueID": "VALID-2D-TRACKING-ID"
}
```

### Trip Complete
The device has complted a trip and all data has been receieved.

**Topic Name:** ${AWS::Region}:${AWS::AccountId}-TripCompleteTopic-Topic-${UID}
***Trigger:*** On Trip Complete

```json
{
  "id": "VALID-TRIP-ID-1",
  "boxID": "VALID-DEVICE-ID-1",
  "blockID":"VALID-BLOCK_ID-1",
  "personID": "VALID-PERSON-ID-1",
  "startTimestamp": "1606928671000",
  "endTimestamp": "1606935465000",
  "endPlacename": "Newcastle upon Tyne"
}
```

### Trip Events Processed
The latest trip has been checked for all event types

**Topic Name:** ${AWS::Region}:${AWS::AccountId}-TripEventsProcessedTopic-Topic-${UID}
***Trigger:*** On Trip Complete

```json
{
  "PersonID": "VALID-PERSON-ID-1",
  "TripID": "VALID-TRIP-ID-1",
  "Distance": 10.1,
  "Duration": 60,
  "StartLocation":{
    "LON": -1.500000,
    "LAT": 55.000000
  },
  "StartPlacename": "Newcastle",
  "StartTimestamp": "1234567890123",
  "EndLocation": {
    "LON": -1.500000,
    "LAT": 55.000000
  },
  "EndPlacename": "Newcastle",
  "EndTimestamp": "1234567890123",
  "BlockID": "VALID-BLOCK-ID-1",
  "Count": 1,
  "IsPerfect": true,
  "Events": [
    {
      "Event": "SPEEDING",
      "Count": 1
    }
  ],
  "Polyline": "BASE64-ENCODED-POLYLINE",
}
```
