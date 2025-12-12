---
layout: default
title: Shares - Revoke
parent: Endpoints
nav_order: 16
---

# Revoke Share

Revoke (delete) an active share.

| Detail | Specification |
|:-------|:--------------|
| **Method** | `DELETE` |
| **URI** | `/v3/shares/{shareCode}` |
| **Security** | JWT Bearer Token |
| **Success Response** | `200 OK` |

## Path Parameters

| Parameter | Type | Required | Description |
|:----------|:-----|:---------|:------------|
| `shareCode` | String | Yes | The share code |

## Example Request

```bash
curl -X DELETE {{baseUrl}}/v3/shares/{{shareCode}} \
  -H "Authorization: Bearer {{accessToken}}"
```

## Success Response (200)

```json
{
  "success": true,
  "message": "Share revoked successfully"
}
```

## Error Responses

| Status | Error | Description |
|:-------|:------|:------------|
| 404 | `Share not found` | The specified share code does not exist |

## Notes

- Revoking a share immediately terminates all active connections using that share
- External viewers will be disconnected from the stream
