---
name: network
description: UniFi Network API reference and implementation patterns. Covers all endpoints across Sites, Devices, Clients, Networks, WiFi Broadcasts, Hotspot, Firewall, Port Forwarding, Traffic Routes, VPN, RADIUS, and Channel Planning. Use when implementing or debugging Network SDK endpoints.
when_to_use: Triggers on "network", "unifi network", "switch", "access point", "wifi", "vlan", "firewall", "client", "hotspot", "voucher", "port forward", "vpn", "radius", "wlan", "channel plan", or when working with Unifi.NET.Network code.
allowed-tools:
  - Read
  - Grep
  - Glob
---

# UniFi Network API

You are working with the UniFi Network API — a REST API for managing network infrastructure
(switches, APs, gateways, VLANs, firewall, clients) on UniFi Network hardware.

## Quick Reference

- **Base URL:** `https://{console-ip}/proxy/network/integration/v1/`
- **Auth:** API Key — `X-API-KEY: {key}`
- **Response format:** JSON with standard HTTP status codes
- **OpenAPI spec:** `docs/network-openapi.json` (OpenAPI 3.1.0, v10.2.105)

## API Documentation

For the complete API reference (all endpoints, schemas, request/response examples):

**Read:** `${CLAUDE_SKILL_DIR}/UniFi_Network_API_Reference.md`

The OpenAPI spec is available at: `docs/network-openapi.json`

## Endpoint Overview

| Area | Endpoints | Description |
|------|-----------|-------------|
| Application Info | 1 | Application metadata |
| Sites | 1 | List local sites |
| UniFi Devices | 8 | Adopt, manage, stats, pending devices |
| Clients | 3 | Connected clients, actions |
| Networks | 5 | CRUD, references |
| WiFi Broadcasts | 5 | WLAN CRUD |
| Hotspot | 5 | Vouchers, operators, payments |
| Firewall | 13 | Policies, rules, zones, groups |
| Port Forwarding | 5 | Port forward rules CRUD |
| Traffic Routes | 5 | Traffic routing rules CRUD |
| Traffic Management | 5 | Speed limits, routes, rules |
| VPN | 7 | Teleport, site-to-site, servers, clients |
| RADIUS | 6 | Profiles, users CRUD |
| Channel Planning | 1 | WiFi channel plan |
| DNS | 2 | DNS records |

## Authentication

The Network Integration API uses API Key authentication:

```
X-API-KEY: {your-api-key}
Accept: application/json
```

API keys are generated from the UniFi Network application settings.

## Error Handling

Standard HTTP status codes with JSON error body:
```json
{
  "statusCode": 404,
  "statusName": "NOT_FOUND",
  "code": "api.request.error",
  "message": "No endpoint GET /integration/v1/...",
  "timestamp": "...",
  "requestPath": "..."
}
```

## Filtering & Pagination

List endpoints support query parameters for filtering:
- Cursor-based pagination
- Field filtering via query params
- Sort ordering

## SDK Implementation Notes

- This API uses `X-API-KEY` header auth (NOT Bearer token like Access)
- The common auth abstraction must support both API key and Bearer token patterns
- Base path includes `/proxy/network/integration/v1/` when accessed via UniFi OS
- Response format differs from Access (no `code`/`msg`/`data` wrapper — direct JSON)
- Follow the DI pattern: `services.AddUnifi(opts => {...}).AddNetwork()`
