---
layout: default
title: Sequence Diagrams
nav_order: 2
mermaid: true
---

# Sequence Diagrams

Visual representations of the IDI Integration API interaction flows.

## Mission Flow

```mermaid
sequenceDiagram
    autonumber
    participant Client
    participant REST API
    participant MQTT Broker
    participant WSS Server
    participant Drone System
    participant Dock

    rect rgb(240, 248, 255)
        Note over Client,REST API: Phase 1: Mission Creation
        Client->>REST API: POST /v3/jobs/create<br/>[JWT + Idempotency-Key]
        REST API->>REST API: Validate JWT & Idempotency
        REST API->>Drone System: Queue Mission Command
        REST API-->>Client: 201 Created {jobID}
    end

    rect rgb(255, 248, 240)
        Note over Client,WSS Server: Phase 2: Subscribe to Updates (MQTTS or WSS)
        alt MQTTS Connection
            Client->>MQTT Broker: CONNECT [JWT as password]
            MQTT Broker-->>Client: CONNACK
            Client->>MQTT Broker: SUBSCRIBE {jobID}/JOB/STATUS
            MQTT Broker-->>Client: SUBACK
        else WSS Connection
            Client->>WSS Server: WSS /v3/jobs/{jobID}/status/ws<br/>[JWT token]
            WSS Server-->>Client: 101 Switching Protocols
        end
    end

    rect rgb(240, 255, 240)
        Note over Drone System,Client: Phase 3: Mission Execution
        Dock->>Dock: Open hatch
        Drone System->>MQTT Broker: Publish: PENDING → INPROGRESS
        MQTT Broker->>WSS Server: Forward status
        par Notify via MQTTS
            MQTT Broker-->>Client: Job Status Update
        and Notify via WSS
            WSS Server-->>Client: Job Status Update
        end

        loop Every 1 second (1 Hz)
            Drone System->>MQTT Broker: Publish telemetry<br/>{navigation, droneState}
            MQTT Broker->>WSS Server: Forward telemetry
            par MQTTS Stream
                MQTT Broker-->>Client: Status Update
            and WSS Stream
                WSS Server-->>Client: Status Update
            end
        end

        Drone System->>MQTT Broker: Publish: INPROGRESS → COMPLETED
        MQTT Broker->>WSS Server: Forward completion
        par MQTTS Notification
            MQTT Broker-->>Client: Job Completed
        and WSS Notification
            WSS Server-->>Client: Job Completed
        end
    end
```

## Authentication Flow

```mermaid
sequenceDiagram
    autonumber
    participant Client
    participant Auth Server
    participant REST API
    participant MQTT Broker
    participant WSS Server

    rect rgb(255, 245, 238)
        Note over Client,Auth Server: JWT Acquisition
        Client->>Auth Server: POST /auth/token<br/>{client_id, client_secret}
        Auth Server->>Auth Server: Validate credentials
        Auth Server-->>Client: {access_token, expires_in, scope}
    end

    rect rgb(240, 248, 255)
        Note over Client,REST API: REST Authentication
        Client->>REST API: POST /v3/jobs/create<br/>Authorization: Bearer {token}
        REST API->>REST API: Validate JWT signature & claims
        REST API-->>Client: 201 Created
    end

    rect rgb(255, 248, 240)
        Note over Client,MQTT Broker: MQTTS Authentication
        Client->>MQTT Broker: CONNECT<br/>username: {client_id}<br/>password: {jwt_token}
        MQTT Broker->>MQTT Broker: Validate JWT
        MQTT Broker-->>Client: CONNACK (success)
    end

    rect rgb(240, 255, 240)
        Note over Client,WSS Server: WebSocket Authentication
        Client->>WSS Server: WSS Upgrade Request<br/>?token={jwt_token}
        WSS Server->>WSS Server: Validate JWT
        WSS Server-->>Client: 101 Switching Protocols
    end
```

## Stream Sharing Flow

```mermaid
sequenceDiagram
    autonumber
    participant Manager
    participant REST API
    participant External Viewer
    participant MQTT Broker
    participant Drone System

    rect rgb(240, 248, 255)
        Note over Manager,REST API: Create Share Link
        Manager->>REST API: POST /v3/streams/share<br/>{DeviceGroupID, role: VIEWER, timeout: 60}
        REST API->>REST API: Validate MANAGER role
        REST API->>REST API: Generate shareID & expiry
        REST API-->>Manager: 201 {shareID, shareLink, expiresAt}
    end

    Manager->>External Viewer: Share link via email/chat

    rect rgb(255, 248, 240)
        Note over External Viewer,MQTT Broker: External Access via Share
        External Viewer->>MQTT Broker: CONNECT<br/>credentials: {shareID}
        MQTT Broker->>MQTT Broker: Validate shareID & expiry
        MQTT Broker-->>External Viewer: CONNACK
        External Viewer->>MQTT Broker: SUBSCRIBE {shareID}/SHARE/STATUS
    end

    rect rgb(240, 255, 240)
        Note over Drone System,External Viewer: Filtered Telemetry Stream
        loop Every 1 second
            Drone System->>MQTT Broker: Full telemetry
            MQTT Broker->>MQTT Broker: Filter by VIEWER role
            MQTT Broker-->>External Viewer: Filtered status update
        end
    end

    rect rgb(255, 240, 240)
        Note over MQTT Broker,External Viewer: Share Expiration
        MQTT Broker->>MQTT Broker: shareID expired
        MQTT Broker-->>External Viewer: DISCONNECT
    end
```

## Resource Status Monitoring

```mermaid
sequenceDiagram
    autonumber
    participant Client A
    participant Client B
    participant MQTT Broker
    participant WSS Server
    participant Device Group

    par MQTTS Connection
        Client A->>MQTT Broker: CONNECT [JWT]
        MQTT Broker-->>Client A: CONNACK
        Client A->>MQTT Broker: SUBSCRIBE<br/>{DeviceGroupID}/DEVICE/STATUS
        MQTT Broker-->>Client A: SUBACK
    and WSS Connection
        Client B->>WSS Server: WSS /v3/devices/{DeviceGroupID}/status/ws
        WSS Server-->>Client B: 101 Upgrade
    end

    loop Every 1 second (1 Hz)
        Device Group->>Device Group: Collect telemetry

        par Broadcast to all subscribers
            Device Group->>MQTT Broker: Publish status
            MQTT Broker-->>Client A: {droneState, dockState,<br/>navigation, weather}
        and
            Device Group->>WSS Server: Push status
            WSS Server-->>Client B: {droneState, dockState,<br/>navigation, weather}
        end
    end
```

## Error Handling Flow

```mermaid
sequenceDiagram
    autonumber
    participant Client
    participant REST API
    participant MQTT Broker

    rect rgb(255, 240, 240)
        Note over Client,REST API: Authentication Error
        Client->>REST API: POST /v3/jobs/create<br/>[Invalid/Expired JWT]
        REST API-->>Client: 401 Unauthorized<br/>{error: "INVALID_TOKEN"}
    end

    rect rgb(255, 245, 238)
        Note over Client,REST API: Idempotency Conflict
        Client->>REST API: POST /v3/jobs/create<br/>[Same Idempotency-Key, Different Payload]
        REST API-->>Client: 409 Conflict<br/>{error: "IDEMPOTENCY_CONFLICT"}
    end

    rect rgb(255, 248, 240)
        Note over Client,REST API: Rate Limit Exceeded
        Client->>REST API: POST /v3/jobs/create (61st request)
        REST API-->>Client: 429 Too Many Requests<br/>X-RateLimit-Reset: {timestamp}
    end

    rect rgb(240, 240, 255)
        Note over Client,MQTT Broker: Connection Failure Recovery
        Client->>MQTT Broker: CONNECT [JWT]
        MQTT Broker-->>Client: CONNACK
        Client->>MQTT Broker: SUBSCRIBE {jobID}/JOB/STATUS

        Note over MQTT Broker: Connection lost
        MQTT Broker--xClient: Disconnect

        Client->>Client: Exponential backoff
        Client->>MQTT Broker: RECONNECT [JWT]
        MQTT Broker-->>Client: CONNACK
        Client->>MQTT Broker: RE-SUBSCRIBE
    end
```

## Drone State Transitions

```mermaid
stateDiagram-v2
    [*] --> DOCKED: Initial State

    DOCKED --> GPS_NORMAL: Dock Opens / Pre-flight Check
    GPS_NORMAL --> WAYPOINT: Mission Started
    GPS_NORMAL --> HOVER: Hover Command
    GPS_NORMAL --> ORBIT: Orbit Command

    WAYPOINT --> HOVER: Waypoint Reached
    WAYPOINT --> RETURNING: Mission Complete / Low Battery

    HOVER --> WAYPOINT: Continue Mission
    HOVER --> ORBIT: Switch to Orbit
    HOVER --> RETURNING: Return Command

    ORBIT --> HOVER: Exit Orbit
    ORBIT --> RETURNING: Return Command

    RETURNING --> LANDED: Approach Dock
    LANDED --> DOCKED: Dock Closes

    MANUAL --> GPS_NORMAL: Auto Mode Enabled
    GPS_NORMAL --> MANUAL: Manual Override

    note right of DOCKED: Charging/Cooling
    note right of WAYPOINT: Following Mission Path
    note right of RETURNING: RTH Active
```

## Job State Lifecycle

```mermaid
stateDiagram-v2
    [*] --> PENDING: CreateJob API Called

    PENDING --> INPROGRESS: Drone Launched
    PENDING --> CANCELLED: User Cancellation
    PENDING --> FAILED: Pre-flight Check Failed

    INPROGRESS --> COMPLETED: Mission Successful
    INPROGRESS --> FAILED: Error During Execution
    INPROGRESS --> CANCELLED: User Cancellation

    COMPLETED --> [*]
    FAILED --> [*]
    CANCELLED --> [*]

    note right of PENDING: Awaiting drone availability
    note right of INPROGRESS: Telemetry streaming at 1Hz
    note right of COMPLETED: Final status published
```

## System Architecture

```mermaid
flowchart TB
    subgraph Clients
        WebApp[Web Application]
        MobileApp[Mobile App]
        ThirdParty[Third-Party Integration]
    end

    subgraph "IDI-FLY-V3 API Gateway"
        REST[REST API<br/>HTTPS :443]
        WSS[WebSocket Server<br/>WSS :443]
        MQTT[MQTT Broker<br/>MQTTS :8883]
    end

    subgraph "Core Services"
        Auth[Auth Service<br/>JWT Validation]
        JobMgr[Job Manager]
        ShareMgr[Share Manager]
        Telemetry[Telemetry Processor]
    end

    subgraph "Field Devices"
        Drone1[Drone System 1]
        Dock1[Dock Platform 1]
        Drone2[Drone System 2]
        Dock2[Dock Platform 2]
    end

    WebApp --> REST
    WebApp --> WSS
    MobileApp --> REST
    MobileApp --> MQTT
    ThirdParty --> REST
    ThirdParty --> MQTT

    REST --> Auth
    WSS --> Auth
    MQTT --> Auth

    REST --> JobMgr
    REST --> ShareMgr

    JobMgr --> Telemetry
    ShareMgr --> Telemetry

    Telemetry --> WSS
    Telemetry --> MQTT

    Drone1 --> Telemetry
    Dock1 --> Telemetry
    Drone2 --> Telemetry
    Dock2 --> Telemetry
```
