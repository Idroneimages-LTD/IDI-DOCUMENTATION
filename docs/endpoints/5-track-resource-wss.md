---
layout: default
title: Track Resource (WSS)
parent: Endpoints
nav_order: 10
---

# Track Resource Status (WSS)

Continuous telemetry stream from a device group via WebSocket.

| Detail | Specification |
|:-------|:--------------|
| **Protocol** | WSS |
| **Base URL** | `{{wss_url}}` |
| **Security** | JWT Auth via query parameter |
| **Frequency** | 1 Hz |

## Connection URL

```
{{wss_url}}?token=<JWT_TOKEN>&resourceId=<RESOURCE_ID>
```

| Parameter | Description |
|:----------|:------------|
| `token` | Your JWT access token |
| `resourceId` | The device group ID to subscribe to |

## Connection Example

```javascript
const ws = new WebSocket(
  '{{wss_url}}?token=' + accessToken + '&resourceId=' + deviceGroupId
);

ws.onmessage = (event) => {
  const telemetry = JSON.parse(event.data);
  console.log(telemetry);
};
```

## Message Payload

Same as MQTTS Track Resource endpoint.
