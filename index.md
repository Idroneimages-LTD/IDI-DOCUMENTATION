---
layout: default
title: Home
nav_order: 1
---

# IDI Integration API Documentation

**Version:** v3 | **Updated:** 2025-11-27 | **Status:** Production

IDI-FLY-V3 Integration API for mission control, resource management, and real-time telemetry using HTTPS, MQTTS, and WSS.

## Quick Start

| Environment | REST | MQTTS | WSS |
|:------------|:-----|:------|:----|
| Production | `https://api.idi-fly.com/v3` | `mqtts://mqtt.idi-fly.com:8883` | `wss://ws.idi-fly.com/v3` |
| Staging | `https://api-staging.idi-fly.com/v3` | `mqtts://mqtt-staging.idi-fly.com:8883` | `wss://ws-staging.idi-fly.com/v3` |

## Authentication

**REST:** `Authorization: Bearer <token>`

**MQTTS:** Username: `<client_id>` / Password: `<jwt_token>`

**WSS:** `wss://ws.idi-fly.com/v3/jobs/{jobID}/status/ws?token=<jwt_token>`

## Endpoints Overview

| # | Name | Method | Path/Topic | Protocol |
|:--|:-----|:-------|:-----------|:---------|
| 1 | CreateJob | POST | `/v3/jobs/create` | HTTPS |
| 2 | Track Job | SUB | `{jobID}/JOB/STATUS` | MQTTS |
| 3 | Track Job | CONNECT | `/v3/jobs/{jobID}/status/ws` | WSS |
| 4 | Track Resource | SUB | `{DeviceGroupID}/DEVICE/STATUS` | MQTTS |
| 5 | Track Resource | CONNECT | `/v3/devices/{DeviceGroupID}/status/ws` | WSS |
| 6 | Stream Share | POST | `/v3/streams/share` | HTTPS |
| 7 | Track Share | SUB | `{shareID}/SHARE/STATUS` | MQTTS |
| 8 | Track Share | CONNECT | `/v3/shares/{shareID}/status/ws` | WSS |
| 9 | Get Resources | GET | `/v3/shares/{sharecode}/resources` | HTTPS |
