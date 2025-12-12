---
layout: default
title: Shares - Get by Code
parent: Endpoints
nav_order: 15
---

# Get Share by Code

Retrieve details of a specific share.

| Detail | Specification |
|:-------|:--------------|
| **Method** | `GET` |
| **URI** | `/v3/shares/{shareCode}` |
| **Security** | JWT Bearer Token |
| **Success Response** | `200 OK` |

## Path Parameters

| Parameter | Type | Required | Description |
|:----------|:-----|:---------|:------------|
| `shareCode` | String | Yes | The share code |

## Example Request

```bash
curl -X GET {{baseUrl}}/v3/shares/{{shareCode}} \
  -H "Authorization: Bearer {{accessToken}}"
```

## Success Response (200)

```json
{
  "success": true,
  "share": {
    "shareCode": "abc123xyz",
    "shareUrl": "https://app.idi-fly.com/share/abc123xyz",
    "resourceId": "device-uuid-123",
    "shareType": "STREAM",
    "permissions": ["view"],
    "expiresAt": "2024-01-14T16:30:00Z",
    "maxViewers": 10,
    "createdAt": "2024-01-14T15:30:00Z"
  }
}
```

## Error Responses

| Status | Error | Description |
|:-------|:------|:------------|
| 404 | `Share not found` | The specified share code does not exist or has expired |
