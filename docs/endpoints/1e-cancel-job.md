---
layout: default
title: Jobs - Cancel
parent: Endpoints
nav_order: 6
---

# Cancel Job

Cancel a pending or queued job.

| Detail | Specification |
|:-------|:--------------|
| **Method** | `POST` |
| **URI** | `/v3/jobs/{jobId}/cancel` |
| **Security** | JWT Bearer Token |
| **Success Response** | `200 OK` |

## Path Parameters

| Parameter | Type | Required | Description |
|:----------|:-----|:---------|:------------|
| `jobId` | String | Yes | The job ID |

## Example Request

```bash
curl -X POST {{baseUrl}}/v3/jobs/{{jobId}}/cancel \
  -H "Authorization: Bearer {{accessToken}}"
```

## Success Response (200)

```json
{
  "success": true,
  "job": {
    "jobId": "job-uuid-abc",
    "status": "CANCELLED",
    "cancelledAt": "2024-01-14T16:00:00Z"
  }
}
```

## Error Responses

| Status | Error | Description |
|:-------|:------|:------------|
| 404 | `Job not found` | The specified job ID does not exist |
| 400 | `Cannot cancel job` | Job is not in a cancellable state |

## Notes

- Only jobs with status `PENDING` or `QUEUED` can be cancelled
- Jobs that are `IN_PROGRESS`, `COMPLETED`, `FAILED`, or already `CANCELLED` cannot be cancelled
