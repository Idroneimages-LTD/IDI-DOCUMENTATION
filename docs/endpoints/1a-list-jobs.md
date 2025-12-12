---
layout: default
title: Jobs - List
parent: Endpoints
nav_order: 2
---

# List Jobs

Retrieve all jobs for your organisation.

| Detail | Specification |
|:-------|:--------------|
| **Method** | `GET` |
| **URI** | `/v3/jobs` |
| **Security** | JWT Bearer Token |
| **Success Response** | `200 OK` |

## Query Parameters

| Parameter | Type | Required | Description |
|:----------|:-----|:---------|:------------|
| `status` | String | No | Filter by status: `PENDING`, `QUEUED`, `IN_PROGRESS`, `COMPLETED`, `FAILED`, `CANCELLED` |
| `limit` | Integer | No | Maximum number of results (default: 20) |

## Example Request

```bash
curl -X GET "{{baseUrl}}/v3/jobs?status=PENDING&limit=10" \
  -H "Authorization: Bearer {{accessToken}}"
```

## Success Response (200)

```json
{
  "success": true,
  "jobs": [
    {
      "jobId": "job-uuid-abc",
      "name": "Inspection Mission Alpha",
      "status": "PENDING",
      "priority": 5,
      "createdAt": "2024-01-14T15:30:00Z"
    }
  ],
  "count": 1
}
```

| Field | Type | Description |
|:------|:-----|:------------|
| `success` | Boolean | Request success status |
| `jobs` | Array | List of job objects |
| `count` | Integer | Total number of jobs returned |
