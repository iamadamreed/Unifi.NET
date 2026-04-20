---
name: common
description: Shared Unifi.NET SDK patterns — Native AOT, System.Text.Json source generation, RestSharp configuration, DI registration, exception handling, and project structure. Load when implementing any SDK feature or reviewing for standards compliance.
when_to_use: Triggers on "AOT", "native aot", "source generator", "json context", "serialization", "RestSharp", "DI", "dependency injection", "IHttpClientFactory", "Polly", "resilience", or when working across multiple Unifi.NET projects.
allowed-tools:
  - Read
  - Grep
  - Glob
---

# Unifi.NET Common Patterns

## Native AOT Requirements (Non-Negotiable)

Every SDK project MUST maintain full Native AOT compatibility:

- `PublishAot=true` in Directory.Build.props
- `JsonSerializerIsReflectionEnabledByDefault=false`
- `IsAotCompatible=true` (enables AOT analyzers)
- Zero AOT warnings policy — verify with:
  ```bash
  ~/.dotnet/dotnet publish -c Release -r linux-x64 --verbosity detailed 2>&1 | grep -i warning
  ```

### What breaks AOT

- ANY reflection-based serialization
- `JsonSerializer.Serialize<T>()` without JsonTypeInfo
- `dynamic` keyword
- `Assembly.GetTypes()`, `Type.GetProperties()`
- Runtime code generation
- Unconstrained generic serialization

## System.Text.Json Source Generation

### Architecture

```
Unifi.NET.{Product}/
└── Serialization/
    ├── Contexts/
    │   ├── UserJsonContext.cs        # Service-specific
    │   ├── DoorJsonContext.cs        # Service-specific
    │   └── CoreJsonContext.cs        # Shared (errors, pagination)
    └── Unifi{Product}JsonContext.cs  # Combined resolver
```

### Pattern

```csharp
// Service-specific context (internal, per service area)
[JsonSourceGenerationOptions(WriteIndented = false)]
[JsonSerializable(typeof(UnifiApiResponse<UserResponse>))]
[JsonSerializable(typeof(CreateUserRequest))]
[JsonSerializable(typeof(List<UserResponse>))]
internal partial class UserJsonContext : JsonSerializerContext { }

// Combined context (public, one per SDK)
public static class UnifiAccessJsonContext
{
    public static IJsonTypeInfoResolver Combined =>
        JsonTypeInfoResolver.Combine(
            CoreJsonContext.Default,
            UserJsonContext.Default,
            DoorJsonContext.Default,
            // ... all service contexts
        );
}

// Usage in BaseService (runtime resolution)
var jsonTypeInfo = (JsonTypeInfo<T>)_jsonOptions.GetTypeInfo(typeof(T));
var result = JsonSerializer.Deserialize(content, jsonTypeInfo);
```

### Rules

- Every DTO needs `[JsonPropertyName("snake_case")]` on all properties
- Register `UnifiApiResponse<T>` for every response type
- Register `List<T>` for collection endpoints
- Enums: use `[JsonConverter]` with `JsonStringEnumConverter<T>` source gen
- Nullable: use `T?`, not default values

## RestSharp + HTTP Client Stack

- RestSharp for HTTP operations (configured for AOT)
- Polly v8+ for resilience via `Microsoft.Extensions.Http.Resilience`
- `IHttpClientFactory` for client lifecycle
- Bearer token auth via custom `AuthenticationHandler`

```csharp
// RestSharp request with AOT-safe serialization
var request = new RestRequest("/api/v1/developer/users", Method.Post);
request.AddJsonBody(createUserRequest, jsonTypeInfo);
var response = await _client.ExecuteAsync(request, ct);
```

## DI Registration Pattern

```csharp
// Consumer usage
services.AddUnifi(options =>
{
    options.Host = "https://192.168.1.1:12445";
    options.ApiToken = "your-token";
    options.MaxRetryAttempts = 3;
})
.AddAccess()     // Registers IUnifiAccessClient + all services
.AddNetwork()    // Registers IUnifiNetworkClient + all services
.AddProtect();   // Registers IUnifiProtectClient + all services

// Extension method pattern (per SDK)
public static IUnifiBuilder AddAccess(this IUnifiBuilder builder)
{
    builder.Services.AddSingleton<IUnifiAccessClient, UnifiAccessClient>();
    builder.Services.AddSingleton<IUserService, UserService>();
    // ... all services
    return builder;
}
```

## Exception Hierarchy

```
UnifiException (base)
├── UnifiAuthenticationException  — 401, CODE_AUTH_FAILED
├── UnifiNotFoundException        — 404, CODE_*_NOT_FOUND
├── UnifiValidationException      — 400, CODE_PARAMS_INVALID
└── UnifiApiException             — Other API errors with code + msg
```

## Project Structure

```
Unifi.NET.{Product}/
├── Configuration/           # Product-specific config extensions
├── Exceptions/              # Product-specific exceptions (if any)
├── Extensions/              # DI registration (AddXxx)
├── Models/                  # Request/response DTOs
│   ├── Users/
│   ├── Doors/
│   └── ...
├── Serialization/           # JSON contexts (see above)
├── Services/                # Service interfaces + implementations
│   ├── IUserService.cs
│   ├── UserService.cs
│   └── ...
├── I{Product}Client.cs      # Top-level client interface
└── {Product}Client.cs       # Top-level client implementation
```

## Versioning

Package versions follow UniFi API versions:
- `3.3.21` — matches UniFi Access API v3.3.21
- `3.3.21.1` — same API version, SDK fixes
- `3.4.0` — new UniFi API version

## Build Commands

```bash
~/.dotnet/dotnet build Unifi.NET.sln          # Build all
~/.dotnet/dotnet test                          # Run all tests
~/.dotnet/dotnet publish -c Release -r linux-x64  # AOT verify
~/.dotnet/dotnet pack -c Release              # NuGet packages
```

## Testing

- xUnit for test framework
- FluentAssertions for assertions
- Moq for mocking HTTP responses
- Test JSON deserialization with sample payloads from API docs
- Verify AOT compatibility as part of CI
