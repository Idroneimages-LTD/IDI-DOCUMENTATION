---
layout: default
title: 4. Track Resource (MQTTS)
parent: Endpoints
nav_order: 4
---

# Track Resource Status (MQTTS)

Continuous telemetry stream from a device group via MQTT.

| Detail | Specification |
|:-------|:--------------|
| **Protocol** | MQTTS |
| **Topic** | `{DeviceGroupID}/DEVICE/STATUS` |
| **QoS** | 1 |
| **Security** | JWT (as password) / TLS |
| **Frequency** | 1 Hz |

## Message Payload

```json
{
  "droneState": "GPS_NORMAL",
  "dockState": "CLOSED",
  "navigation": {
    "position": {
      "type": "Point",
      "coordinates": [-122.4194, 37.7749, 0]
    },
    "orientation": {
      "heading": 0,
      "pitch": 0,
      "roll": 0
    }
  },
  "weather": {
    "windSpeed": 5.2,
    "windDirection": 180,
    "temperature": 22.5,
    "humidity": 65,
    "rain": false
  },
  "timestamp": "2025-11-27T10:30:15.123Z"
}
```
