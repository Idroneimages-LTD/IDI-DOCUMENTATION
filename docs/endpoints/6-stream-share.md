---
layout: default
title: 6. STREAM_SHARE
parent: Endpoints
nav_order: 6
---

# STREAM_SHARE (REST)

Generates a limited-time credential for external access to a resource stream.

| Detail | Specification |
|:-------|:--------------|
| **Method** | `POST` |
| **URI** | `/v3/streams/share` |
| **Security** | JWT Auth (requires `MANAGER` role) |
| **Success Response** | `201 Created` |

## Request Body

| Field | Type | Mandatory | Description |
|:------|:-----|:----------|:------------|
| `DeviceGroupID` | UUID | Yes | The resource whose data stream is being shared |
| `role` | Enum | Yes | Access level: `VIEWER`, `PILOT`, or `MANAGER` |
| `timeout` | Integer | Yes | Duration in minutes for which the share is valid |

## Example Request

```json
{
  "DeviceGroupID": "b775cc6e-1234-5678-90ab-cdef12345678",
  "role": "VIEWER",
  "timeout": 60
}
```

## Success Response (201)

```json
{
  "shareID": "share-abc123def456",
  "shareLink": "https://app.idi-fly.com/shared/share-abc123def456",
  "expiresAt": "2025-11-27T11:30:00Z",
  "role": "VIEWER",
  "DeviceGroupID": "b775cc6e-1234-5678-90ab-cdef12345678",
  "_links": {
    "status_ws": {
      "href": "/v3/shares/share-abc123def456/status/ws"
    },
    "status_mqtt": {
      "topic": "share-abc123def456/SHARE/STATUS"
    }
  }
}
```
