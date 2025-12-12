---
layout: default
title: Jobs - Update
parent: Endpoints
nav_order: 4
---

# Update Job

Update an existing job's attributes.

| Detail | Specification |
|:-------|:--------------|
| **Method** | `PATCH` |
| **URI** | `/v3/jobs/{jobId}` |
| **Security** | JWT Bearer Token |
| **Success Response** | `200 OK` |

## Path Parameters

| Parameter | Type | Required | Description |
|:----------|:-----|:---------|:------------|
| `jobId` | String | Yes | The job ID |

## Request Body

| Field | Type | Required | Description |
|:------|:-----|:---------|:------------|
| `name` | String | No | Updated job name |
| `description` | String | No | Updated description |
| `priority` | Integer | No | Updated priority (1-10) |

## Example Request

```bash
curl -X PATCH {{baseUrl}}/v3/jobs/{{jobId}} \
  -H "Authorization: Bearer {{accessToken}}" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "Updated Mission Name",
    "description": "Updated description",
    "priority": 8
  }'
```

## Success Response (200)

```json
{
  "success": true,
  "job": {
    "jobId": "job-uuid-abc",
    "name": "Updated Mission Name",
    "description": "Updated description",
    "status": "PENDING",
    "priority": 8,
    "updatedAt": "2024-01-14T16:00:00Z"
  }
}
```

## Error Responses

| Status | Error | Description |
|:-------|:------|:------------|
| 404 | `Job not found` | The specified job ID does not exist |
| 400 | `invalid_request` | Invalid update parameters |
