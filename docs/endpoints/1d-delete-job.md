---
layout: default
title: Jobs - Delete
parent: Endpoints
nav_order: 5
---

# Delete Job

Delete a job.

| Detail | Specification |
|:-------|:--------------|
| **Method** | `DELETE` |
| **URI** | `/v3/jobs/{jobId}` |
| **Security** | JWT Bearer Token |
| **Success Response** | `200 OK` |

## Path Parameters

| Parameter | Type | Required | Description |
|:----------|:-----|:---------|:------------|
| `jobId` | String | Yes | The job ID |

## Example Request

```bash
curl -X DELETE {{baseUrl}}/v3/jobs/{{jobId}} \
  -H "Authorization: Bearer {{accessToken}}"
```

## Success Response (200)

```json
{
  "success": true,
  "message": "Job deleted successfully"
}
```

## Error Responses

| Status | Error | Description |
|:-------|:------|:------------|
| 404 | `Job not found` | The specified job ID does not exist |
