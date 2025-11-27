---
layout: default
title: 7. Track Share (MQTTS)
parent: Endpoints
nav_order: 7
---

# Track Share Status (MQTTS)

Real-time status updates for a shared stream session.

| Detail | Specification |
|:-------|:--------------|
| **Protocol** | MQTTS |
| **Topic** | `{shareID}/SHARE/STATUS` |
| **Authentication** | `shareID` as temporary session credential |
| **Frequency** | 1 Hz |

**Note:** Payload content is dynamically filtered according to the role defined when creating the share.
