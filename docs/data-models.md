---
layout: default
title: Data Models
nav_order: 7
---

# Data Models

## Job States

| State | Description |
|:------|:------------|
| `PENDING` | Job created, awaiting execution |
| `INPROGRESS` | Job is actively being executed |
| `COMPLETED` | Job finished successfully |
| `FAILED` | Job encountered an error |
| `CANCELLED` | Job was cancelled by user |

## Drone States

| State | Description |
|:------|:------------|
| `MANUAL` | Drone under manual control |
| `GPS_NORMAL` | GPS lock acquired, ready for autonomous operation |
| `WAYPOINT` | Following waypoint navigation |
| `HOVER` | Holding position in hover mode |
| `ORBIT` | Executing orbit pattern |
| `LANDED` | Drone has landed |
| `DOCKED` | Drone is docked in charging station |
| `RETURNING` | Returning to home/dock |

## Dock States

| State | Description |
|:------|:------------|
| `OPEN` | Dock is open |
| `CLOSED` | Dock is closed |
| `OPENING` | Dock is in process of opening |
| `CLOSING` | Dock is in process of closing |
| `COOLING` | Dock is cooling the drone |
| `CHARGING` | Dock is charging the drone |
| `TASK_INPROGRESS` | Dock is performing a task |

## GeoJSON Point

```json
{
  "type": "Point",
  "coordinates": [longitude, latitude, altitude]
}
```

- `longitude`: Decimal degrees (-180 to 180)
- `latitude`: Decimal degrees (-90 to 90)
- `altitude`: Meters above sea level (optional)

## Navigation Object

```json
{
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
}
```

## Weather Object

```json
{
  "windSpeed": 5.2,
  "windDirection": 180,
  "temperature": 22.5,
  "humidity": 65,
  "rain": false
}
```

| Field | Type | Unit | Description |
|:------|:-----|:-----|:------------|
| `windSpeed` | Float | m/s | Wind speed |
| `windDirection` | Integer | degrees | Wind direction (0-360) |
| `temperature` | Float | °C | Ambient temperature |
| `humidity` | Integer | % | Relative humidity (0-100) |
| `rain` | Boolean | - | Rain detected |
