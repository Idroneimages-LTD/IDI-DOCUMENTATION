---
layout: default
title: 9. GET RESOURCES
parent: Endpoints
nav_order: 9
---

# GET RESOURCES

Retrieves the collection of Device Group IDs accessible via a share code.

| Detail | Specification |
|:-------|:--------------|
| **Method** | `GET` |
| **URI** | `/v3/shares/{sharecode}/resources` |
| **Security** | JWT Auth (mandatory) |
| **Success Response** | `200 OK` |

## Path Parameters

| Parameter | Type | Description |
|:----------|:-----|:------------|
| `sharecode` | String | The unique ID obtained from STREAM_SHARE |

## Success Response (200)

```json
{
  "resources": [
    "drone-system-b775cc6e",
    "dock-platform-d99f01a3"
  ],
  "page": {
    "size": 10,
    "totalElements": 2,
    "totalPages": 1,
    "number": 0
  },
  "_links": {
    "self": {
      "href": "/v3/shares/{sharecode}/resources"
    }
  }
}
```
