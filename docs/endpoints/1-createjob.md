---
layout: default
title: 1. CreateJob
parent: Endpoints
nav_order: 1
---

# Mission Request (CreateJob)

Creates a new mission job for a specified device group.

| Detail | Specification |
|:-------|:--------------|
| **Method** | `POST` |
| **URI** | `/v3/jobs/create` |
| **Content-Type** | `application/json` |
| **Security** | JWT Auth, Mandatory `Idempotency-Key` header |
| **Success Response** | `201 Created` |

## Request Body

| Field | Type | Mandatory | Description |
|:------|:-----|:----------|:------------|
| `DeviceGroupID` | UUID | Yes | The ID of the drone/dock to be commanded |
| `MissionID` | UUID | Conditional | Required if `ActionOnArrival` is `MISSION` |
| `DesiredLocation` | GeoJSON Point | Conditional | Required if `ActionOnArrival` is `HOVER` or `ORBIT` |
| `ActionOnArrival` | Enum | Yes | `HOVER`, `ORBIT`, or `MISSION` |

## Example Request

```json
{
  "DeviceGroupID": "b775cc6e-1234-5678-90ab-cdef12345678",
  "MissionID": "a1b2c3d4-5678-90ab-cdef-123456789012",
  "ActionOnArrival": "MISSION"
}
```

## Example with Location

```json
{
  "DeviceGroupID": "b775cc6e-1234-5678-90ab-cdef12345678",
  "DesiredLocation": {
    "type": "Point",
    "coordinates": [-122.4194, 37.7749, 100]
  },
  "ActionOnArrival": "HOVER"
}
```

## Success Response (201)

```json
{
  "jobID": "job-f47ac10b-58cc-4372-a567-0e02b2c3d479",
  "status": "PENDING",
  "createdAt": "2025-11-27T10:30:00Z",
  "DeviceGroupID": "b775cc6e-1234-5678-90ab-cdef12345678",
  "_links": {
    "self": {
      "href": "/v3/jobs/job-f47ac10b-58cc-4372-a567-0e02b2c3d479"
    },
    "status_ws": {
      "href": "/v3/jobs/job-f47ac10b-58cc-4372-a567-0e02b2c3d479/status/ws"
    },
    "status_mqtt": {
      "topic": "job-f47ac10b-58cc-4372-a567-0e02b2c3d479/JOB/STATUS"
    }
  }
}
```
