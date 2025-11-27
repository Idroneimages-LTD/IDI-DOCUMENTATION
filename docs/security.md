---
layout: default
title: Security
nav_order: 4
---

# Security and Access Control

All API interactions must adhere to stringent security protocols.

## JWT Authentication

Authentication is mandatory for all REST endpoints and real-time connections.

### REST (HTTPS)

```http
Authorization: Bearer <token>
```

### MQTTS

```
Username: <client_id>
Password: <jwt_token>
```

### WSS

```
wss://ws.idi-fly.com/v3/jobs/{jobID}/status/ws?token=<jwt_token>
```

## Idempotency

| Requirement | Description |
|:------------|:------------|
| Key Format | UUID v4 (e.g., `550e8400-e29b-41d4-a716-446655440000`) |
| Header | `Idempotency-Key: <uuid>` |
| Scope | Keys are uniquely scoped by API version and endpoint |
| TTL | Idempotency keys expire after 24 hours |

**Example:**

```http
POST /v3/jobs/create HTTP/1.1
Host: api.idi-fly.com
Authorization: Bearer <token>
Idempotency-Key: 550e8400-e29b-41d4-a716-446655440000
Content-Type: application/json
```

## Role-Based Access Control

| Role | Description | Permissions |
|:-----|:------------|:------------|
| `VIEWER` | Read-only access | View telemetry, View status |
| `PILOT` | Operational control | All VIEWER + Control missions |
| `MANAGER` | Full administrative | All PILOT + Share streams, Manage resources |
