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
| **Base URL** | `{{wss_url}}` |
| **Security** | JWT Auth via query parameter |
| **Frequency** | 1 Hz |

## Connection URL

```
{{wss_url}}?token=<JWT_TOKEN>&resourceId=<JOB_ID>
```

| Parameter | Description |
|:----------|:------------|
| `token` | Your JWT access token |
| `resourceId` | The job ID to subscribe to |

## Connection Example

```javascript
const ws = new WebSocket(
  '{{wss_url}}?token=' + accessToken + '&resourceId=' + jobId
);

ws.onmessage = (event) => {
  const status = JSON.parse(event.data);
  console.log(status);
};
```

## Message Payload

Same as MQTTS Track Job endpoint.
