---
layout: default
title: Error Handling
nav_order: 8
---

# Error Handling

## Standard Error Response

```json
{
  "error": "ErrorCode",
  "message": "Human-readable error description",
  "code": 400,
  "details": {},
  "requestId": "req-abc123",
  "timestamp": "2025-11-27T10:30:00Z"
}
```

## HTTP Status Codes

| Code | Meaning | Description |
|:-----|:--------|:------------|
| `200` | OK | Request succeeded |
| `201` | Created | Resource created successfully |
| `400` | Bad Request | Invalid request payload |
| `401` | Unauthorized | Missing or invalid JWT |
| `403` | Forbidden | Insufficient permissions |
| `404` | Not Found | Resource not found |
| `409` | Conflict | Duplicate idempotency key with different payload |
| `415` | Unsupported Media Type | Invalid API version requested |
| `422` | Unprocessable Entity | Validation error |
| `429` | Too Many Requests | Rate limit exceeded |
| `500` | Internal Server Error | Server error |
| `503` | Service Unavailable | Service temporarily unavailable |

## Common Error Codes

| Error Code | HTTP Status | Description |
|:-----------|:------------|:------------|
| `INVALID_TOKEN` | 401 | JWT is invalid or expired |
| `INSUFFICIENT_PERMISSIONS` | 403 | User lacks required role |
| `DEVICE_NOT_FOUND` | 404 | DeviceGroupID does not exist |
| `JOB_NOT_FOUND` | 404 | JobID does not exist |
| `MISSION_NOT_FOUND` | 404 | MissionID does not exist |
| `IDEMPOTENCY_CONFLICT` | 409 | Same key used with different payload |
| `VALIDATION_ERROR` | 422 | Request validation failed |
| `DEVICE_OFFLINE` | 503 | Device is not currently online |
