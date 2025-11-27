---
layout: default
title: 8. Track Share (WSS)
parent: Endpoints
nav_order: 8
---

# Track Share Status (WSS)

Real-time status updates for a shared stream session via WebSocket.

| Detail | Specification |
|:-------|:--------------|
| **Protocol** | WSS |
| **URI** | `/v3/shares/{shareID}/status/ws` |
| **Authentication** | `shareID` as temporary session credential |
| **Frequency** | 1 Hz |
