---
layout: default
title: Track Job (MQTTS)
parent: Endpoints
nav_order: 7
---

# Track Job Status (MQTTS)

Real-time job status updates via MQTT over TLS.

| Detail | Specification |
|:-------|:--------------|
| **Protocol** | MQTTS |
| **Topic** | `{jobID}/JOB/STATUS` |
| **QoS** | 1 (At least once) |
| **Security** | JWT (as password) / TLS |
| **Frequency** | 1 Hz |

## Subscription Example

```
Topic: job-f47ac10b-58cc-4372-a567-0e02b2c3d479/JOB/STATUS
```

## Message Payload

```json
{
  "jobState": "INPROGRESS",
  "droneState": "WAYPOINT",
  "dockState": "OPEN",
  "navigation": {
    "position": {
      "type": "Point",
      "coordinates": [-122.4194, 37.7749, 85.5]
    },
    "orientation": {
      "heading": 270.5,
      "pitch": -2.1,
      "roll": 0.3
    },
    "velocity": {
      "groundSpeed": 12.5,
      "verticalSpeed": -0.5
    }
  },
  "timestamp": "2025-11-27T10:30:15.123Z"
}
```
