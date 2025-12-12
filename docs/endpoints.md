---
layout: default
title: Endpoints
nav_order: 6
has_children: true
---

# Endpoint Specification

The IDI Integration API provides REST endpoints for mission control, resource management, and real-time telemetry.

## REST API Endpoints

### Authentication

| Method | Endpoint | Description |
|:-------|:---------|:------------|
| POST | `/v3/auth/token` | Get access token |

### Jobs API

| Method | Endpoint | Description |
|:-------|:---------|:------------|
| POST | `/v3/jobs/create` | Create a new job |
| GET | `/v3/jobs` | List all jobs |
| GET | `/v3/jobs/{jobId}` | Get job details |
| PATCH | `/v3/jobs/{jobId}` | Update a job |
| DELETE | `/v3/jobs/{jobId}` | Delete a job |
| POST | `/v3/jobs/{jobId}/cancel` | Cancel a job |

### Streams API

| Method | Endpoint | Description |
|:-------|:---------|:------------|
| POST | `/v3/streams/share` | Create stream share |

### Shares API

| Method | Endpoint | Description |
|:-------|:---------|:------------|
| GET | `/v3/shares` | List all shares |
| GET | `/v3/shares/{shareCode}` | Get share details |
| DELETE | `/v3/shares/{shareCode}` | Revoke a share |

### Resources API

| Method | Endpoint | Description |
|:-------|:---------|:------------|
| GET | `/v3/resources` | Get device groups |

## Real-Time Tracking Endpoints

| # | Name | Method | Path/Topic | Protocol |
|:--|:-----|:-------|:-----------|:---------|
| 1 | Track Job | SUB | `{jobID}/JOB/STATUS` | MQTTS |
| 2 | Track Job | CONNECT | `/v3/jobs/{jobID}/status/ws` | WSS |
| 3 | Track Resource | SUB | `{DeviceGroupID}/DEVICE/STATUS` | MQTTS |
| 4 | Track Resource | CONNECT | `/v3/devices/{DeviceGroupID}/status/ws` | WSS |
| 5 | Track Share | SUB | `{shareID}/SHARE/STATUS` | MQTTS |
| 6 | Track Share | CONNECT | `/v3/shares/{shareID}/status/ws` | WSS |
