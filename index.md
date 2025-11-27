# IDI-FLY-V3 API Documentation

**Version:** v3 | **Status:** Production | **Updated:** 2025-11-27

Hybrid API for mission control, resource management, and real-time telemetry using HTTPS, MQTTS, and WSS.

---

## Sequence Diagrams

### Mission Flow

```mermaid
sequenceDiagram
    autonumber
    participant Client
    participant REST API
    participant MQTT Broker
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
        Note over Client,MQTT Broker: Phase 2: Subscribe to Updates
        Client->>MQTT Broker: CONNECT [JWT as password]
        MQTT Broker-->>Client: CONNACK
        Client->>MQTT Broker: SUBSCRIBE {jobID}/JOB/STATUS
        MQTT Broker-->>Client: SUBACK
    end

    rect rgb(240, 255, 240)
        Note over Drone System,Client: Phase 3: Mission Execution
        Dock->>Dock: Open hatch
        Drone System->>MQTT Broker: Publish: PENDING → INPROGRESS
        MQTT Broker-->>Client: Job Status Update

        loop Every 1 second (1 Hz)
            Drone System->>MQTT Broker: Publish telemetry<br/>{navigation, droneState}
            MQTT Broker-->>Client: Status Update
        end

        Drone System->>MQTT Broker: Publish: INPROGRESS → COMPLETED
        MQTT Broker-->>Client: Job Completed
    end
```

### Authentication Flow

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

### Stream Sharing Flow

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
```

### Drone State Transitions

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
```

### Job State Lifecycle

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
```

### System Architecture

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

---

## Base URLs

| Environment | REST | MQTTS | WSS |
|:------------|:-----|:------|:----|
| Production | `https://api.idi-fly.com/v3` | `mqtts://mqtt.idi-fly.com:8883` | `wss://ws.idi-fly.com/v3` |
| Staging | `https://api-staging.idi-fly.com/v3` | `mqtts://mqtt-staging.idi-fly.com:8883` | `wss://ws-staging.idi-fly.com/v3` |

---

## Authentication

**REST:** `Authorization: Bearer <token>`

**MQTTS:** Username: `<client_id>` / Password: `<jwt_token>`

**WSS:** `wss://ws.idi-fly.com/v3/jobs/{jobID}/status/ws?token=<jwt_token>`

---

## Endpoints

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

---

### 1. CreateJob

`POST /v3/jobs/create`

**Headers:** `Authorization: Bearer <token>`, `Idempotency-Key: <uuid>`

```json
{
  "DeviceGroupID": "b775cc6e-1234-5678-90ab-cdef12345678",
  "MissionID": "a1b2c3d4-5678-90ab-cdef-123456789012",
  "ActionOnArrival": "MISSION"
}
```

**Response (201):**
```json
{
  "jobID": "job-f47ac10b-58cc-4372-a567-0e02b2c3d479",
  "status": "PENDING",
  "createdAt": "2025-11-27T10:30:00Z"
}
```

---

### 2-3. Track Job Status

**MQTTS Topic:** `{jobID}/JOB/STATUS` | **WSS:** `/v3/jobs/{jobID}/status/ws`

```json
{
  "jobState": "INPROGRESS",
  "droneState": "WAYPOINT",
  "dockState": "OPEN",
  "navigation": {
    "position": { "type": "Point", "coordinates": [-122.4194, 37.7749, 85.5] },
    "orientation": { "heading": 270.5, "pitch": -2.1, "roll": 0.3 },
    "velocity": { "groundSpeed": 12.5, "verticalSpeed": -0.5 }
  },
  "timestamp": "2025-11-27T10:30:15.123Z"
}
```

---

### 4-5. Track Resource Status

**MQTTS Topic:** `{DeviceGroupID}/DEVICE/STATUS` | **WSS:** `/v3/devices/{DeviceGroupID}/status/ws`

```json
{
  "droneState": "GPS_NORMAL",
  "dockState": "CLOSED",
  "navigation": {
    "position": { "type": "Point", "coordinates": [-122.4194, 37.7749, 0] }
  },
  "weather": {
    "windSpeed": 5.2,
    "windDirection": 180,
    "temperature": 22.5,
    "humidity": 65,
    "rain": false
  },
  "timestamp": "2025-11-27T10:30:15.123Z"
}
```

---

### 6. Stream Share

`POST /v3/streams/share` (Requires MANAGER role)

```json
{
  "DeviceGroupID": "b775cc6e-1234-5678-90ab-cdef12345678",
  "role": "VIEWER",
  "timeout": 60
}
```

**Response (201):**
```json
{
  "shareID": "share-abc123def456",
  "shareLink": "https://app.idi-fly.com/shared/share-abc123def456",
  "expiresAt": "2025-11-27T11:30:00Z"
}
```

---

### 7-8. Track Share Status

**MQTTS Topic:** `{shareID}/SHARE/STATUS` | **WSS:** `/v3/shares/{shareID}/status/ws`

Uses `shareID` as credential. Payload filtered by role.

---

### 9. Get Resources

`GET /v3/shares/{sharecode}/resources`

```json
{
  "resources": ["drone-system-b775cc6e", "dock-platform-d99f01a3"]
}
```

---

## States

| Job | Drone | Dock |
|:----|:------|:-----|
| PENDING | MANUAL | OPEN |
| INPROGRESS | GPS_NORMAL | CLOSED |
| COMPLETED | WAYPOINT | OPENING |
| FAILED | HOVER | CLOSING |
| CANCELLED | ORBIT | COOLING |
| | LANDED | CHARGING |
| | DOCKED | TASK_INPROGRESS |
| | RETURNING | |

---

## Roles

| Role | Permissions |
|:-----|:------------|
| VIEWER | View telemetry and status |
| PILOT | VIEWER + Control missions |
| MANAGER | PILOT + Share streams, Manage resources |

---

## Rate Limits

| Endpoint | Limit |
|:---------|:------|
| POST /v3/jobs/create | 60/min |
| POST /v3/streams/share | 30/min |
| GET /v3/shares/{sharecode}/resources | 120/min |

**Connections:** MQTTS 100 subs/client | WSS 50 conn/client

---

## Error Codes

| Code | Status | Description |
|:-----|:-------|:------------|
| INVALID_TOKEN | 401 | JWT invalid or expired |
| INSUFFICIENT_PERMISSIONS | 403 | Missing required role |
| DEVICE_NOT_FOUND | 404 | DeviceGroupID not found |
| IDEMPOTENCY_CONFLICT | 409 | Duplicate key, different payload |
| DEVICE_OFFLINE | 503 | Device not online |

---

*IDI-FLY-V3 API v3.0.0*
