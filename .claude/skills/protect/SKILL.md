---
name: protect
description: UniFi Protect API reference and implementation patterns. Covers all endpoints across Cameras, NVR, Lights, Sensors, Chimes, Viewers, Live Views, PTZ, WebSocket events, RTSPS streams, snapshots, talkback, and device assets. Use when implementing or debugging Protect SDK endpoints.
when_to_use: Triggers on "protect", "unifi protect", "camera", "nvr", "motion", "smart detection", "snapshot", "rtsps", "recording", "ptz", "doorbell", "chime", "sensor", "liveview", "talkback", or when working with Unifi.NET.Protect code.
allowed-tools:
  - Read
  - Grep
  - Glob
---

# UniFi Protect API

You are working with the UniFi Protect API — a REST API for managing video surveillance
(cameras, NVR, sensors, lights, chimes, smart detections, recordings) on UniFi Protect hardware.

## Quick Reference

- **Base URL:** `https://{console-ip}/proxy/protect/integration/v1/`
- **Auth:** API Key — `X-API-KEY: {key}`
- **Response format:** JSON with standard HTTP status codes
- **OpenAPI spec:** `docs/protect-openapi.json` (OpenAPI 3.1.0, v7.0.104)

## API Documentation

For the complete API reference (all endpoints, schemas, request/response examples):

**Read:** `${CLAUDE_SKILL_DIR}/UniFi_Protect_API_Reference.md`

The OpenAPI spec is available at: `docs/protect-openapi.json`

## Endpoint Overview

| Area | Endpoints | Description |
|------|-----------|-------------|
| Application Info | 1 | Get application information |
| Viewers | 3 | Viewer details, settings, list all |
| Live Views | 4 | CRUD for live view configurations |
| WebSocket | 2 | Device updates, Protect event messages |
| Camera PTZ | 3 | Start/stop patrol, move to preset |
| Alarm Manager | 1 | Webhook to alarm manager |
| Lights | 3 | Light details, settings, list all |
| Cameras | 9 | Details, settings, RTSPS streams, snapshots, talkback, mic disable |
| Sensors | 3 | Sensor details, settings, list all |
| NVR | 1 | NVR details |
| Device Assets | 2 | Upload/get device asset files |
| Chimes | 3 | Chime details, settings, list all |

## Authentication

The Protect Integration API uses API Key authentication:

```
X-API-KEY: {your-api-key}
Accept: application/json
```

API keys are generated from the UniFi Protect application or UniFi OS settings.

## Key Technical Considerations

### WebSocket Events
Protect provides two WebSocket endpoints for real-time updates:
- **Device updates** — state changes for cameras, sensors, lights, etc.
- **Protect events** — motion events, smart detections, doorbell rings

The SDK should support both polling (GET) and streaming (WebSocket) patterns.

### Binary Endpoints
Some endpoints return binary data, not JSON:
- **Camera snapshots** — returns JPEG image data
- **RTSPS streams** — returns stream URLs for video
- **Device assets** — file upload/download

These require special handling in the RestSharp/HTTP client layer — do NOT attempt
JSON deserialization on binary responses.

### RTSPS Streams
Camera streaming uses RTSPS (RTSP over TLS):
1. `POST /cameras/{id}/rtsps-streams` — create a stream
2. `GET /cameras/{id}/rtsps-streams` — list active streams
3. `DELETE /cameras/{id}/rtsps-streams/{streamId}` — delete a stream

### Camera Talkback
Two-way audio uses a talkback session:
1. `POST /cameras/{id}/talkback-sessions` — create session (returns WebSocket URL)
2. Stream audio over the WebSocket connection

## Error Handling

Standard HTTP status codes with JSON error body:
```json
{
  "error": "Entity not found",
  "name": "NOT_FOUND",
  "entity": "camera",
  "id": "...",
  "idKey": "id"
}
```

## SDK Implementation Notes

- Uses `X-API-KEY` header auth (same as Network, different from Access Bearer token)
- Base path includes `/proxy/protect/integration/v1/` when accessed via UniFi OS
- Response format differs from Access (no `code`/`msg`/`data` wrapper)
- Binary responses need distinct handling (snapshots, streams)
- WebSocket support is a differentiator for this SDK
- Follow the DI pattern: `services.AddUnifi(opts => {...}).AddProtect()`
