# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Linear Configuration

```yaml
LINEAR_PROJECT_ID: "62b16559-5a98-4a38-8980-bcfc96cc72f1"
LINEAR_PROJECT_NAME: "Unifi.NET"
LINEAR_TEAM_ID: "7fe71b21-cd2a-4776-98ef-8814b2921b8c"
LINEAR_TEAM_NAME: "Spire Recovery Solutions"

# Status IDs
LINEAR_STATUS_ON_DECK: "2ec690d5-e27e-45dc-bf26-9201f3b2e276"
LINEAR_STATUS_IN_PROGRESS: "d171b2d5-87f8-4c4e-b7d0-a13dd5bc6689"
LINEAR_STATUS_BLOCKED: "8a88d533-e078-425a-b1f1-ad93cad81ef2"
LINEAR_STATUS_IN_REVIEW: "6f1d6fc0-4e3c-42da-903a-85fcc3ea83e9"
LINEAR_STATUS_DONE: "cf75dc3e-2542-4918-b2da-980fac469533"

# Branch Configuration
DEFAULT_BRANCH: "main"
```

## Project Overview

Unifi.NET is a collection of .NET 9 SDKs for Ubiquiti UniFi APIs. The solution wraps UniFi service APIs (Access, Network, Protect, Site Manager) with strongly-typed, Native AOT-compatible client libraries.

## Solution Structure

```
Unifi.NET.sln
├── Unifi.NET.Common/            # Shared components and abstractions
├── Unifi.NET.Access/            # UniFi Access API SDK
├── Unifi.NET.Network/           # UniFi Network API SDK (placeholder)
├── Unifi.NET.Protect/           # UniFi Protect API SDK (placeholder)
├── Unifi.NET.SiteManager/       # UniFi Site Manager SDK (placeholder)
├── Unifi.NET.Common.Tests/      # Common library tests
├── Unifi.NET.Access.Tests/      # Access SDK tests
├── Unifi.NET.Samples/           # Sample console application
├── Directory.Build.props        # Shared MSBuild properties
├── Directory.Packages.props     # Central package management
├── global.json                  # .NET SDK version (9.0.100)
└── .editorconfig               # Code style configuration
```

## Build and Development Commands

**Note: dotnet CLI is located at `~/.dotnet/dotnet`**

```bash
# Build the entire solution
~/.dotnet/dotnet build

# Build a specific SDK project
~/.dotnet/dotnet build Unifi.NET.Access/Unifi.NET.Access.csproj

# Run tests
~/.dotnet/dotnet test

# Run tests for specific SDK
~/.dotnet/dotnet test Unifi.NET.Access.Tests

# Run tests with coverage
~/.dotnet/dotnet test /p:CollectCoverage=true /p:CoverletOutputFormat=opencover

# Verify Native AOT compatibility (must produce zero warnings)
~/.dotnet/dotnet publish -c Release -r linux-x64 --verbosity detailed | grep -i warning

# Publish with Native AOT
~/.dotnet/dotnet publish -c Release -r linux-x64
~/.dotnet/dotnet publish -c Release -r win-x64
~/.dotnet/dotnet publish -c Release -r osx-arm64

# Pack NuGet packages
~/.dotnet/dotnet pack -c Release

# Clean build artifacts
~/.dotnet/dotnet clean
```

## Architecture and Key Design Decisions

### Native AOT Requirements
This project MUST maintain full Native AOT compatibility. Every SDK component must:
- Use System.Text.Json source generators for ALL serialization
- Avoid ALL runtime reflection for serialization
- Configure `JsonSerializerContext` with `[JsonSerializable]` attributes
- Maintain zero AOT warnings policy

### MSBuild Configuration
These properties are configured in Directory.Build.props and apply to all projects:
- `PublishAot=true` - Enables Native AOT compilation
- `JsonSerializerIsReflectionEnabledByDefault=false` - Disables reflection-based JSON
- `IsAotCompatible=true` - Enables AOT analyzers
- `EnableConfigurationBindingGenerator=true` - Uses source generators for configuration
- `OptimizationPreference=Speed` - Optimizes for performance in AOT
- `ManagePackageVersionsCentrally=true` - Central package management

### Service Collection Pattern
SDKs use fluent extension methods:
```csharp
services.AddUnifi(options => { /* global config */ })
    .AddAccess()    // Unifi.NET.Access
    .AddNetwork()   // Unifi.NET.Network
    .AddProtect()   // Unifi.NET.Protect
```

### HTTP Client Stack
- **RestSharp** for HTTP operations (configured for AOT)
- **Polly v8+** for resilience via Microsoft.Extensions.Http.Resilience
- **IHttpClientFactory** for client management
- Bearer token authentication using UniFi API tokens

### Common Project Components
The Unifi.NET.Common project contains:
- `Configuration/UnifiConfiguration.cs` - Base configuration with API token, URL, retry settings
- `Abstractions/IUnifiClient.cs` - Base interface for all SDK clients
- `Models/UnifiApiResponse<T>` - Generic wrapper for API responses
- `Exceptions/UnifiException.cs` - Exception hierarchy (UnifiAuthenticationException, UnifiNotFoundException)
- `Constants/UnifiConstants.cs` - Shared constants and error codes

### Versioning Strategy
Package versions follow UniFi API versions with revision suffix:
- `3.3.21` - Matches UniFi Access API v3.3.21
- `3.3.21.1` - Same API version, SDK fixes
- `3.4.0` - New UniFi API version

### Authentication
All SDKs use Bearer token authentication:
- Tokens obtained from UniFi Portal
- Configured via `UnifiConfiguration.ApiToken`
- Header: `Authorization: Bearer {token}`

## UniFi API Reference

### UniFi Access API
- Base URL: `https://{console-ip}:12445`
- API Documentation: `Unifi.NET.Access/UniFi_Access_API_Reference.md`
- Minimum Version: 1.9.1
- Note: Not available after Identity Enterprise upgrade

### API Response Format
```json
{
    "code": "SUCCESS",
    "msg": "success",
    "data": {}
}
```

## Critical Implementation Notes

### JSON Serialization
The project uses ASP.NET Core-style Native AOT patterns:

#### Architecture
```csharp
// Service-specific contexts in /Serialization/Contexts/
[JsonSourceGenerationOptions(WriteIndented = false)]
[JsonSerializable(typeof(UserResponse))]
internal partial class UserJsonContext : JsonSerializerContext { }

// Combined using TypeInfoResolver
public static class UnifiAccessJsonContext
{
    public static IJsonTypeInfoResolver Combined => 
        JsonTypeInfoResolver.Combine(
            CoreJsonContext.Default,
            UserJsonContext.Default,
            // ... other contexts
        );
}

// Runtime type resolution in BaseService
var jsonTypeInfo = (JsonTypeInfo<T>)_jsonOptions.GetTypeInfo(typeof(T));
var result = JsonSerializer.Deserialize(response.Content, jsonTypeInfo);
```

#### Key Patterns
- Service-specific `JsonSerializerContext` classes for organization
- `JsonTypeInfoResolver.Combine()` to merge all contexts
- `JsonTypeInfo<T>` and `GetTypeInfo()` for runtime type resolution
- No manual type mapping or reflection

### Error Handling
UniFi APIs return structured error codes (e.g., `CODE_PARAMS_INVALID`, `CODE_AUTH_FAILED`). Map these to typed exceptions.

### Testing Requirements
- All new endpoints must have unit tests
- Integration tests should mock UniFi responses
- AOT compatibility must be verified with `~/.dotnet/dotnet publish`

### Package Naming
NuGet packages follow folder structure:
- `Unifi.NET.Access`
- `Unifi.NET.Network`
- `Unifi.NET.Protect`
- `Unifi.NET.SiteManager`

## Package Dependencies

Central package versions (Directory.Packages.props):
- **RestSharp**: 112.1.0
- **Microsoft.Extensions.Http.Resilience**: 9.0.0
- **System.Text.Json**: 9.0.0
- **xunit**: 2.9.2
- **FluentAssertions**: 6.12.2
- **Moq**: 4.20.72

## API Implementation Workflow

### 📊 Progress Tracking
**ALWAYS check `API_IMPLEMENTATION_STATUS.md` first** - This document tracks all 120+ endpoints with:
- Implementation status for each endpoint
- Priority levels (P0-P3)
- Testing completion
- Progress metrics

### 🎯 Implementation Priority
1. **P0 (Core)** - Start here: Authentication, Users, Doors, Access Policies
2. **P1 (Essential)** - Credentials (NFC, PIN), User Groups, Door Groups
3. **P2 (Extended)** - Visitors, Schedules, Holiday Groups
4. **P3 (Advanced)** - WebSockets, Webhooks, Identity, Certificates

### 📝 Adding New Endpoints
Follow the workflow in `CONTRIBUTING.md`:

1. **Check Status** - Review `API_IMPLEMENTATION_STATUS.md`
2. **Create Models** - Add request/response DTOs with `[JsonPropertyName]`
3. **Register Models** - Add to `UnifiAccessJsonContext` for source generation
4. **Update Interface** - Add method to appropriate service interface
5. **Implement** - Create service implementation with RestSharp
6. **Test** - Write unit and integration tests
7. **Document** - Add XML docs and update tracking
8. **Verify AOT** - Ensure zero warnings with `~/.dotnet/dotnet publish`
9. **Update Progress** - Check off in `API_IMPLEMENTATION_STATUS.md`

### 🗂️ Service Organization
UniFi Access API is organized into service interfaces:
- `IUserService` - Users and user groups (26 endpoints)
- `IVisitorService` - Visitor management (13 endpoints)
- `IAccessPolicyService` - Policies, schedules, holidays (12 endpoints)
- `ICredentialService` - NFC, PIN, Touch Pass, QR (19 endpoints)
- `IDoorService` - Doors and door groups (13 endpoints)
- `IDeviceService` - Device management (3 endpoints)
- `ISystemLogService` - Audit logs (5 endpoints)
- `IIdentityService` - UniFi Identity integration (6 endpoints)
- `INotificationService` - WebSockets and webhooks (7 endpoints)
- `ICertificateService` - HTTPS certificates (2 endpoints)

## Current Development Status

**Completed Infrastructure:**
- ✅ Solution structure with 8 projects
- ✅ Directory.Build.props with Native AOT configuration
- ✅ Central package management
- ✅ Common library with base abstractions
- ✅ Test project structure
- ✅ API implementation tracking system (`API_IMPLEMENTATION_STATUS.md`)
- ✅ Service interface scaffolding
- ✅ Contributing guidelines (`CONTRIBUTING.md`)
- ✅ ASP.NET Core-style JSON serialization with TypeInfoResolver
- ✅ Full Native AOT compatibility with zero warnings

**UniFi Access SDK Status:**
- 📋 **Total Endpoints**: 120+
- ✅ **Implemented**: 45/120 (37.5%)
  - Core user management (10 endpoints)
  - User groups (10 endpoints)
  - NFC card management (9 endpoints)
  - PIN code management (3 endpoints)
  - Access policies (5 endpoints)
  - Doors (7 endpoints)
  - Devices (1 endpoint)
- 🧪 **Tested**: Basic unit tests + integration tested against live UniFi Access
- 📝 **Documented**: Full API reference + comprehensive sample application

**Recent Improvements:**
- ✅ Refactored to ASP.NET Core Native AOT patterns
- ✅ Fixed device listing to return all devices (flattened nested arrays)
- ✅ Fixed UserGroupResponse model to match actual API response
- ✅ Added comprehensive sample application with 17+ menu options
- ✅ Successfully tested against production UniFi Access instance

**Next Steps:**
1. Continue implementing remaining P1/P2 endpoints
2. Add comprehensive unit tests for all services
3. Implement webhook/WebSocket support for real-time events

**Future SDK Phases:**
- Phase 2: UniFi Network SDK
- Phase 3: UniFi Protect SDK  
- Phase 4: UniFi Site Manager SDK
- Never mention Claude when making Git Commits