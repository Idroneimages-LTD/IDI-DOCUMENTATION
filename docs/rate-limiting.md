---
layout: default
title: Rate Limiting
nav_order: 9
---

# Rate Limiting

## REST Endpoints

| Endpoint | Rate Limit | Window |
|:---------|:-----------|:-------|
| `POST /v3/jobs/create` | 60 requests | per minute |
| `POST /v3/streams/share` | 30 requests | per minute |
| `GET /v3/shares/{sharecode}/resources` | 120 requests | per minute |

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
