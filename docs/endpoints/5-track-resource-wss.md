---
layout: default
title: 5. Track Resource (WSS)
parent: Endpoints
nav_order: 5
---

# Track Resource Status (WSS)

Continuous telemetry stream from a device group via WebSocket.

| Detail | Specification |
|:-------|:--------------|
| **Protocol** | WSS |
| **URI** | `/v3/devices/{DeviceGroupID}/status/ws` |
| **Security** | JWT Auth / TLS |
| **Frequency** | 1 Hz |

## Message Payload

Same as MQTTS Track Resource endpoint.
