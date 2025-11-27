---
layout: default
title: Endpoints
nav_order: 6
has_children: true
---

# Endpoint Specification

The IDI-FLY-V3 API provides nine endpoints for mission control and telemetry.

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
