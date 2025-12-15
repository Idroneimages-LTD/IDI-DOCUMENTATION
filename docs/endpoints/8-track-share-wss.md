---
layout: default
title: Track Share (WSS)
parent: Endpoints
nav_order: 12
---

# Track Share Status (WSS)

Real-time status updates for a shared stream session via WebSocket.

| Detail | Specification |
|:-------|:--------------|
| **Protocol** | WSS |
| **Base URL** | `{{wss_url}}` |
| **Security** | Share ID via query parameter |
| **Frequency** | 1 Hz |

## Connection URL

```
{{wss_url}}?token=<JWT_TOKEN>&resourceId=<SHARE_ID>
```

| Parameter | Description |
|:----------|:------------|
| `token` | Your JWT access token |
| `resourceId` | The share ID to subscribe to |

## Connection Example

```javascript
const ws = new WebSocket(
  '{{wss_url}}?token=' + accessToken + '&resourceId=' + shareId
);

ws.onmessage = (event) => {
  const status = JSON.parse(event.data);
  console.log(status);
};
```

## Message Payload

Payload content is dynamically filtered according to the role defined when creating the share.
