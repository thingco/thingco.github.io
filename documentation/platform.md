---
layout: page
title: Platform Info
permalink: /platform
---

ThingCo Platform
================

ThingCo's platform is build upon an event-driven architecture leveraging AWS SNS as the backbone event bus. This means that a partner is able to subscribe to a range of topics and securely receieve messages in real time. This page will outline the available topics along with the message schema that is published.

The ARN of a Topic is contructed as follows `arn:aws:sns:${AWS::Region}:${AWS::AccountId}:SNS_TOPIC_NAME`, and should be used if adding a trigger to a lambda function. 

### Stack Details

To ensure data segregation ThingCo deploy each partners stack in a seperate AWS account and maintian a strict POLP model for accessing data. Details of the installation are as follows.

##### AccountId: 000000000000
##### Region: eu-west-1

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
    "erraticEvents": {
      "braking1": 56,
      "braking2": 2,
      "speedingUp1": 241,
      "speedingUp2": 4
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

### Device Connected
When the device completes a trip by having been stationary for 10 minuets. It connects to the platform and sends a message before the remaining trip data.

**Topic Name:** ${AWS::Region}:${AWS::AccountId}-DeviceConnectedTopic-Topic-${UID}

```json
{
  "box_id": "VALID-DEVICE-1",
  "location": [-1.403091, 55.014623],
  "timestamp": "1234567890123",
  "trip_id": "VALID-TRIP-ID-1",
  "trip_start": "1234567890123",
  "version": ["1.0.0"],
  "connection_type": 1
}
```

### Device Disconnected
The device has finished sending trip data and is switching to sleep mode.

**Topic Name:** ${AWS::Region}:${AWS::AccountId}-DeviceDisconnectedTopic-Topic-${UID}

```json
{
  "box_id": "VALID-DEVICE-1",
  "location": [-1.403091, 55.014623],
  "timestamp": "1234567890123",
  "trip_id": "VALID-TRIP-ID-1",
  "trip_start": "1234567890123",
  "version": ["1.0.0"],
  "connection_type": 4
}
```

### Device Installed
The customer has installed their device.

**Topic Name:** ${AWS::Region}:${AWS::AccountId}-DeviceInstallationTopic-Topic-${UID}

```json
{
  "boxID": "VALID-DEVICE-1",
  "personID": "VALID-PERSON-ID-1"
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
The customer has exceeded the excessive speeding criteria. 48hrs after the inital event, the cancel field will be marked as true.

**Topic Name:** ${AWS::Region}:${AWS::AccountId}-ExcessiveSpeedingTopic-Topic-${UID}

```json
{
  "boxID": "VALID-DEVICE-1",
  "personID": "VALID-PERSON-ID-1",
  "cancel": false,
  "location":{"lat": 55.008765, "lng":-1.456833},
  "timestamp": "1597088197000",
  "roadName": "Hawkeys Lane",
  "roadSpeed": 113,
  "roadType": "B",
  "speed": 169
}
```

### Matched Points
Trip points post ThingCo enrichment.

**Topic Name:** ${AWS::Region}:${AWS::AccountId}-MatchedPointsTopic-Topic-${UID}

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

### Persistent Speeding
The customer has not improved their persistent speeding pattern over a number of blocks and should be cancelled.

**Topic Name:** ${AWS::Region}:${AWS::AccountId}-PersistentSpeedingTopic-Topic-${UID}

```json
{
  "boxID": "VALID-DEVICE-1",
  "personID": "VALID-PERSON-ID-1",
  "timestamp": "1597088197000",
  "cancel": true // false - 9 blocks | true - 12 blocks
}
```

### Persistent Speeding Events
The customer has been speeding persistently over the course of a trip

**Topic Name:** ${AWS::Region}:${AWS::AccountId}-PersistentSpeedingEventsTopic-Topic-${UID}

```json
{
  "boxID": "VALID-DEVICE-1",
  "personID": "VALID-PERSON-ID-1",
  "tripID": "VALID-TRIP-ID",
  "blockID": "VALID-BLOCK-ID",
  "events": [
    {
      "location": [-4.18974, 51.01756],
      "placename": "A1",
      "timestamp": "1597088197000"
    }
  ]
}
```

### Standard Points
Trip points after basic validation.

**Topic Name:** ${AWS::Region}:${AWS::AccountId}-StandardPointsTopic-Topic-${UID}

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
  "metadata": "ACCIDENT#VALID-ACCIDENT-ID-1",
  "personID": "VALID-PERSON-ID-1",
  "timestamp": "1580405492910",
  "placename": "Newbury",
  "tripID": "VALID-TRIP-ID-1",
  "tripStartTime": "1551442483000",
  "location": { "Lat": 51.30144, "Lon": -1.33661 },
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
  "boxID": "VALID-DEVICE-1",
  "personID": "VALID-PERSON-ID-1"
}
```

### Trip Complete
The device has complted a trip and all data has been receieved.

**Topic Name:** ${AWS::Region}:${AWS::AccountId}-TripCompleteTopic-Topic-${UID}

```json
{
  "tripID": "VALID-TRIP-ID-1",
  "boxID": "VALID-DEVICE-ID-1",
  "blockID":"VALID-BLOCK_ID-1",
  "personID": "VALID-PERSON-ID-1",
  "startTimestamp": "1606928671000",
  "endTimestamp": "1606935465000",
  "endPlacename": "Newcastle upon Tyne",
}
```

### Trip Events Processed
The latest trip has been checked for all event types

**Topic Name:** ${AWS::Region}:${AWS::AccountId}-TripEventsProcessedTopic-Topic-${UID}

```json
{
  "PersonID": "VALID-PERSON-ID-1",
  "TripID": "VALID-TRIP-ID-1",
  "Distance": 10.1,
  "Duration": 60,
  "StartLocation":{
    "LON":-1.500000,
    "LAT":55.000000
  },
  "StartPlacename:"Newcastle",
  "StartTimestamp": "1234567890123",
  "EndLocation":{
    "LON":-1.500000,
    "LAT":55.000000
  },
  "EndPlacename:"Newcastle",
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
