---
layout: default
title: Jobs - Create
parent: Endpoints
nav_order: 1
---

# Create Job

Create a new job (drone mission request).

| Detail | Specification |
|:-------|:--------------|
| **Method** | `POST` |
| **URI** | `/v3/jobs/create` |
| **Content-Type** | `application/json` |
| **Security** | JWT Bearer Token |
| **Success Response** | `201 Created` |

## Request Body

| Field | Type | Required | Description |
|:------|:-----|:---------|:------------|
| `name` | String | Yes | Job name |
| `description` | String | No | Job description |
| `missionId` | String | No | Mission ID to execute |
| `deviceId` | String | No | Target device ID |
| `deviceGroupId` | String | Yes | Target device group ID |
| `priority` | Integer | No | Priority level (1-10) |
| `scheduledAt` | String | No | ISO 8601 scheduled time |
| `metadata` | Object | No | Additional metadata |

## Example Request

```bash
curl -X POST {{baseUrl}}/v3/jobs/create \
  -H "Authorization: Bearer {{accessToken}}" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "Inspection Mission Alpha",
    "description": "Routine infrastructure inspection",
    "missionId": "mission-uuid-123",
    "deviceId": "drone-uuid-456",
    "deviceGroupId": "group-uuid-789",
    "priority": 5,
    "scheduledAt": "2024-01-15T10:00:00Z",
    "metadata": {
      "customer": "ACME Corp"
    }
  }'
```

## Success Response (201)

```json
{
  "success": true,
  "job": {
    "jobId": "job-uuid-abc",
    "name": "Inspection Mission Alpha",
    "status": "PENDING",
    "priority": 5,
    "createdAt": "2024-01-14T15:30:00Z"
  }
}
```

## Job Status Values

| Status | Description |
|:-------|:------------|
| `PENDING` | Job created, awaiting processing |
| `QUEUED` | Job queued for execution |
| `IN_PROGRESS` | Job currently executing |
| `COMPLETED` | Job completed successfully |
| `FAILED` | Job failed |
| `CANCELLED` | Job was cancelled |

## Error Responses

| Status | Error | Description |
|:-------|:------|:------------|
| 400 | `invalid_request` | Missing or invalid parameters |
| 404 | `Device group not found` | The specified device group does not exist |
