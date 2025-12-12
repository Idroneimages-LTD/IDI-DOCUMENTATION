---
layout: default
title: Track Job (WSS)
parent: Endpoints
nav_order: 8
---

# Track Job Status (WSS)

Real-time job status updates via WebSocket Secure.

| Detail | Specification |
|:-------|:--------------|
| **Protocol** | WSS |
| **URI** | `/v3/jobs/{jobID}/status/ws` |
| **Security** | JWT Auth / TLS |
| **Frequency** | 1 Hz |

## Connection Example

```javascript
const ws = new WebSocket(
  '{{wsUrl}}/v3/jobs/{jobId}/status/ws?token={{accessToken}}'
);
```

## Message Payload

Same as MQTTS Track Job endpoint.
