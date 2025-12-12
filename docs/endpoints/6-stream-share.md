---
layout: default
title: Streams - Create Share
parent: Endpoints
nav_order: 13
---

# Create Stream Share

Create a shareable link for a device stream or telemetry.

| Detail | Specification |
|:-------|:--------------|
| **Method** | `POST` |
| **URI** | `/v3/streams/share` |
| **Security** | JWT Bearer Token |
| **Success Response** | `201 Created` |

## Request Body

| Field | Type | Required | Description |
|:------|:-----|:---------|:------------|
| `resourceId` | String | Yes | The device ID to share |
| `shareType` | Enum | Yes | `STREAM` or `DEVICE_STATUS` |
| `permissions` | Array | Yes | Array of permissions, e.g., `["view"]` |
| `expiresIn` | Integer | Yes | Duration in seconds |
| `maxViewers` | Integer | No | Maximum concurrent viewers |
| `metadata` | Object | No | Additional metadata |

## Example Request

```bash
curl -X POST {{baseUrl}}/v3/streams/share \
  -H "Authorization: Bearer {{accessToken}}" \
  -H "Content-Type: application/json" \
  -d '{
    "resourceId": "device-uuid-123",
    "shareType": "STREAM",
    "permissions": ["view"],
    "expiresIn": 3600,
    "maxViewers": 10,
    "metadata": {
      "purpose": "Client demo"
    }
  }'
```

## Success Response (201)

```json
{
  "success": true,
  "share": {
    "shareCode": "abc123xyz",
    "shareUrl": "https://app.idi-fly.com/share/abc123xyz",
    "expiresAt": "2024-01-14T16:30:00Z",
    "maxViewers": 10
  }
}
```

## Share Types

| Type | Description |
|:-----|:------------|
| `STREAM` | Live video stream share |
| `DEVICE_STATUS` | Device telemetry/status share |
