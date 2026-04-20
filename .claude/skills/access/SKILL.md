---
name: access
description: UniFi Access API reference and implementation patterns. Covers all 120+ endpoints across Users, Visitors, Access Policies, Credentials (NFC/PIN/Touch Pass), Doors, Devices, System Logs, Identity, Notifications, and Certificates. Use when implementing or debugging Access SDK endpoints.
when_to_use: Triggers on "access", "unifi access", "door", "nfc", "credential", "pin code", "touch pass", "visitor", "access policy", "schedule", "holiday group", "user group", "system log", "webhook", "door lock", or when working with Unifi.NET.Access code.
allowed-tools:
  - Read
  - Grep
  - Glob
---

# UniFi Access API

You are working with the UniFi Access API — a REST API for managing physical access control
(doors, users, credentials, policies) on UniFi Access hardware.

## Quick Reference

- **Base URL:** `https://{console-ip}:12445`
- **Auth:** Bearer token — `Authorization: Bearer {token}`
- **Response format:** `{ "code": "SUCCESS", "msg": "success", "data": {} }`
- **Min firmware:** 1.9.1 (some endpoints require later versions, noted in docs)
- **Port:** 12445 (HTTPS, self-signed cert)

## API Documentation

For the complete API reference (all endpoints, schemas, request/response examples):

**Read:** `${CLAUDE_SKILL_DIR}/UniFi_Access_API_Reference.md`

This is a 194-page reference covering:

| Section | Endpoints | Topic |
|---------|-----------|-------|
| 3. User | 30 | Users, groups, credentials assignment, profile photos |
| 4. Visitor | 14 | Visitor CRUD, credential assignment |
| 5. Access Policy | 18 | Policies, holiday groups, schedules |
| 6. Credential | 17+ | NFC enrollment, PIN, Touch Pass, QR codes |
| 7. Door | 13 | Doors, door groups, locking rules, emergency |
| 8. Device | 4 | Device listing, access method settings, doorbells |
| 9. System Log | 5 | Audit logs, exports, resources |
| 10. Identity | 6 | UniFi Identity Enterprise integration |
| 11. Notification | 7 | WebSocket events, webhook management |
| 12. API Server | 2 | HTTPS certificate management |

## Error Codes

Key error codes to handle:

- `CODE_PARAMS_INVALID` — invalid parameters
- `CODE_AUTH_FAILED` — authentication failure
- `CODE_CREDS_NFC_HAS_BIND_USER` — NFC card already assigned
- `CODE_CREDS_PIN_CODE_CREDS_ALREADY_EXIST` — PIN already exists
- `CODE_DEVICE_DEVICE_NOT_FOUND` — device not found
- `CODE_DEVICE_DEVICE_OFFLINE` — device offline
- `CODE_OTHERS_UID_ADOPTED_NOT_SUPPORTED` — API unavailable after Identity Enterprise upgrade

## SDK Implementation Pattern

```csharp
// 1. Models in Models/{ServiceArea}/
public class CreateUserRequest
{
    [JsonPropertyName("first_name")]
    public string FirstName { get; set; }
    
    [JsonPropertyName("last_name")]
    public string LastName { get; set; }
}

// 2. Register in Serialization/Contexts/{Area}JsonContext.cs
[JsonSourceGenerationOptions(WriteIndented = false)]
[JsonSerializable(typeof(UnifiApiResponse<UserResponse>))]
[JsonSerializable(typeof(CreateUserRequest))]
internal partial class UserJsonContext : JsonSerializerContext { }

// 3. Add to combined context in Serialization/UnifiAccessJsonContext.cs
public static IJsonTypeInfoResolver Combined =>
    JsonTypeInfoResolver.Combine(
        CoreJsonContext.Default,
        UserJsonContext.Default,
        // ...
    );

// 4. Interface in Services/IUserService.cs
public interface IUserService
{
    Task<UserResponse> CreateUserAsync(CreateUserRequest request, CancellationToken ct = default);
}

// 5. Implementation in Services/UserService.cs (uses BaseService)
var jsonTypeInfo = (JsonTypeInfo<UnifiApiResponse<UserResponse>>)
    _jsonOptions.GetTypeInfo(typeof(UnifiApiResponse<UserResponse>));
```

## Implementation Status

Check `API_IMPLEMENTATION_STATUS.md` at the repo root for current progress:
- P0 (Core): 100% — Users, Access Policies, Doors
- P1 (Essential): 43% — Credentials, User Groups, Devices
- P2 (Extended): 0% — Visitors, Schedules, Holiday Groups
- P3 (Advanced): 0% — WebSockets, Webhooks, Identity, Certificates
