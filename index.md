---
layout: default
title: Home
nav_order: 1
---

# IDI Integration API Documentation

**Version:** v3 | **Updated:** 2025-12-12 | **Status:** Production

IDI-FLY-V3 Integration API for mission control, resource management, and real-time telemetry using HTTPS, MQTTS, and WSS.

## Quick Start

### Production URLs

| Type | URL |
|:-----|:----|
| **REST API** | `https://9v3tf7g6dj.execute-api.eu-west-2.amazonaws.com/prod` |
| **WebSocket** | `wss://qt00j5r1s7.execute-api.eu-west-2.amazonaws.com/prod` |

### WebSocket Connection

Connect to WebSocket with token and resource ID as query parameters:

```
{{wsUrl}}/prod?token=<JWT_TOKEN>&resourceId=<RESOURCE_ID>
```

| Parameter | Description |
|:----------|:------------|
| `token` | Your JWT access token |
| `resourceId` | The device or job ID to subscribe to |

## Authentication

This API uses **OAuth 2.0 M2M** (`client_credentials` grant) authentication.

### Get Access Token

```bash
curl -X POST {{baseUrl}}/v3/auth/token \
  -H "x-api-key: {{apiKey}}" \
  -H "Content-Type: application/json" \
  -d '{
    "grant_type": "client_credentials",
    "client_id": "{{clientId}}",
    "client_secret": "{{clientSecret}}"
  }'
```

### Use Access Token

| Protocol | Authentication Method |
|:---------|:---------------------|
| **REST** | `Authorization: Bearer {{accessToken}}` |
| **MQTTS** | Username: `{{clientId}}` / Password: `{{accessToken}}` |
| **WSS** | `{{wsUrl}}/v3/...?token={{accessToken}}` |

> Token expires in **1 hour**. Request a new token when expired.

## REST API Endpoints

### Authentication

| Method | Endpoint | Description |
|:-------|:---------|:------------|
| POST | `/v3/auth/token` | Get access token (requires x-api-key header) |

### Jobs API

| Method | Endpoint | Description |
|:-------|:---------|:------------|
| POST | `/v3/jobs/create` | Create a new job (drone mission request) |
| GET | `/v3/jobs` | List all jobs |
| GET | `/v3/jobs/{jobId}` | Get job details |
| PATCH | `/v3/jobs/{jobId}` | Update a job |
| DELETE | `/v3/jobs/{jobId}` | Delete a job |
| POST | `/v3/jobs/{jobId}/cancel` | Cancel a pending job |

### Streams API

| Method | Endpoint | Description |
|:-------|:---------|:------------|
| POST | `/v3/streams/share` | Create a shareable stream link |

### Shares API

| Method | Endpoint | Description |
|:-------|:---------|:------------|
| GET | `/v3/shares` | List all active shares |
| GET | `/v3/shares/{shareCode}` | Get share details |
| DELETE | `/v3/shares/{shareCode}` | Revoke a share |

### Resources API

| Method | Endpoint | Description |
|:-------|:---------|:------------|
| GET | `/v3/resources` | Get available device groups |

## Real-Time Tracking Endpoints

| # | Name | Method | Path/Topic | Protocol |
|:--|:-----|:-------|:-----------|:---------|
| 1 | Track Job | SUB | `{jobID}/JOB/STATUS` | MQTTS |
| 2 | Track Job | CONNECT | `/v3/jobs/{jobID}/status/ws` | WSS |
| 3 | Track Resource | SUB | `{DeviceGroupID}/DEVICE/STATUS` | MQTTS |
| 4 | Track Resource | CONNECT | `/v3/devices/{DeviceGroupID}/status/ws` | WSS |
| 5 | Track Share | SUB | `{shareID}/SHARE/STATUS` | MQTTS |
| 6 | Track Share | CONNECT | `/v3/shares/{shareID}/status/ws` | WSS |
