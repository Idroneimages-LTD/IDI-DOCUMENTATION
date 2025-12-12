---
layout: default
title: Resources - Get Device Groups
parent: Endpoints
nav_order: 17
---

# Get Resources (Device Groups)

Retrieve all device groups available to your organisation.

| Detail | Specification |
|:-------|:--------------|
| **Method** | `GET` |
| **URI** | `/v3/resources` |
| **Security** | JWT Bearer Token |
| **Success Response** | `200 OK` |

## Example Request

```bash
curl -X GET {{baseUrl}}/v3/resources \
  -H "Authorization: Bearer {{accessToken}}"
```

## Success Response (200)

```json
{
  "success": true,
  "resources": [
    {
      "deviceGroupId": "group-uuid-123",
      "name": "Fleet Alpha",
      "description": "Primary inspection fleet",
      "deviceCount": 5,
      "devices": [
        {
          "deviceId": "drone-001",
          "name": "Mavic 3 Enterprise",
          "status": "online"
        }
      ]
    }
  ],
  "count": 1
}
```

| Field | Type | Description |
|:------|:-----|:------------|
| `success` | Boolean | Request success status |
| `resources` | Array | List of device group objects |
| `count` | Integer | Total number of device groups |

## Device Group Object

| Field | Type | Description |
|:------|:-----|:------------|
| `deviceGroupId` | String | Unique identifier for the device group |
| `name` | String | Display name |
| `description` | String | Description of the group |
| `deviceCount` | Integer | Number of devices in the group |
| `devices` | Array | List of devices in the group |
