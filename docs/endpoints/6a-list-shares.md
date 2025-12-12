---
layout: default
title: Shares - List
parent: Endpoints
nav_order: 14
---

# List Shares

Retrieve all active shares for your organisation.

| Detail | Specification |
|:-------|:--------------|
| **Method** | `GET` |
| **URI** | `/v3/shares` |
| **Security** | JWT Bearer Token |
| **Success Response** | `200 OK` |

## Example Request

```bash
curl -X GET {{baseUrl}}/v3/shares \
  -H "Authorization: Bearer {{accessToken}}"
```

## Success Response (200)

```json
{
  "success": true,
  "shares": [
    {
      "shareCode": "abc123xyz",
      "shareUrl": "https://app.idi-fly.com/share/abc123xyz",
      "resourceId": "device-uuid-123",
      "shareType": "STREAM",
      "permissions": ["view"],
      "expiresAt": "2024-01-14T16:30:00Z",
      "maxViewers": 10,
      "createdAt": "2024-01-14T15:30:00Z"
    }
  ],
  "count": 1
}
```

| Field | Type | Description |
|:------|:-----|:------------|
| `success` | Boolean | Request success status |
| `shares` | Array | List of share objects |
| `count` | Integer | Total number of shares returned |
