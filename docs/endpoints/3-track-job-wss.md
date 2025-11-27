---
layout: default
title: 3. Track Job (WSS)
parent: Endpoints
nav_order: 3
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
  'wss://ws.idi-fly.com/v3/jobs/job-f47ac10b-58cc-4372-a567-0e02b2c3d479/status/ws',
  [],
  { headers: { 'Authorization': 'Bearer <token>' } }
);
```

## Message Payload

Same as MQTTS Track Job endpoint.
