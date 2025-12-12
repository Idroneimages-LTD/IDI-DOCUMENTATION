---
layout: default
title: Rate Limiting
nav_order: 9
---

# Rate Limiting

## REST Endpoints

| Endpoint | Rate Limit | Burst |
|:---------|:-----------|:------|
| `POST /v3/auth/token` | 50/sec | 100 |
| `POST /v3/jobs/create` | 60/min | - |
| `POST /v3/streams/share` | 30/min | - |
| `GET /v3/shares/{sharecode}/resources` | 120/min | - |
| All other endpoints | No limit | - |

## Rate Limit Headers

```http
X-RateLimit-Limit: 60
X-RateLimit-Remaining: 45
X-RateLimit-Reset: 1732700400
```

## Streaming Connections

| Protocol | Limit |
|:---------|:------|
| MQTTS | 100 concurrent subscriptions per client |
| WSS | 50 concurrent connections per client |
