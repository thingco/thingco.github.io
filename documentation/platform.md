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

To ensure data segregation ThingCo deploy each partners stack in a seperate AWS account and maintian a strict POLP model for accessing data. Details of the Freedom installation are as follows.

##### AccountId: 453340648180
##### Region: eu-west-1

### Battery Data
A devices charge level at the end of a trip.

**Topic Name:** Production-policywise-BatteryDataTopic-Topic-1Q1MSZTCRC259

```json
{
  "deviceID": "VALID-DEVICE-1",
  "personID": "VALID-PERSON-ID-1",
  "tripID": "VALID-TRIP-ID-1",
  "tripStart": 12031999,
  "points": [
    {
      "timestamp": 123456789,
      "batteryVoltage": 4600,
      "batteryPercentage": 100,
      "ax": 1.2,
      "ay": 3.4,
      "az": 5.6,
      "lng": 7.8,
      "lat": 9.1,
      "heading": 0.9,
      "speed": 12.3,
      "hdop": 45.6,
      "quality": 9,
      "satellites": 7
    }
  ]
}
```

### Block Distance Complete
The user has completed a block ~100 miles, and it is now ready to be scored. A block is made up of a minumium of 100 miles and will be scored as soon as the last trip in the block is completed. This means that if the user has driven 95 miles in there first block, then dirves a 15 mile trip the final block total will be 110 miles. This is to avoid the trip appearing in multiple blocks.

**Topic Name:** Production-policywise-BlockDistanceCompleteTopic-Topic-1M3FVUFA38BMR

```json
{
  "blockID": "VALID-BLOCK-ID-1",
  "createdAt": "1580931184425",
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

**Topic Name:** Production-policywise-BlockScoreCompleteTopic-Topic-1RSF8DYLXV9GV

```json
{
  "blockID": "VALID-BLOCK-ID-1",
  "createdAt": "1580931184425",
  "distance": 167452.82438520904,
  "duration": 8038,
  "eventCounts": {
    "DrivingDuration": 99,
    "ErraticDriving": 99,
    "Speeding": 99,
    "TimeOfDay": 99
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

**Topic Name:** Production-policywise-DeviceConnectedTopic-Topic-M8DJQC0E7OLL

```json
{
  "box_id": "VALID-DEVICE-1",
  "location": [-1.403091, 55.014623],
  "timestamp": "1234567890012",
  "trip_id": "VALID-TRIP-ID-1",
  "trip_start": "2109876543210",
  "version": ["1.0.0"],
  "connection_type": 1
}
```

### Device Disconnected
The device has finished sending trip data and is switching to sleep mode.

**Topic Name:** Production-policywise-DeviceDisconnectedTopic-Topic-8H78UMCCXQN2

```json
{
  "box_id": "VALID-DEVICE-1",
  "location": [-1.403091, 55.014623],
  "timestamp": "1234567890012",
  "trip_id": "VALID-TRIP-ID-1",
  "trip_start": "2109876543210",
  "version": ["1.0.0"],
  "connection_type": 4
}
```

### Device Installed
The customer has installed their device.

**Topic Name:** Production-policywise-DeviceInstallationTopic-Topic-5UAAT9U0P0NS

```json
{
  "boxID": "VALID-DEVICE-1",
  "personID": "VALID-PERSON-ID-1"
}
```

### Device Offline
The customer has not responded to the 7 day chasers, that the device is offline.

Types: `LOW_BATTERY`, `NO_INSTALL`, `TAMPER`

**Topic Name:** Production-policywise-DeviceOfflineCanxTopic-Topic-W5J4GDSHJZT3

```json
{
  "boxID": "VALID-DEVICE-1",
  "personID": "VALID-PERSON-ID-1",
  "eventType": "TAMPER"
}
```

### Device Returned
ThingCo has received the customers device and wether it can be re-used.

**Topic Name:** Production-policywise-DeviceReturnedTopic-Topic-OTRMBHX1Q3R4

```json
{
  "boxID": "VALID-DEVICE-1",
  "personID": "VALID-PERSON-ID-1",
  "damaged": false
}
```

### Excessive Speeding
The customer has exceeded the excessive speeding criteria. 48hrs after the inital event, the cancel field will be marked as true.

**Topic Name:** Production-policywise-ExcessiveSpeedingTopic-Topic-SL6UI5H8HGCY

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

**Topic Name:** Production-policywise-MatchedPointsTopic-Topic-YIPGEKFG9OH3

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

**Topic Name:** Production-policywise-PersistentSpeedingTopic-Topic-1DM9WC2BMNCL5

```json
{
  "boxID": "VALID-DEVICE-1",
  "personID": "VALID-PERSON-ID-1",
  "timestamp": "1597088197000"
}
```

### Standard Points
Trip points after basic validation.

**Topic Name:** Production-policywise-StandardPointsTopic-Topic-RXPQNF0CXJNN

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

**Topic Name:** Production-policywise-StandardisedAccidentTopic-Topic-SQ37CIUOMIL6

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

**Topic Name:** Production-policywise-StandardisedSensorTopic-Topic-13WK5CDGSG0VH

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

**Topic Name:** Production-policywise-TamperTopic-Topic-XIW9CF14VTHV

```json
{
  "boxID": "VALID-DEVICE-1",
  "personID": "VALID-PERSON-ID-1"
}
```

### Trip Complete
The device has complted a trip and all data has been receieved.

**Topic Name:** Production-policywise-TripCompleteTopic-Topic-1RW3N6CRAA9DL

```json
{
  "id": "VALID-TRIP-ID-1",
  "boxID": "VALID-DEVICE-ID-1",
  "personID": "VALID-PERSON-ID-1",
  "startPlacename": "Newcastle upon Tyne",
  "endPlacename": "Newcastle upon Tyne",
  "eventCount": 0,
  "perfect": false,
  "durationMap": {
    "1565242704004": 138
  },
  "distanceMap": {
    "1565242704004": 20350.6107122000003
  },
  "anomaly": false
}
```

### Trip Events Processed
The latest trip has been checked for all event types

**Topic Name:** Production-policywise-TripEventsProcessedTopic-Topic-1F5EUH6Y0N6U

```json
{
  "personID": "VALID-PERSON-ID-1",
  "tripID": "VALID-TRIP-ID-1",
  "blockID": "VALID-BLOCK-ID-1",
  "count": 1,
  "isPerfect": true,
  "events": [
    {
      "event": "SPEEDING",
      "count": 1
    }
  ]
}

```