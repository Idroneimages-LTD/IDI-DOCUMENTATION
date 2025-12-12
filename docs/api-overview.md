---
layout: default
title: API Overview
nav_order: 3
---

# API Overview and Integration Standards

The IDI Integration API implements a hybrid approach common in B2B SaaS integrations, distinguishing between asynchronous real-time event distribution and synchronous management commands.

## Architecture

```mermaid
flowchart TB
    subgraph Partners["Integration Partners"]
        P1[Partner Server]
    end

    subgraph Gateway["API Gateway"]
        REST[REST API]
        WS[WebSocket API]
        AUTH[Token Auth<br/>x-api-key]
    end

    subgraph Lambda["Lambda Functions"]
        JOBS[Jobs]
        STREAMS[Streams]
        SHARES[Shares]
        AUTHFN[Auth]
    end

    subgraph Storage["DynamoDB Tables"]
        JOBST[Jobs Table]
        SHAREST[Shares Table]
        CONNT[Connections Table]
    end

    P1 --> REST
    P1 --> WS
    P1 --> AUTH

    REST --> JOBS
    REST --> STREAMS
    REST --> SHARES
    WS --> JOBS
    AUTH --> AUTHFN

    JOBS --> JOBST
    STREAMS --> SHAREST
    SHARES --> SHAREST
    WS --> CONNT
```

## Protocol Overview

| API Function | Protocol | Standard Model | Rationale |
|:-------------|:---------|:---------------|:----------|
| Mission Request (CreateJob) | HTTPS (REST) | Synchronous Command Creation | Actuator Command; creates an asynchronous task |
| Track Job / Resource / Share | MQTTS & WSS | Asynchronous Data Streaming (1 Hz) | Secure transport for dynamic data and status |
| STREAM_SHARE / GET RESOURCES | HTTPS (REST) | Synchronous Access/Management | Resource discovery and credential management |

## Base URLs

| Environment | REST Base URL | MQTTS Broker | WSS Base URL |
|:------------|:--------------|:-------------|:-------------|
| Your Environment | `{{baseUrl}}` | `{{mqttBroker}}` | `{{wsUrl}}` |

> **Note:** Environment-specific URLs will be provided during partner onboarding.
