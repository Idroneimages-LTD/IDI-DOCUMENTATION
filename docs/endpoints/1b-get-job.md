---
layout: default
title: Jobs - Get by ID
parent: Endpoints
nav_order: 3
---

# Get Job by ID

Retrieve details of a specific job.

| Detail | Specification |
|:-------|:--------------|
| **Method** | `GET` |
| **URI** | `/v3/jobs/{jobId}` |
| **Security** | JWT Bearer Token |
| **Success Response** | `200 OK` |

## Path Parameters

| Parameter | Type | Required | Description |
|:----------|:-----|:---------|:------------|
| `jobId` | String | Yes | The job ID |

## Example Request

```bash
curl -X GET {{baseUrl}}/v3/jobs/{{jobId}} \
  -H "Authorization: Bearer {{accessToken}}"
```

## Success Response (200)

```json
{
  "success": true,
  "job": {
    "jobId": "job-uuid-abc",
    "name": "Inspection Mission Alpha",
    "description": "Routine infrastructure inspection",
    "missionId": "mission-uuid-123",
    "deviceId": "drone-uuid-456",
    "deviceGroupId": "group-uuid-789",
    "status": "PENDING",
    "priority": 5,
    "createdAt": "2024-01-14T15:30:00Z",
    "metadata": {
      "customer": "ACME Corp"
    }
  }
}
```

## Error Responses

| Status | Error | Description |
|:-------|:------|:------------|
| 404 | `Job not found` | The specified job ID does not exist |
