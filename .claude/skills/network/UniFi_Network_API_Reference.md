Title: Control Plane

URL Source: blob:76adf8d2bc892132de0eba45077ffef669c818ab8ef6348b329f587141ce5a62

Markdown Content:
# Control Plane

S3-UDM

[Network](/network/default "Network")

![Image 1: Adam reed](blob:http://localhost/9b4fa6bae9f443900f54f79b408e0441)

*   Getting Started 
*   Filtering 
*   Error Handling 
    *   schema Error Message 

*   Application Info 
    *   get Get Application Info 

*   Sites 
    *   get List Local Sites 

*   UniFi Devices 
    *   get List Adopted Devices 
    *   post Adopt Devices 
    *   post Execute Port Action 
    *   post Execute Adopted Device Action 
    *   get Get Adopted Device Details 
    *   del Remove (Unadopt) Device 
    *   get Get Latest Adopted Device Statistics 
    *   get List Devices Pending Adoption 

*   Clients 
    *   post Execute Client Action 
    *   get List Connected Clients 
    *   get Get Connected Client Details 

*   Networks 
    *   get Get Network Details 
    *   put Update Network 
    *   del Delete Network 
    *   get List Networks 
    *   post Create Network 
    *   get Get Network References 

*   WiFi Broadcasts 
    *   get Get Wifi Broadcast Details 
    *   put Update Wifi Broadcast 
    *   del Delete Wifi Broadcast 
    *   get List Wifi Broadcasts 
    *   post Create Wifi Broadcast 

*   Hotspot 
    *   get List Vouchers 
    *   post Generate Vouchers 
    *   del Delete Vouchers 
    *   get Get Voucher Details 
    *   del Delete Voucher 

*   Firewall 
    *   get Get Firewall Zone 
    *   put Update Firewall Zone 
    *   del Delete Custom Firewall Zone 
    *   get Get Firewall Policy 
    *   put Update Firewall Policy 
    *   del Delete Firewall Policy 
    *   patch Patch Firewall Policy 
    *   get Get User-Defined Firewall Policy Ordering 
    *   put Reorder User-Defined Firewall Policies 
    *   get List Firewall Zones 
    *   post Create Custom Firewall Zone 
    *   get List Firewall Policies 
    *   post Create Firewall Policy 

*   Access Control (ACL Rules) 
    *   get Get ACL Rule 
    *   put Update ACL Rule 
    *   del Delete ACL Rule 
    *   get Get User-Defined ACL Rule Ordering 
    *   put Reorder User-Defined ACL Rules 
    *   get List ACL Rules 
    *   post Create ACL Rule 

*   Switching 
    *   get List Switch Stacks 
    *   get Get Switch Stack 
    *   get List MC-LAG Domains 
    *   get Get MC-LAG Domain 
    *   get List LAGs 
    *   get Get LAG Details 

*   DNS Policies 
    *   get Get DNS Policy 
    *   put Update DNS Policy 
    *   del Delete DNS Policy 
    *   get List DNS Policies 
    *   post Create DNS Policy 

*   Traffic Matching Lists 
    *   get Get Traffic Matching List 
    *   put Update Traffic Matching List 
    *   del Delete Traffic Matching List 
    *   get List Traffic Matching Lists 
    *   post Create Traffic Matching List 

*   Supporting Resources 
    *   get List WAN Interfaces 
    *   get List Site-To-Site VPN Tunnels 
    *   get List VPN Servers 
    *   get List Radius Profiles 
    *   get List Device Tags 
    *   get List DPI Application Categories 
    *   get List DPI Applications 
    *   get List Countries 

[![Image 2: redocly logo](https://cdn.redoc.ly/redoc/logo-mini.svg)API docs by Redocly](https://redocly.com/redoc/)

Dream Machine Pro UniFi Network API

Network Statistics

UniFi Device List

Client Device List

Settings (Coming Soon)

Site A

CloudKey+UniFi Network API

Network Statistics

UniFi Device List

Client Device List

Settings (Coming Soon)

Site B

Official UniFi Hosting UniFi Network API

Network Statistics

UniFi Device List

Client Device List

Settings (Coming Soon)

Site C

Network Statistics

UniFi Device List

Client Device List

Settings (Coming Soon)

Site D

Site Manager API

Site List

Online Status

Internet Health Metrics

Client Device Counts

Updates Available

# UniFi Network API (10.2.105)

Download OpenAPI specification:[Download](blob:https://10.103.0.1/b8ce2d3a-822e-4f56-ad4f-e96f24c5fbdf)

License: []()

## [](#tag/Getting-Started)Getting Started

Provides an overview of the UniFi Network API, including authentication using API keys and request format. Start here to understand how to connect and make your first request.

### Introduction

Each UniFi Application has its own API endpoints running locally on each site, offering detailed analytics and control related to that specific application. For a single endpoint with high-level insights across all your UniFi sites, refer to the [UniFi Site Manager API](https://developer.ui.com).

### Authentication and Request Format

An API Key is a unique identifier used to authenticate API requests. To generate API Keys and view an example of the API Request Format, visit the Integrations section of your UniFi application.

## [](#tag/Filtering)Filtering

Explains how to use the filter query parameter for advanced querying across list endpoints, including supported property types, syntax, and operators.

Some `GET` and `DELETE` endpoints support filtering using the `filter` query parameter. Each endpoint supporting filtering will have a detailed list of filterable properties, their types, and allowed functions.

### Filtering Syntax

Filtering follows a structured, URL-safe syntax with three types of expressions.

#### 1. Property Expressions

Apply functions to an individual property using the form `<property>.<function>(<arguments>)`, where argument values are separated by commas.

Examples:

*   `id.eq(123)` checks if `id` is equal to `123`;
*   `name.isNotNull()` checks if `name` is not null;
*   `createdAt.in(2025-01-01, 2025-01-05)` checks if `createdAt` is either `2025-01-01` or `2025-01-05`.

#### 2. Compound Expressions

Combine two or more expressions with logical operators using the form `<logical-operator>(<expressions>)`, where expressions are separated by commas.

Examples:

*   `and(name.isNull(), createdAt.gt(2025-01-01))` checks if `name` is null **and**`createdAt` is greater than `2025-01-01`;
*   `or(name.isNull(), expired.isNull(), expiresAt.isNull())` check is **any** of `name`, `expired`, or `expiresAt` is null.

#### 3. Negation Expressions

Negate any other expressions using the the form `not(<expression>)`.

Example:

*   `not(name.like('guest*'))` matches all values except those that start with `guest`.

### Filterable Property Types

The table below lists all supported property types.

| Type | Examples | Syntax |
| --- | --- | --- |
| `STRING` | `'Hello, ''World''!'` | Must be wrapped in single quotes. To escape a single quote, use another single quote. |
| `INTEGER` | `123` | Must start with a digit. |
| `DECIMAL` | `123`, `123.321` | Must start with a digit. Can include a decimal point (.). |
| `TIMESTAMP` | `2025-01-29`, `2025-01-29T12:39:11Z` | Must follow ISO 8601 format (date or date-time). |
| `BOOLEAN` | `true`, `false` | Can be `true` or `false`. |
| `UUID` | `550e8400-e29b-41d4-a716-446655440000` | Must be a valid UUID format (8-4-4-4-12). |
| `SET(STRING|INTEGER|DECIMAL|TIMESTAMP|UUID)` | `[1, 2, 3, 4, 5]` | A set of (unique) values. |

### Filtering Functions

The table below lists available filtering functions, their arguments, and applicable property types:

| Function | Arguments | Semantics | Supported property types |
| --- | --- | --- | --- |
| `isNull` | 0 | is null | all types |
| `isNotNull` | 0 | is not null | all types |
| `eq` | 1 | equals | `STRING`, `INTEGER`, `DECIMAL`, `TIMESTAMP`, `BOOLEAN`, `UUID` |
| `ne` | 1 | not equals | `STRING`, `INTEGER`, `DECIMAL`, `TIMESTAMP`, `BOOLEAN`, `UUID` |
| `gt` | 1 | greater than | `STRING`, `INTEGER`, `DECIMAL`, `TIMESTAMP`, `UUID` |
| `ge` | 1 | greater than or equals | `STRING`, `INTEGER`, `DECIMAL`, `TIMESTAMP`, `UUID` |
| `lt` | 1 | less than | `STRING`, `INTEGER`, `DECIMAL`, `TIMESTAMP`, `UUID` |
| `le` | 1 | less than or equals | `STRING`, `INTEGER`, `DECIMAL`, `TIMESTAMP`, `UUID` |
| `like` | 1 | matches pattern | `STRING` |
| `in` | 1 or more | one of | `STRING`, `INTEGER`, `DECIMAL`, `TIMESTAMP`, `UUID` |
| `notIn` | 1 or more | not one of | `STRING`, `INTEGER`, `DECIMAL`, `TIMESTAMP`, `UUID` |
| `isEmpty` | 0 | is empty | `SET` |
| `contains` | 1 | contains | `SET` |
| `containsAny` | 1 or more | contains any of | `SET` |
| `containsAll` | 1 or more | contains all of | `SET` |
| `containsExactly` | 1 or more | contains exactly | `SET` |

#### Pattern Matching (`like` Function)

The `like` function allows matching string properties using simple patterns:

*   `.` matches any **single** character. Example: `type.like('type.')` matches `type1`, but not `type100`;
*   `*` matches **any number** of characters. Example: `name.like('guest*')` matches `guest1` and `guest100`;
*   `\` is used to escape `.` and `*`.

## [](#tag/Error-Handling)Error Handling

Describes the standard API error response structure, including error codes, status names, and troubleshooting guidance.

## [](#schema/Error-Message)Error Message

statusCode integer<int32>
statusName string
code string
message string
timestamp string<date-time>
requestPath string
requestId string<uuid>

In case of Internal Server Error (core = 500), request ID can be used to track down the error in the server log

Copy

`{"statusCode": 400,"statusName": "UNAUTHORIZED","code": "api.authentication.missing-credentials","message": "Missing credentials","timestamp": "2024-11-27T08:13:46.966Z","requestPath": "/integration/v1/sites/123","requestId": "3fa85f64-5717-4562-b3fc-2c963f66afa6"}`

## [](#tag/Application-Info)Application Info

Returns general details about the UniFi Network application, including version and runtime metadata. Useful for integration validation.

## [](#tag/Application-Info/operation/getInfo)Get Application Info

Retrieve general information about the UniFi Network application.

### Responses

**200**

OK

get/v1/info

https://10.103.0.1/proxy/network/integration/v1/info

### Response samples

*   200

Content type

application/json

Copy

`{"applicationVersion": "9.1.0"}`

## [](#tag/Sites)Sites

Endpoints for listing and managing UniFi sites within a local Network application. Site ID is required for most other API requests.

## [](#tag/Sites/operation/getSiteOverviewPage)List Local Sites

Retrieve a paginated list of local sites managed by this Network application. Site ID is required for other UniFi Network API calls.

Filterable properties (click to expand)
| Name | Type | Allowed functions |
| --- | --- | --- |
| `id` | `UUID` | `eq``ne``in``notIn` |
| `internalReference` | `STRING` | `eq``ne``in``notIn` |
| `name` | `STRING` | `eq``ne``in``notIn` |

##### query Parameters

offset integer<int32>>= 0 

 Default: 0
limit integer<int32> [ 0 .. 200 ] 

 Default: 25
filter string

### Responses

**200**

OK

get/v1/sites

https://10.103.0.1/proxy/network/integration/v1/sites

### Response samples

*   200

Content type

application/json

Copy

 Expand all  Collapse all 

`{"offset": 0,"limit": 25,"count": 10,"totalCount": 1000,"data": [{"id": "497f6eca-6276-4993-bfeb-53cbbbba6f08","internalReference": "string","name": "string"}]}`

## [](#tag/UniFi-Devices)UniFi Devices

Endpoints to list, inspect, and interact with UniFi devices, including adopted and pending devices. Provides device stats, port control, and actions.

## [](#tag/UniFi-Devices/operation/getAdoptedDeviceOverviewPage)List Adopted Devices

Retrieve a paginated list of all adopted devices on a site, including basic device information.

Filterable properties (click to expand)
| Name | Type | Allowed functions |
| --- | --- | --- |
| `id` | `UUID` | `eq``ne``in``notIn` |
| `macAddress` | `STRING` | `eq``ne``in``notIn` |
| `ipAddress` | `STRING` | `eq``ne``in``notIn` |
| `name` | `STRING` | `eq``ne``in``notIn``like` |
| `model` | `STRING` | `eq``ne``in``notIn` |
| `state` | `STRING` | `eq``ne``in``notIn` |
| `supported` | `BOOLEAN` | `eq``ne` |
| `firmwareVersion` | `STRING` | `isNull``isNotNull``eq``ne``gt``ge``lt``le``like``in``notIn` |
| `firmwareUpdatable` | `BOOLEAN` | `eq``ne` |
| `features` | `SET(STRING)` | `isEmpty``contains``containsAny``containsAll``containsExactly` |
| `interfaces` | `SET(STRING)` | `isEmpty``contains``containsAny``containsAll``containsExactly` |

##### path Parameters

siteId

required string<uuid>

##### query Parameters

offset integer<int32>>= 0 

 Default: 0
limit integer<int32> [ 0 .. 200 ] 

 Default: 25
filter string

### Responses

**200**

OK

get/v1/sites/{siteId}/devices

https://10.103.0.1/proxy/network/integration/v1/sites/{siteId}/devices

### Response samples

*   200

Content type

application/json

Copy

 Expand all  Collapse all 

`{"offset": 0,"limit": 25,"count": 10,"totalCount": 1000,"data": [{"id": "497f6eca-6276-4993-bfeb-53cbbbba6f08","macAddress": "94:2a:6f:26:c6:ca","ipAddress": "192.168.1.55","name": "IW HD","model": "UHDIW","state": "ONLINE","supported": true,"firmwareVersion": "6.6.55","firmwareUpdatable": true,"features": ["switching"],"interfaces": ["ports"]}]}`

## [](#tag/UniFi-Devices/operation/adoptDevice)Adopt Devices

Adopt a device to a site.

##### path Parameters

siteId

required string<uuid>

##### Request Body schema: application/json

required

macAddress

required string
ignoreDeviceLimit

required boolean

### Responses

**200**

OK

post/v1/sites/{siteId}/devices

https://10.103.0.1/proxy/network/integration/v1/sites/{siteId}/devices

### Request samples

*   Payload

Content type

application/json

Copy

`{"macAddress": "string","ignoreDeviceLimit": true}`

### Response samples

*   200

Content type

application/json

Copy

 Expand all  Collapse all 

`{"id": "497f6eca-6276-4993-bfeb-53cbbbba6f08","macAddress": "94:2a:6f:26:c6:ca","ipAddress": "192.168.1.55","name": "IW HD","model": "UHDIW","supported": true,"state": "ONLINE","firmwareVersion": "6.6.55","firmwareUpdatable": true,"adoptedAt": "2019-08-24T14:15:22Z","provisionedAt": "2019-08-24T14:15:22Z","configurationId": "7596498d2f367dc2","uplink": {"deviceId": "4de4adb9-21ee-47e3-aeb4-8cf8ed6c109a"},"features": {"switching": null,"accessPoint": null},"interfaces": {"ports": [{"idx": 1,"state": "UP","connector": "RJ45","maxSpeedMbps": 10000,"speedMbps": 1000,"poe": {"standard": "802.3bt","type": 3,"enabled": true,"state": "UP"}}],"radios": [{"wlanStandard": "802.11a","frequencyGHz": 2.4,"channelWidthMHz": 40,"channel": 36}]}}`

## [](#tag/UniFi-Devices/operation/executePortAction)Execute Port Action

Perform an action on a specific device port. The request body must include the action name and any applicable input arguments.

##### path Parameters

portIdx

required integer<int32>
siteId

required string<uuid>
deviceId

required string<uuid>

##### Request Body schema: application/json

required

action

required string

POWER_CYCLE

### Responses

**200**

OK

post/v1/sites/{siteId}/devices/{deviceId}/interfaces/ports/{portIdx}/actions

https://10.103.0.1/proxy/network/integration/v1/sites/{siteId}/devices/{deviceId}/interfaces/ports/{portIdx}/actions

### Request samples

*   Payload

Content type

application/json

Copy

`{"action": "POWER_CYCLE"}`

## [](#tag/UniFi-Devices/operation/executeAdoptedDeviceAction)Execute Adopted Device Action

Perform an action on an specific adopted device. The request body must include the action name and any applicable input arguments.

##### path Parameters

siteId

required string<uuid>
deviceId

required string<uuid>

##### Request Body schema: application/json

required

action

required string

RESTART

### Responses

**200**

OK

post/v1/sites/{siteId}/devices/{deviceId}/actions

https://10.103.0.1/proxy/network/integration/v1/sites/{siteId}/devices/{deviceId}/actions

### Request samples

*   Payload

Content type

application/json

Copy

`{"action": "RESTART"}`

## [](#tag/UniFi-Devices/operation/getAdoptedDeviceDetails)Get Adopted Device Details

Retrieve detailed information about a specific adopted device, including firmware versioning, uplink state, details about device features and interfaces (ports, radios) and other key attributes.

##### path Parameters

siteId

required string<uuid>
deviceId

required string<uuid>

### Responses

**200**

OK

get/v1/sites/{siteId}/devices/{deviceId}

https://10.103.0.1/proxy/network/integration/v1/sites/{siteId}/devices/{deviceId}

### Response samples

*   200

Content type

application/json

Copy

 Expand all  Collapse all 

`{"id": "497f6eca-6276-4993-bfeb-53cbbbba6f08","macAddress": "94:2a:6f:26:c6:ca","ipAddress": "192.168.1.55","name": "IW HD","model": "UHDIW","supported": true,"state": "ONLINE","firmwareVersion": "6.6.55","firmwareUpdatable": true,"adoptedAt": "2019-08-24T14:15:22Z","provisionedAt": "2019-08-24T14:15:22Z","configurationId": "7596498d2f367dc2","uplink": {"deviceId": "4de4adb9-21ee-47e3-aeb4-8cf8ed6c109a"},"features": {"switching": null,"accessPoint": null},"interfaces": {"ports": [{"idx": 1,"state": "UP","connector": "RJ45","maxSpeedMbps": 10000,"speedMbps": 1000,"poe": {"standard": "802.3bt","type": 3,"enabled": true,"state": "UP"}}],"radios": [{"wlanStandard": "802.11a","frequencyGHz": 2.4,"channelWidthMHz": 40,"channel": 36}]}}`

## [](#tag/UniFi-Devices/operation/removeDevice)Remove (Unadopt) Device

Removes (unadopts) an adopted device from the site. If the device is online, it will be reset to factory defaults.

##### path Parameters

siteId

required string<uuid>
deviceId

required string<uuid>

### Responses

**200**

OK

delete/v1/sites/{siteId}/devices/{deviceId}

https://10.103.0.1/proxy/network/integration/v1/sites/{siteId}/devices/{deviceId}

## [](#tag/UniFi-Devices/operation/getAdoptedDeviceLatestStatistics)Get Latest Adopted Device Statistics

Retrieve the latest real-time statistics of a specific adopted device, such as uptime, data transmission rates, CPU and memory utilization.

##### path Parameters

siteId

required string<uuid>
deviceId

required string<uuid>

### Responses

**200**

OK

get/v1/sites/{siteId}/devices/{deviceId}/statistics/latest

https://10.103.0.1/proxy/network/integration/v1/sites/{siteId}/devices/{deviceId}/statistics/latest

### Response samples

*   200

Content type

application/json

Copy

 Expand all  Collapse all 

`{"uptimeSec": 0,"lastHeartbeatAt": "2019-08-24T14:15:22Z","nextHeartbeatAt": "2019-08-24T14:15:22Z","loadAverage1Min": 0.1,"loadAverage5Min": 0.1,"loadAverage15Min": 0.1,"cpuUtilizationPct": 0.1,"memoryUtilizationPct": 0.1,"uplink": {"txRateBps": 0,"rxRateBps": 0},"interfaces": {"radios": [{"frequencyGHz": 2.4,"txRetriesPct": 0.1}]}}`

## [](#tag/UniFi-Devices/operation/getPendingDevicePage)List Devices Pending Adoption

Retrieve a paginated list of devices pending adoption, including basic device information.

Filterable properties (click to expand)
| Name | Type | Allowed functions |
| --- | --- | --- |
| `macAddress` | `STRING` | `eq``ne``in``notIn` |
| `ipAddress` | `STRING` | `eq``ne``in``notIn` |
| `model` | `STRING` | `eq``ne``in``notIn` |
| `state` | `STRING` | `eq``ne``in``notIn` |
| `supported` | `BOOLEAN` | `eq``ne` |
| `firmwareVersion` | `STRING` | `isNull``isNotNull``eq``ne``gt``ge``lt``le``like``in``notIn` |
| `firmwareUpdatable` | `BOOLEAN` | `eq``ne` |
| `features` | `SET(STRING)` | `isEmpty``contains``containsAny``containsAll``containsExactly` |

##### query Parameters

offset integer<int32>>= 0 

 Default: 0
limit integer<int32> [ 0 .. 200 ] 

 Default: 25
filter string

### Responses

**200**

OK

get/v1/pending-devices

https://10.103.0.1/proxy/network/integration/v1/pending-devices

### Response samples

*   200

Content type

application/json

Copy

 Expand all  Collapse all 

`{"offset": 0,"limit": 25,"count": 10,"totalCount": 1000,"data": [{"macAddress": "94:2a:6f:26:c6:ca","ipAddress": "192.168.1.55","model": "UHDIW","state": "ONLINE","supported": true,"firmwareVersion": "6.6.55","firmwareUpdatable": true,"features": ["switching"],"adoptionTargetSiteIds": ["497f6eca-6276-4993-bfeb-53cbbbba6f08"]}]}`

## [](#tag/Clients)Clients

Endpoints for viewing and managing connected clients (wired, wireless, VPN, and guest). Supports actions such as authorizing or unauthorizing guest access.

## [](#tag/Clients/operation/executeConnectedClientAction)Execute Client Action

Perform an action on a specific connected client. The request body must include the action name and any applicable input arguments.

##### path Parameters

clientId

required string<uuid>
siteId

required string<uuid>

##### Request Body schema: application/json

required

action

required string

AUTHORIZE_GUEST_ACCESS
timeLimitMinutes integer<int64> [ 1 .. 1000000 ] 

(Optional) how long (in minutes) the guest will be authorized to access the network. If not specified, the default limit is used from the site settings
dataUsageLimitMBytes integer<int64> [ 1 .. 1048576 ] 

(Optional) data usage limit in megabytes
rxRateLimitKbps integer<int64> [ 2 .. 100000 ] 

(Optional) download rate limit in kilobits per second
txRateLimitKbps integer<int64> [ 2 .. 100000 ] 

(Optional) upload rate limit in kilobits per second

### Responses

**200**

OK

post/v1/sites/{siteId}/clients/{clientId}/actions

https://10.103.0.1/proxy/network/integration/v1/sites/{siteId}/clients/{clientId}/actions

### Request samples

*   Payload

Content type

application/json

Example

AUTHORIZE_GUEST_ACCESS 

Copy

`{"action": "AUTHORIZE_GUEST_ACCESS","timeLimitMinutes": 1,"dataUsageLimitMBytes": 1,"rxRateLimitKbps": 2,"txRateLimitKbps": 2}`

### Response samples

*   200

Content type

application/json

Example

AUTHORIZE_GUEST_ACCESS 

Copy

 Expand all  Collapse all 

`{"action": "AUTHORIZE_GUEST_ACCESS","revokedAuthorization": {"authorizedAt": "2019-08-24T14:15:22Z","authorizationMethod": "VOUCHER","expiresAt": "2019-08-24T14:15:22Z","dataUsageLimitMBytes": 1024,"rxRateLimitKbps": 1000,"txRateLimitKbps": 1000,"usage": {"durationSec": 0,"rxBytes": 0,"txBytes": 0,"bytes": 0}},"grantedAuthorization": {"authorizedAt": "2019-08-24T14:15:22Z","authorizationMethod": "VOUCHER","expiresAt": "2019-08-24T14:15:22Z","dataUsageLimitMBytes": 1024,"rxRateLimitKbps": 1000,"txRateLimitKbps": 1000,"usage": {"durationSec": 0,"rxBytes": 0,"txBytes": 0,"bytes": 0}}}`

## [](#tag/Clients/operation/getConnectedClientOverviewPage)List Connected Clients

Retrieve a paginated list of all connected clients on a site, including physical devices (computers, smartphones) and active VPN connections.

Filterable properties (click to expand)
| Name | Type | Allowed functions |
| --- | --- | --- |
| `id` | `UUID` | `eq``ne``in``notIn` |
| `type` | `STRING` | `eq``ne``in``notIn` |
| `macAddress` | `STRING` | `isNull``isNotNull``eq``ne``in``notIn` |
| `ipAddress` | `STRING` | `isNull``isNotNull``eq``ne``in``notIn` |
| `connectedAt` | `TIMESTAMP` | `isNull``isNotNull``eq``ne``gt``ge``lt``le` |
| `access.type` | `STRING` | `eq``ne``in``notIn` |
| `access.authorized` | `BOOLEAN` | `isNull``isNotNull``eq``ne` |

##### path Parameters

siteId

required string<uuid>

##### query Parameters

offset integer<int32>>= 0 

 Default: 0
limit integer<int32> [ 0 .. 200 ] 

 Default: 25
filter string

### Responses

**200**

OK

get/v1/sites/{siteId}/clients

https://10.103.0.1/proxy/network/integration/v1/sites/{siteId}/clients

### Response samples

*   200

Content type

application/json

Copy

 Expand all  Collapse all 

`{"offset": 0,"limit": 25,"count": 10,"totalCount": 1000,"data": [{"type": "string","id": "497f6eca-6276-4993-bfeb-53cbbbba6f08","name": "string","connectedAt": "2019-08-24T14:15:22Z","ipAddress": "string","access": {"type": "DEFAULT"}}]}`

## [](#tag/Clients/operation/getConnectedClientDetails)Get Connected Client Details

Retrieve detailed information about a specific connected client, including name, IP address, MAC address, connection type and access information.

##### path Parameters

clientId

required string<uuid>
siteId

required string<uuid>

### Responses

**200**

OK

get/v1/sites/{siteId}/clients/{clientId}

https://10.103.0.1/proxy/network/integration/v1/sites/{siteId}/clients/{clientId}

### Response samples

*   200

Content type

application/json

Example

WIRED 

Copy

 Expand all  Collapse all 

`{"type": "WIRED","id": "497f6eca-6276-4993-bfeb-53cbbbba6f08","name": "string","connectedAt": "2019-08-24T14:15:22Z","ipAddress": "string","access": {"type": "string"},"macAddress": "string","uplinkDeviceId": "c2692e57-1e51-4519-bb90-c2bdad5882ca"}`

## [](#tag/Networks)Networks

Endpoints for creating, updating, deleting, and inspecting network configurations including VLANs, DHCP, NAT, and IPv4/IPv6 settings.

## [](#tag/Networks/operation/getNetworkDetails)Get Network Details

Retrieve detailed information about a specific network.

##### path Parameters

networkId

required string<uuid>
siteId

required string<uuid>

### Responses

**200**

OK

get/v1/sites/{siteId}/networks/{networkId}

https://10.103.0.1/proxy/network/integration/v1/sites/{siteId}/networks/{networkId}

### Response samples

*   200

Content type

application/json

Example

UNMANAGED 

Copy

 Expand all  Collapse all 

`{"management": "UNMANAGED","id": "497f6eca-6276-4993-bfeb-53cbbbba6f08","name": "Default Network","enabled": true,"vlanId": 1,"metadata": {"origin": "string"},"dhcpGuarding": {"trustedDhcpServerIpAddresses": ["string"]},"default": true}`

## [](#tag/Networks/operation/updateNetwork)Update Network

Update an existing network on a site.

##### path Parameters

networkId

required string<uuid>
siteId

required string<uuid>

##### Request Body schema: application/json

required

management

required string

UNMANAGED
name

required string
enabled

required boolean
vlanId

required integer<int32> [ 1 .. 4009 ] 

VLAN ID. Must be 1 for the default network and >= 2 for additional networks.
dhcpGuarding object (Network DHCP Guarding) 

DHCP Guarding settings for this Network. If this field is omitted or null, the feature is disabled

### Responses

**200**

OK

put/v1/sites/{siteId}/networks/{networkId}

https://10.103.0.1/proxy/network/integration/v1/sites/{siteId}/networks/{networkId}

### Request samples

*   Payload

Content type

application/json

Example

UNMANAGED 

Copy

 Expand all  Collapse all 

`{"management": "UNMANAGED","name": "Default Network","enabled": true,"vlanId": 1,"dhcpGuarding": {"trustedDhcpServerIpAddresses": ["string"]}}`

### Response samples

*   200

Content type

application/json

Example

UNMANAGED 

Copy

 Expand all  Collapse all 

`{"management": "UNMANAGED","id": "497f6eca-6276-4993-bfeb-53cbbbba6f08","name": "Default Network","enabled": true,"vlanId": 1,"metadata": {"origin": "string"},"dhcpGuarding": {"trustedDhcpServerIpAddresses": ["string"]},"default": true}`

## [](#tag/Networks/operation/deleteNetwork)Delete Network

Delete an existing network on a site.

##### path Parameters

networkId

required string<uuid>
siteId

required string<uuid>

##### query Parameters

force boolean

 Default: false

### Responses

**200**

OK

delete/v1/sites/{siteId}/networks/{networkId}

https://10.103.0.1/proxy/network/integration/v1/sites/{siteId}/networks/{networkId}

## [](#tag/Networks/operation/getNetworksOverviewPage)List Networks

Retrieve a paginated list of all Networks on a site.

Filterable properties (click to expand)
| Name | Type | Allowed functions |
| --- | --- | --- |
| `management` | `STRING` | `eq``ne``in``notIn` |
| `id` | `UUID` | `eq``ne``in``notIn` |
| `name` | `STRING` | `eq``ne``in``notIn``like` |
| `enabled` | `BOOLEAN` | `eq``ne` |
| `vlanId` | `INTEGER` | `eq``ne``gt``ge``lt``le``in``notIn` |
| `deviceId` | `UUID` | `eq``ne``in``notIn``isNull``isNotNull` |
| `metadata.origin` | `STRING` | `eq``ne``in``notIn` |

##### path Parameters

siteId

required string<uuid>

##### query Parameters

offset integer<int32>>= 0 

 Default: 0
limit integer<int32> [ 0 .. 200 ] 

 Default: 25
filter string

### Responses

**200**

OK

get/v1/sites/{siteId}/networks

https://10.103.0.1/proxy/network/integration/v1/sites/{siteId}/networks

### Response samples

*   200

Content type

application/json

Copy

 Expand all  Collapse all 

`{"offset": 0,"limit": 25,"count": 10,"totalCount": 1000,"data": [{"management": "string","id": "497f6eca-6276-4993-bfeb-53cbbbba6f08","name": "Default Network","enabled": true,"vlanId": 1,"metadata": {"origin": "string"},"default": true}]}`

## [](#tag/Networks/operation/createNetwork)Create Network

Create a new network on a site.

##### path Parameters

siteId

required string<uuid>

##### Request Body schema: application/json

required

management

required string

UNMANAGED
name

required string
enabled

required boolean
vlanId

required integer<int32> [ 1 .. 4009 ] 

VLAN ID. Must be 1 for the default network and >= 2 for additional networks.
dhcpGuarding object (Network DHCP Guarding) 

DHCP Guarding settings for this Network. If this field is omitted or null, the feature is disabled

### Responses

**201**

Created

post/v1/sites/{siteId}/networks

https://10.103.0.1/proxy/network/integration/v1/sites/{siteId}/networks

### Request samples

*   Payload

Content type

application/json

Example

UNMANAGED 

Copy

 Expand all  Collapse all 

`{"management": "UNMANAGED","name": "Default Network","enabled": true,"vlanId": 1,"dhcpGuarding": {"trustedDhcpServerIpAddresses": ["string"]}}`

### Response samples

*   201

Content type

application/json

Example

UNMANAGED 

Copy

 Expand all  Collapse all 

`{"management": "UNMANAGED","id": "497f6eca-6276-4993-bfeb-53cbbbba6f08","name": "Default Network","enabled": true,"vlanId": 1,"metadata": {"origin": "string"},"dhcpGuarding": {"trustedDhcpServerIpAddresses": ["string"]},"default": true}`

## [](#tag/Networks/operation/getNetworkReferences)Get Network References

Retrieve references to a specific network.

##### path Parameters

networkId

required string<uuid>
siteId

required string<uuid>

### Responses

**200**

OK

get/v1/sites/{siteId}/networks/{networkId}/references

https://10.103.0.1/proxy/network/integration/v1/sites/{siteId}/networks/{networkId}/references

### Response samples

*   200

Content type

application/json

Copy

 Expand all  Collapse all 

`{"referenceResources": [{"resourceType": "CLIENT","referenceCount": 1,"references": [{"referenceId": "8502eb05-558d-4480-8511-c1011710b340"}]}]}`

## [](#tag/WiFi-Broadcasts)WiFi Broadcasts

Endpoints to create, update, or remove WiFi networks (SSIDs). Supports configuration of security, band steering, multicast filtering, and captive portals.

## [](#tag/WiFi-Broadcasts/operation/getWifiBroadcastDetails)Get Wifi Broadcast Details

Retrieve detailed information about a specific Wifi.

##### path Parameters

wifiBroadcastId

required string<uuid>
siteId

required string<uuid>

### Responses

**200**

OK

get/v1/sites/{siteId}/wifi/broadcasts/{wifiBroadcastId}

https://10.103.0.1/proxy/network/integration/v1/sites/{siteId}/wifi/broadcasts/{wifiBroadcastId}

### Response samples

*   200

Content type

application/json

Example

STANDARD 

Copy

 Expand all  Collapse all 

`{"type": "STANDARD","id": "497f6eca-6276-4993-bfeb-53cbbbba6f08","name": "string","metadata": {"origin": "string"},"enabled": true,"network": {"type": "string"},"securityConfiguration": {"type": "string","radiusConfiguration": null},"broadcastingDeviceFilter": {"type": "string"},"mdnsProxyConfiguration": {"mode": "string"},"multicastFilteringPolicy": {"action": "string"},"multicastToUnicastConversionEnabled": true,"clientIsolationEnabled": true,"hideName": true,"uapsdEnabled": true,"basicDataRateKbpsByFrequencyGHz": {"5": 6000,"2.4": 2000},"clientFilteringPolicy": {"action": "ALLOW","macAddressFilter": ["string"]},"blackoutScheduleConfiguration": {"days": [{"type": "string","day": "SUN"}]},"broadcastingFrequenciesGHz": [2.4,5],"hotspotConfiguration": {"type": "string"},"mloEnabled": true,"bandSteeringEnabled": true,"arpProxyEnabled": true,"bssTransitionEnabled": true,"advertiseDeviceName": true,"dtimPeriodByFrequencyGHzOverride": {"5": 1,"6": 1,"2.4": 1}}`

## [](#tag/WiFi-Broadcasts/operation/updateWifiBroadcast)Update Wifi Broadcast

Update an existing Wifi Broadcast on the specified site.

##### path Parameters

wifiBroadcastId

required string<uuid>
siteId

required string<uuid>

##### Request Body schema: application/json

required

type

required string

STANDARD
name

required string
network object (Wifi network reference)
enabled

required boolean
securityConfiguration

required object (Wifi security configuration detailObject)
broadcastingDeviceFilter object (Broadcasting device filter) 

Defines the custom scope of devices that will broadcast this WiFi network. If null, the WiFi network will be broadcast by all Access Point capable devices.
mdnsProxyConfiguration object (mDNS filtering configuration)
multicastFilteringPolicy object (Multicast filtering policy)
multicastToUnicastConversionEnabled

required boolean
clientIsolationEnabled

required boolean
hideName

required boolean
uapsdEnabled

required boolean

Indicates whether Unscheduled Automatic Power Save Delivery (U-APSD) is enabled
basicDataRateKbpsByFrequencyGHz object (IntegrationWifiBasicDataRateConfigurationDto)
clientFilteringPolicy object (IntegrationWifiClientFilteringPolicyDto) 

Client connection filtering policy. Allow/restrict access to the WiFi network based on client device MAC addresses.
blackoutScheduleConfiguration object (Integration blackout schedule configuration)
broadcastingFrequenciesGHz

required Array of numbers [ 1 .. 2147483647 ] items  unique 

Items Enum:2.4 5 6
hotspotConfiguration object (Wifi hotspot configuration)
mloEnabled boolean
bandSteeringEnabled boolean
arpProxyEnabled

required boolean
bssTransitionEnabled

required boolean
advertiseDeviceName

required boolean

Indicates whether the device name is advertised in beacon frames.
dtimPeriodByFrequencyGHzOverride object (IntegrationWifiDtimPeriodConfigurationDto)

### Responses

**200**

OK

put/v1/sites/{siteId}/wifi/broadcasts/{wifiBroadcastId}

https://10.103.0.1/proxy/network/integration/v1/sites/{siteId}/wifi/broadcasts/{wifiBroadcastId}

### Request samples

*   Payload

Content type

application/json

Example

STANDARD 

Copy

 Expand all  Collapse all 

`{"type": "STANDARD","name": "string","network": {"type": "string"},"enabled": true,"securityConfiguration": {"type": "string","radiusConfiguration": null},"broadcastingDeviceFilter": {"type": "string"},"mdnsProxyConfiguration": {"mode": "string"},"multicastFilteringPolicy": {"action": "string"},"multicastToUnicastConversionEnabled": true,"clientIsolationEnabled": true,"hideName": true,"uapsdEnabled": true,"basicDataRateKbpsByFrequencyGHz": {"5": 6000,"2.4": 2000},"clientFilteringPolicy": {"action": "ALLOW","macAddressFilter": ["string"]},"blackoutScheduleConfiguration": {"days": [{"type": "string","day": "SUN"}]},"broadcastingFrequenciesGHz": [2.4,5],"hotspotConfiguration": {"type": "string"},"mloEnabled": true,"bandSteeringEnabled": true,"arpProxyEnabled": true,"bssTransitionEnabled": true,"advertiseDeviceName": true,"dtimPeriodByFrequencyGHzOverride": {"5": 1,"6": 1,"2.4": 1}}`

### Response samples

*   200

Content type

application/json

Example

STANDARD 

Copy

 Expand all  Collapse all 

`{"type": "STANDARD","id": "497f6eca-6276-4993-bfeb-53cbbbba6f08","name": "string","metadata": {"origin": "string"},"enabled": true,"network": {"type": "string"},"securityConfiguration": {"type": "string","radiusConfiguration": null},"broadcastingDeviceFilter": {"type": "string"},"mdnsProxyConfiguration": {"mode": "string"},"multicastFilteringPolicy": {"action": "string"},"multicastToUnicastConversionEnabled": true,"clientIsolationEnabled": true,"hideName": true,"uapsdEnabled": true,"basicDataRateKbpsByFrequencyGHz": {"5": 6000,"2.4": 2000},"clientFilteringPolicy": {"action": "ALLOW","macAddressFilter": ["string"]},"blackoutScheduleConfiguration": {"days": [{"type": "string","day": "SUN"}]},"broadcastingFrequenciesGHz": [2.4,5],"hotspotConfiguration": {"type": "string"},"mloEnabled": true,"bandSteeringEnabled": true,"arpProxyEnabled": true,"bssTransitionEnabled": true,"advertiseDeviceName": true,"dtimPeriodByFrequencyGHzOverride": {"5": 1,"6": 1,"2.4": 1}}`

## [](#tag/WiFi-Broadcasts/operation/deleteWifiBroadcast)Delete Wifi Broadcast

Delete an existing Wifi Broadcast from the specified site.

##### path Parameters

wifiBroadcastId

required string<uuid>
siteId

required string<uuid>

##### query Parameters

force boolean

 Default: false

### Responses

**200**

OK

delete/v1/sites/{siteId}/wifi/broadcasts/{wifiBroadcastId}

https://10.103.0.1/proxy/network/integration/v1/sites/{siteId}/wifi/broadcasts/{wifiBroadcastId}

## [](#tag/WiFi-Broadcasts/operation/getWifiBroadcastPage)List Wifi Broadcasts

Retrieve a paginated list of all Wifi Broadcasts on a site.

Filterable properties (click to expand)
| Name | Type | Allowed functions |
| --- | --- | --- |
| `type` | `STRING` | `eq``ne``in``notIn` |
| `id` | `UUID` | `eq``ne``in``notIn` |
| `enabled` | `BOOLEAN` | `eq``ne` |
| `name` | `STRING` | `eq``ne``in``notIn``like` |
| `broadcastingFrequenciesGHz` | `SET(DECIMAL)` | `contains``containsAny``containsAll``containsExactly` |
| `metadata.origin` | `STRING` | `eq``ne``in``notIn` |
| `network.type` | `STRING` | `eq``ne``in``notIn``isNull``isNotNull` |
| `network.networkId` | `UUID` | `eq``ne``in``notIn` |
| `securityConfiguration.type` | `STRING` | `eq``ne``in``notIn` |
| `broadcastingDeviceFilter.type` | `STRING` | `eq``ne``in``notIn``isNull``isNotNull` |
| `broadcastingDeviceFilter.deviceIds` | `SET(UUID)` | `contains``containsAny``containsAll``containsExactly` |
| `broadcastingDeviceFilter.deviceTagIds` | `SET(UUID)` | `contains``containsAny``containsAll``containsExactly` |
| `hotspotConfiguration.type` | `STRING` | `eq``ne``in``notIn` |

##### path Parameters

siteId

required string<uuid>

##### query Parameters

offset integer<int32>>= 0 

 Default: 0
limit integer<int32> [ 0 .. 200 ] 

 Default: 25
filter string

### Responses

**200**

OK

get/v1/sites/{siteId}/wifi/broadcasts

https://10.103.0.1/proxy/network/integration/v1/sites/{siteId}/wifi/broadcasts

### Response samples

*   200

Content type

application/json

Copy

 Expand all  Collapse all 

`{"offset": 0,"limit": 25,"count": 10,"totalCount": 1000,"data": [{"type": "string","id": "497f6eca-6276-4993-bfeb-53cbbbba6f08","name": "string","enabled": true,"metadata": {"origin": "string"},"network": {"type": "string"},"securityConfiguration": {"type": "string"},"broadcastingDeviceFilter": {"type": "string"}}]}`

## [](#tag/WiFi-Broadcasts/operation/createWifiBroadcast)Create Wifi Broadcast

Create a new Wifi Broadcast on the specified site.

##### path Parameters

siteId

required string<uuid>

##### Request Body schema: application/json

required

type

required string

STANDARD
name

required string
network object (Wifi network reference)
enabled

required boolean
securityConfiguration

required object (Wifi security configuration detailObject)
broadcastingDeviceFilter object (Broadcasting device filter) 

Defines the custom scope of devices that will broadcast this WiFi network. If null, the WiFi network will be broadcast by all Access Point capable devices.
mdnsProxyConfiguration object (mDNS filtering configuration)
multicastFilteringPolicy object (Multicast filtering policy)
multicastToUnicastConversionEnabled

required boolean
clientIsolationEnabled

required boolean
hideName

required boolean
uapsdEnabled

required boolean

Indicates whether Unscheduled Automatic Power Save Delivery (U-APSD) is enabled
basicDataRateKbpsByFrequencyGHz object (IntegrationWifiBasicDataRateConfigurationDto)
clientFilteringPolicy object (IntegrationWifiClientFilteringPolicyDto) 

Client connection filtering policy. Allow/restrict access to the WiFi network based on client device MAC addresses.
blackoutScheduleConfiguration object (Integration blackout schedule configuration)
broadcastingFrequenciesGHz

required Array of numbers [ 1 .. 2147483647 ] items  unique 

Items Enum:2.4 5 6
hotspotConfiguration object (Wifi hotspot configuration)
mloEnabled boolean
bandSteeringEnabled boolean
arpProxyEnabled

required boolean
bssTransitionEnabled

required boolean
advertiseDeviceName

required boolean

Indicates whether the device name is advertised in beacon frames.
dtimPeriodByFrequencyGHzOverride object (IntegrationWifiDtimPeriodConfigurationDto)

### Responses

**201**

Created

post/v1/sites/{siteId}/wifi/broadcasts

https://10.103.0.1/proxy/network/integration/v1/sites/{siteId}/wifi/broadcasts

### Request samples

*   Payload

Content type

application/json

Example

STANDARD 

Copy

 Expand all  Collapse all 

`{"type": "STANDARD","name": "string","network": {"type": "string"},"enabled": true,"securityConfiguration": {"type": "string","radiusConfiguration": null},"broadcastingDeviceFilter": {"type": "string"},"mdnsProxyConfiguration": {"mode": "string"},"multicastFilteringPolicy": {"action": "string"},"multicastToUnicastConversionEnabled": true,"clientIsolationEnabled": true,"hideName": true,"uapsdEnabled": true,"basicDataRateKbpsByFrequencyGHz": {"5": 6000,"2.4": 2000},"clientFilteringPolicy": {"action": "ALLOW","macAddressFilter": ["string"]},"blackoutScheduleConfiguration": {"days": [{"type": "string","day": "SUN"}]},"broadcastingFrequenciesGHz": [2.4,5],"hotspotConfiguration": {"type": "string"},"mloEnabled": true,"bandSteeringEnabled": true,"arpProxyEnabled": true,"bssTransitionEnabled": true,"advertiseDeviceName": true,"dtimPeriodByFrequencyGHzOverride": {"5": 1,"6": 1,"2.4": 1}}`

### Response samples

*   201

Content type

application/json

Example

STANDARD 

Copy

 Expand all  Collapse all 

`{"type": "STANDARD","id": "497f6eca-6276-4993-bfeb-53cbbbba6f08","name": "string","metadata": {"origin": "string"},"enabled": true,"network": {"type": "string"},"securityConfiguration": {"type": "string","radiusConfiguration": null},"broadcastingDeviceFilter": {"type": "string"},"mdnsProxyConfiguration": {"mode": "string"},"multicastFilteringPolicy": {"action": "string"},"multicastToUnicastConversionEnabled": true,"clientIsolationEnabled": true,"hideName": true,"uapsdEnabled": true,"basicDataRateKbpsByFrequencyGHz": {"5": 6000,"2.4": 2000},"clientFilteringPolicy": {"action": "ALLOW","macAddressFilter": ["string"]},"blackoutScheduleConfiguration": {"days": [{"type": "string","day": "SUN"}]},"broadcastingFrequenciesGHz": [2.4,5],"hotspotConfiguration": {"type": "string"},"mloEnabled": true,"bandSteeringEnabled": true,"arpProxyEnabled": true,"bssTransitionEnabled": true,"advertiseDeviceName": true,"dtimPeriodByFrequencyGHzOverride": {"5": 1,"6": 1,"2.4": 1}}`

## [](#tag/Hotspot)Hotspot

Endpoints for managing guest access via Hotspot vouchers — create, list, or revoke vouchers and track their usage and expiration.

## [](#tag/Hotspot/operation/getVouchers)List Vouchers

Retrieve a paginated list of Hotspot vouchers.

Filterable properties (click to expand)
| Name | Type | Allowed functions |
| --- | --- | --- |
| `id` | `UUID` | `eq``ne``in``notIn` |
| `createdAt` | `TIMESTAMP` | `eq``ne``gt``ge``lt``le` |
| `name` | `STRING` | `eq``ne``in``notIn``like` |
| `code` | `STRING` | `eq``ne``in``notIn` |
| `authorizedGuestLimit` | `INTEGER` | `isNull``isNotNull``eq``ne``gt``ge``lt``le` |
| `authorizedGuestCount` | `INTEGER` | `eq``ne``gt``ge``lt``le` |
| `activatedAt` | `TIMESTAMP` | `eq``ne``gt``ge``lt``le` |
| `expiresAt` | `TIMESTAMP` | `eq``ne``gt``ge``lt``le` |
| `expired` | `BOOLEAN` | `eq``ne` |
| `timeLimitMinutes` | `INTEGER` | `eq``ne``gt``ge``lt``le` |
| `dataUsageLimitMBytes` | `INTEGER` | `isNull``isNotNull``eq``ne``gt``ge``lt``le` |
| `rxRateLimitKbps` | `INTEGER` | `isNull``isNotNull``eq``ne``gt``ge``lt``le` |
| `txRateLimitKbps` | `INTEGER` | `isNull``isNotNull``eq``ne``gt``ge``lt``le` |

##### path Parameters

siteId

required string<uuid>

##### query Parameters

offset integer<int32>>= 0 

 Default: 0
limit integer<int32> [ 0 .. 1000 ] 

 Default: 100
filter string

### Responses

**200**

OK

get/v1/sites/{siteId}/hotspot/vouchers

https://10.103.0.1/proxy/network/integration/v1/sites/{siteId}/hotspot/vouchers

### Response samples

*   200

Content type

application/json

Copy

 Expand all  Collapse all 

`{"offset": 0,"limit": 25,"count": 10,"totalCount": 1000,"data": [{"id": "497f6eca-6276-4993-bfeb-53cbbbba6f08","createdAt": "2019-08-24T14:15:22Z","name": "hotel-guest","code": 4861409510,"authorizedGuestLimit": 1,"authorizedGuestCount": 0,"activatedAt": "2019-08-24T14:15:22Z","expiresAt": "2019-08-24T14:15:22Z","expired": true,"timeLimitMinutes": 1440,"dataUsageLimitMBytes": 1024,"rxRateLimitKbps": 1000,"txRateLimitKbps": 1000}]}`

## [](#tag/Hotspot/operation/createVouchers)Generate Vouchers

Create one or more Hotspot vouchers.

##### path Parameters

siteId

required string<uuid>

##### Request Body schema: application/json

required

count integer<int32> [ 1 .. 1000 ] 

 Default: 1

Number of vouchers to generate
name

required string non-empty 

Voucher note, duplicated across all generated vouchers
authorizedGuestLimit integer<int64>>= 1 

(Optional) limit for how many different guests can use the same voucher to authorize network access
timeLimitMinutes

required integer<int64> [ 1 .. 1000000 ] 

How long (in minutes) the voucher will provide access to the network since authorization of the first guest. Subsequently connected guests, if allowed, will share the same expiration time.
dataUsageLimitMBytes integer<int64> [ 1 .. 1048576 ] 

(Optional) data usage limit in megabytes
rxRateLimitKbps integer<int64> [ 2 .. 100000 ] 

(Optional) download rate limit in kilobits per second
txRateLimitKbps integer<int64> [ 2 .. 100000 ] 

(Optional) upload rate limit in kilobits per second

### Responses

**201**

Created

post/v1/sites/{siteId}/hotspot/vouchers

https://10.103.0.1/proxy/network/integration/v1/sites/{siteId}/hotspot/vouchers

### Request samples

*   Payload

Content type

application/json

Copy

`{"count": 1,"name": "string","authorizedGuestLimit": 1,"timeLimitMinutes": 1,"dataUsageLimitMBytes": 1,"rxRateLimitKbps": 2,"txRateLimitKbps": 2}`

### Response samples

*   201

Content type

application/json

Copy

 Expand all  Collapse all 

`{"vouchers": [{"id": "497f6eca-6276-4993-bfeb-53cbbbba6f08","createdAt": "2019-08-24T14:15:22Z","name": "hotel-guest","code": 4861409510,"authorizedGuestLimit": 1,"authorizedGuestCount": 0,"activatedAt": "2019-08-24T14:15:22Z","expiresAt": "2019-08-24T14:15:22Z","expired": true,"timeLimitMinutes": 1440,"dataUsageLimitMBytes": 1024,"rxRateLimitKbps": 1000,"txRateLimitKbps": 1000}]}`

## [](#tag/Hotspot/operation/deleteVouchers)Delete Vouchers

Remove Hotspot vouchers based on the specified filter criteria.

Filterable properties (click to expand)
| Name | Type | Allowed functions |
| --- | --- | --- |
| `id` | `UUID` | `eq``ne``in``notIn` |
| `createdAt` | `TIMESTAMP` | `eq``ne``gt``ge``lt``le` |
| `name` | `STRING` | `eq``ne``in``notIn``like` |
| `code` | `STRING` | `eq``ne``in``notIn` |
| `authorizedGuestLimit` | `INTEGER` | `isNull``isNotNull``eq``ne``gt``ge``lt``le` |
| `authorizedGuestCount` | `INTEGER` | `eq``ne``gt``ge``lt``le` |
| `activatedAt` | `TIMESTAMP` | `eq``ne``gt``ge``lt``le` |
| `expiresAt` | `TIMESTAMP` | `eq``ne``gt``ge``lt``le` |
| `expired` | `BOOLEAN` | `eq``ne` |
| `timeLimitMinutes` | `INTEGER` | `eq``ne``gt``ge``lt``le` |
| `dataUsageLimitMBytes` | `INTEGER` | `isNull``isNotNull``eq``ne``gt``ge``lt``le` |
| `rxRateLimitKbps` | `INTEGER` | `isNull``isNotNull``eq``ne``gt``ge``lt``le` |
| `txRateLimitKbps` | `INTEGER` | `isNull``isNotNull``eq``ne``gt``ge``lt``le` |

##### path Parameters

siteId

required string<uuid>

##### query Parameters

filter

required string

### Responses

**200**

OK

delete/v1/sites/{siteId}/hotspot/vouchers

https://10.103.0.1/proxy/network/integration/v1/sites/{siteId}/hotspot/vouchers

### Response samples

*   200

Content type

application/json

Copy

`{"vouchersDeleted": 0}`

## [](#tag/Hotspot/operation/getVoucher)Get Voucher Details

Retrieve details of a specific Hotspot voucher.

##### path Parameters

voucherId

required string<uuid>
siteId

required string<uuid>

### Responses

**200**

OK

get/v1/sites/{siteId}/hotspot/vouchers/{voucherId}

https://10.103.0.1/proxy/network/integration/v1/sites/{siteId}/hotspot/vouchers/{voucherId}

### Response samples

*   200

Content type

application/json

Copy

`{"id": "497f6eca-6276-4993-bfeb-53cbbbba6f08","createdAt": "2019-08-24T14:15:22Z","name": "hotel-guest","code": 4861409510,"authorizedGuestLimit": 1,"authorizedGuestCount": 0,"activatedAt": "2019-08-24T14:15:22Z","expiresAt": "2019-08-24T14:15:22Z","expired": true,"timeLimitMinutes": 1440,"dataUsageLimitMBytes": 1024,"rxRateLimitKbps": 1000,"txRateLimitKbps": 1000}`

## [](#tag/Hotspot/operation/deleteVoucher)Delete Voucher

Remove a specific Hotspot voucher.

##### path Parameters

voucherId

required string<uuid>
siteId

required string<uuid>

### Responses

**200**

OK

delete/v1/sites/{siteId}/hotspot/vouchers/{voucherId}

https://10.103.0.1/proxy/network/integration/v1/sites/{siteId}/hotspot/vouchers/{voucherId}

### Response samples

*   200

Content type

application/json

Copy

`{"vouchersDeleted": 0}`

## [](#tag/Firewall)Firewall

Endpoints for managing custom firewall zones and policies within a site. Define or update network segmentation and security boundaries.

## [](#tag/Firewall/operation/getFirewallZone)Get Firewall Zone

Get a firewall zone on a site.

##### path Parameters

firewallZoneId

required string<uuid>
siteId

required string<uuid>

### Responses

**200**

OK

get/v1/sites/{siteId}/firewall/zones/{firewallZoneId}

https://10.103.0.1/proxy/network/integration/v1/sites/{siteId}/firewall/zones/{firewallZoneId}

### Response samples

*   200

Content type

application/json

Copy

 Expand all  Collapse all 

`{"id": "ffcdb32c-6278-4364-8947-df4f77118df8","name": "Hotspot|My custom zone","networkIds": ["dfb21062-8ea0-4dca-b1d8-1eb3da00e58b"],"metadata": {"origin": "string"}}`

## [](#tag/Firewall/operation/updateFirewallZone)Update Firewall Zone

Update a firewall zone on a site.

##### path Parameters

firewallZoneId

required string<uuid>
siteId

required string<uuid>

##### Request Body schema: application/json

required

name

required string

Name of a firewall zone
networkIds

required Array of strings<uuid>>= 0 items [ items<uuid > ]

List of Network IDs

### Responses

**200**

OK

put/v1/sites/{siteId}/firewall/zones/{firewallZoneId}

https://10.103.0.1/proxy/network/integration/v1/sites/{siteId}/firewall/zones/{firewallZoneId}

### Request samples

*   Payload

Content type

application/json

Copy

 Expand all  Collapse all 

`{"name": "Hotspot|My custom zone","networkIds": ["dfb21062-8ea0-4dca-b1d8-1eb3da00e58b"]}`

### Response samples

*   200

Content type

application/json

Copy

 Expand all  Collapse all 

`{"id": "ffcdb32c-6278-4364-8947-df4f77118df8","name": "Hotspot|My custom zone","networkIds": ["dfb21062-8ea0-4dca-b1d8-1eb3da00e58b"],"metadata": {"origin": "string"}}`

## [](#tag/Firewall/operation/deleteFirewallZone)Delete Custom Firewall Zone

Delete a custom firewall zone from a site.

##### path Parameters

firewallZoneId

required string<uuid>
siteId

required string<uuid>

### Responses

**200**

OK

delete/v1/sites/{siteId}/firewall/zones/{firewallZoneId}

https://10.103.0.1/proxy/network/integration/v1/sites/{siteId}/firewall/zones/{firewallZoneId}

## [](#tag/Firewall/operation/getFirewallPolicy)Get Firewall Policy

Retrieve specific firewall policy.

##### path Parameters

firewallPolicyId

required string<uuid>
siteId

required string<uuid>

### Responses

**200**

OK

get/v1/sites/{siteId}/firewall/policies/{firewallPolicyId}

https://10.103.0.1/proxy/network/integration/v1/sites/{siteId}/firewall/policies/{firewallPolicyId}

### Response samples

*   200

Content type

application/json

Copy

 Expand all  Collapse all 

`{"id": "497f6eca-6276-4993-bfeb-53cbbbba6f08","enabled": true,"name": "My firewall policy","description": "A description for my firewall policy","index": 0,"action": {"type": "string"},"source": {"zoneId": "c3920607-5069-4ac3-ba10-00754e7a8e8b","trafficFilter": {"type": "string"}},"destination": {"zoneId": "c3920607-5069-4ac3-ba10-00754e7a8e8b","trafficFilter": {"type": "string"}},"ipProtocolScope": {"ipVersion": "string"},"connectionStateFilter": ["NEW"],"ipsecFilter": "MATCH_ENCRYPTED","loggingEnabled": true,"schedule": {"mode": "string"},"metadata": {"origin": "string"}}`

## [](#tag/Firewall/operation/updateFirewallPolicy)Update Firewall Policy

Update an existing firewall policy on a site.

##### path Parameters

firewallPolicyId

required string<uuid>
siteId

required string<uuid>

##### Request Body schema: application/json

required

enabled

required boolean
name

required string non-empty
description string
action

required object (Firewall policy action) 

Defines action for matched traffic.
source

required object (Firewall policy source)
destination

required object (Firewall policy destination)
ipProtocolScope

required object (Firewall policy IP protocol scope) 

Defines rules for matching by IP version and protocol.
connectionStateFilter Array of strings [ 1 .. 2147483647 ] items  unique 

Items Enum:"NEW""INVALID""ESTABLISHED""RELATED"

Match on firewall connection state. If null, matches all connection states.
ipsecFilter string

 Enum:"MATCH_ENCRYPTED""MATCH_NOT_ENCRYPTED"

Match on traffic encrypted, or not encrypted by IPsec. If null, matches all traffic.
loggingEnabled

required boolean

Generate syslog entries when traffic is matched. Such entries are sent to a remote syslog server.
schedule object (Firewall schedule) 

Defines date and time when the entity is active. If null, the entity is always active.

### Responses

**200**

OK

put/v1/sites/{siteId}/firewall/policies/{firewallPolicyId}

https://10.103.0.1/proxy/network/integration/v1/sites/{siteId}/firewall/policies/{firewallPolicyId}

### Request samples

*   Payload

Content type

application/json

Copy

 Expand all  Collapse all 

`{"enabled": true,"name": "My firewall policy","description": "A description for my firewall policy","action": {"type": "string"},"source": {"zoneId": "c3920607-5069-4ac3-ba10-00754e7a8e8b","trafficFilter": {"type": "string"}},"destination": {"zoneId": "c3920607-5069-4ac3-ba10-00754e7a8e8b","trafficFilter": {"type": "string"}},"ipProtocolScope": {"ipVersion": "string"},"connectionStateFilter": ["NEW"],"ipsecFilter": "MATCH_ENCRYPTED","loggingEnabled": true,"schedule": {"mode": "string"}}`

### Response samples

*   200

Content type

application/json

Copy

 Expand all  Collapse all 

`{"id": "497f6eca-6276-4993-bfeb-53cbbbba6f08","enabled": true,"name": "My firewall policy","description": "A description for my firewall policy","index": 0,"action": {"type": "string"},"source": {"zoneId": "c3920607-5069-4ac3-ba10-00754e7a8e8b","trafficFilter": {"type": "string"}},"destination": {"zoneId": "c3920607-5069-4ac3-ba10-00754e7a8e8b","trafficFilter": {"type": "string"}},"ipProtocolScope": {"ipVersion": "string"},"connectionStateFilter": ["NEW"],"ipsecFilter": "MATCH_ENCRYPTED","loggingEnabled": true,"schedule": {"mode": "string"},"metadata": {"origin": "string"}}`

## [](#tag/Firewall/operation/deleteFirewallPolicy)Delete Firewall Policy

Delete an existing firewall policy on a site.

##### path Parameters

firewallPolicyId

required string<uuid>
siteId

required string<uuid>

### Responses

**200**

OK

delete/v1/sites/{siteId}/firewall/policies/{firewallPolicyId}

https://10.103.0.1/proxy/network/integration/v1/sites/{siteId}/firewall/policies/{firewallPolicyId}

## [](#tag/Firewall/operation/patchFirewallPolicy)Patch Firewall Policy

Patch an existing firewall policy on a site.

##### path Parameters

firewallPolicyId

required string<uuid>
siteId

required string<uuid>

##### Request Body schema: application/json

required

loggingEnabled boolean

### Responses

**200**

OK

patch/v1/sites/{siteId}/firewall/policies/{firewallPolicyId}

https://10.103.0.1/proxy/network/integration/v1/sites/{siteId}/firewall/policies/{firewallPolicyId}

### Request samples

*   Payload

Content type

application/json

Copy

`{"loggingEnabled": true}`

### Response samples

*   200

Content type

application/json

Copy

 Expand all  Collapse all 

`{"id": "497f6eca-6276-4993-bfeb-53cbbbba6f08","enabled": true,"name": "My firewall policy","description": "A description for my firewall policy","index": 0,"action": {"type": "string"},"source": {"zoneId": "c3920607-5069-4ac3-ba10-00754e7a8e8b","trafficFilter": {"type": "string"}},"destination": {"zoneId": "c3920607-5069-4ac3-ba10-00754e7a8e8b","trafficFilter": {"type": "string"}},"ipProtocolScope": {"ipVersion": "string"},"connectionStateFilter": ["NEW"],"ipsecFilter": "MATCH_ENCRYPTED","loggingEnabled": true,"schedule": {"mode": "string"},"metadata": {"origin": "string"}}`

## [](#tag/Firewall/operation/getFirewallPolicyOrdering)Get User-Defined Firewall Policy Ordering

Retrieve user-defined firewall policy ordering for a specific source/destination zone pair.

##### path Parameters

siteId

required string<uuid>

##### query Parameters

sourceFirewallZoneId

required string<uuid>
destinationFirewallZoneId

required string<uuid>

### Responses

**200**

OK

get/v1/sites/{siteId}/firewall/policies/ordering

https://10.103.0.1/proxy/network/integration/v1/sites/{siteId}/firewall/policies/ordering

### Response samples

*   200

Content type

application/json

Copy

 Expand all  Collapse all 

`{"orderedFirewallPolicyIds": {"beforeSystemDefined": ["497f6eca-6276-4993-bfeb-53cbbbba6f08"],"afterSystemDefined": ["497f6eca-6276-4993-bfeb-53cbbbba6f08"]}}`

## [](#tag/Firewall/operation/updateFirewallPolicyOrdering)Reorder User-Defined Firewall Policies

Reorder user-defined firewall policies for a specific source/destination zone pair.

##### path Parameters

siteId

required string<uuid>

##### query Parameters

sourceFirewallZoneId

required string<uuid>
destinationFirewallZoneId

required string<uuid>

##### Request Body schema: application/json

required

orderedFirewallPolicyIds

required object (Ordered firewall policy IDs)

### Responses

**200**

OK

put/v1/sites/{siteId}/firewall/policies/ordering

https://10.103.0.1/proxy/network/integration/v1/sites/{siteId}/firewall/policies/ordering

### Request samples

*   Payload

Content type

application/json

Copy

 Expand all  Collapse all 

`{"orderedFirewallPolicyIds": {"beforeSystemDefined": ["497f6eca-6276-4993-bfeb-53cbbbba6f08"],"afterSystemDefined": ["497f6eca-6276-4993-bfeb-53cbbbba6f08"]}}`

### Response samples

*   200

Content type

application/json

Copy

 Expand all  Collapse all 

`{"orderedFirewallPolicyIds": {"beforeSystemDefined": ["497f6eca-6276-4993-bfeb-53cbbbba6f08"],"afterSystemDefined": ["497f6eca-6276-4993-bfeb-53cbbbba6f08"]}}`

## [](#tag/Firewall/operation/getFirewallZones)List Firewall Zones

Retrieve a list of all firewall zones on a site.

Filterable properties (click to expand)
| Name | Type | Allowed functions |
| --- | --- | --- |
| `id` | `UUID` | `eq``ne``in``notIn` |
| `name` | `STRING` | `eq``ne``in``notIn``like` |
| `metadata.origin` | `STRING` | `eq``ne``in``notIn` |
| `metadata.configurable` | `BOOLEAN` | `eq``ne``isNull``isNotNull` |

##### path Parameters

siteId

required string<uuid>

##### query Parameters

offset integer<int32>>= 0 

 Default: 0
limit integer<int32> [ 0 .. 200 ] 

 Default: 25
filter string

### Responses

**200**

OK

get/v1/sites/{siteId}/firewall/zones

https://10.103.0.1/proxy/network/integration/v1/sites/{siteId}/firewall/zones

### Response samples

*   200

Content type

application/json

Copy

 Expand all  Collapse all 

`{"offset": 0,"limit": 25,"count": 10,"totalCount": 1000,"data": [{"id": "ffcdb32c-6278-4364-8947-df4f77118df8","name": "Hotspot|My custom zone","networkIds": ["dfb21062-8ea0-4dca-b1d8-1eb3da00e58b"],"metadata": {"origin": "string"}}]}`

## [](#tag/Firewall/operation/createFirewallZone)Create Custom Firewall Zone

Create a new custom firewall zone on a site.

##### path Parameters

siteId

required string<uuid>

##### Request Body schema: application/json

required

name

required string

Name of a firewall zone
networkIds

required Array of strings<uuid>>= 0 items [ items<uuid > ]

List of Network IDs

### Responses

**201**

Created

post/v1/sites/{siteId}/firewall/zones

https://10.103.0.1/proxy/network/integration/v1/sites/{siteId}/firewall/zones

### Request samples

*   Payload

Content type

application/json

Copy

 Expand all  Collapse all 

`{"name": "Hotspot|My custom zone","networkIds": ["dfb21062-8ea0-4dca-b1d8-1eb3da00e58b"]}`

### Response samples

*   201

Content type

application/json

Copy

 Expand all  Collapse all 

`{"id": "ffcdb32c-6278-4364-8947-df4f77118df8","name": "Hotspot|My custom zone","networkIds": ["dfb21062-8ea0-4dca-b1d8-1eb3da00e58b"],"metadata": {"origin": "string"}}`

## [](#tag/Firewall/operation/getFirewallPolicies)List Firewall Policies

Retrieve a list of all firewall policies on a site.

Filterable properties (click to expand)
| Name | Type | Allowed functions |
| --- | --- | --- |
| `id` | `UUID` | `eq``ne``in``notIn` |
| `name` | `STRING` | `eq``ne``in``notIn``like` |
| `source.zoneId` | `UUID` | `eq``ne``in``notIn` |
| `destination.zoneId` | `UUID` | `eq``ne``in``notIn` |
| `metadata.origin` | `STRING` | `eq``ne``in``notIn` |

##### path Parameters

siteId

required string<uuid>

##### query Parameters

offset integer<int32>>= 0 

 Default: 0
limit integer<int32> [ 0 .. 200 ] 

 Default: 25
filter string

### Responses

**200**

OK

get/v1/sites/{siteId}/firewall/policies

https://10.103.0.1/proxy/network/integration/v1/sites/{siteId}/firewall/policies

### Response samples

*   200

Content type

application/json

Copy

 Expand all  Collapse all 

`{"offset": 0,"limit": 25,"count": 10,"totalCount": 1000,"data": [{"id": "497f6eca-6276-4993-bfeb-53cbbbba6f08","enabled": true,"name": "My firewall policy","description": "A description for my firewall policy","index": 0,"action": {"type": "string"},"source": {"zoneId": "c3920607-5069-4ac3-ba10-00754e7a8e8b","trafficFilter": {"type": "string"}},"destination": {"zoneId": "c3920607-5069-4ac3-ba10-00754e7a8e8b","trafficFilter": {"type": "string"}},"ipProtocolScope": {"ipVersion": "string"},"connectionStateFilter": ["NEW"],"ipsecFilter": "MATCH_ENCRYPTED","loggingEnabled": true,"schedule": {"mode": "string"},"metadata": {"origin": "string"}}]}`

## [](#tag/Firewall/operation/createFirewallPolicy)Create Firewall Policy

Create a new firewall policy on a site.

##### path Parameters

siteId

required string<uuid>

##### Request Body schema: application/json

required

enabled

required boolean
name

required string non-empty
description string
action

required object (Firewall policy action) 

Defines action for matched traffic.
source

required object (Firewall policy source)
destination

required object (Firewall policy destination)
ipProtocolScope

required object (Firewall policy IP protocol scope) 

Defines rules for matching by IP version and protocol.
connectionStateFilter Array of strings [ 1 .. 2147483647 ] items  unique 

Items Enum:"NEW""INVALID""ESTABLISHED""RELATED"

Match on firewall connection state. If null, matches all connection states.
ipsecFilter string

 Enum:"MATCH_ENCRYPTED""MATCH_NOT_ENCRYPTED"

Match on traffic encrypted, or not encrypted by IPsec. If null, matches all traffic.
loggingEnabled

required boolean

Generate syslog entries when traffic is matched. Such entries are sent to a remote syslog server.
schedule object (Firewall schedule) 

Defines date and time when the entity is active. If null, the entity is always active.

### Responses

**201**

Created

post/v1/sites/{siteId}/firewall/policies

https://10.103.0.1/proxy/network/integration/v1/sites/{siteId}/firewall/policies

### Request samples

*   Payload

Content type

application/json

Copy

 Expand all  Collapse all 

`{"enabled": true,"name": "My firewall policy","description": "A description for my firewall policy","action": {"type": "string"},"source": {"zoneId": "c3920607-5069-4ac3-ba10-00754e7a8e8b","trafficFilter": {"type": "string"}},"destination": {"zoneId": "c3920607-5069-4ac3-ba10-00754e7a8e8b","trafficFilter": {"type": "string"}},"ipProtocolScope": {"ipVersion": "string"},"connectionStateFilter": ["NEW"],"ipsecFilter": "MATCH_ENCRYPTED","loggingEnabled": true,"schedule": {"mode": "string"}}`

### Response samples

*   201

Content type

application/json

Copy

 Expand all  Collapse all 

`{"id": "497f6eca-6276-4993-bfeb-53cbbbba6f08","enabled": true,"name": "My firewall policy","description": "A description for my firewall policy","index": 0,"action": {"type": "string"},"source": {"zoneId": "c3920607-5069-4ac3-ba10-00754e7a8e8b","trafficFilter": {"type": "string"}},"destination": {"zoneId": "c3920607-5069-4ac3-ba10-00754e7a8e8b","trafficFilter": {"type": "string"}},"ipProtocolScope": {"ipVersion": "string"},"connectionStateFilter": ["NEW"],"ipsecFilter": "MATCH_ENCRYPTED","loggingEnabled": true,"schedule": {"mode": "string"},"metadata": {"origin": "string"}}`

## [](#tag/Access-Control-(ACL-Rules))Access Control (ACL Rules)

Endpoints for creating, listing, and managing ACL (Access Control List) rule that enforce traffic filtering across devices and networks.

## [](#tag/Access-Control-(ACL-Rules)/operation/getAclRule)Get ACL Rule

##### path Parameters

aclRuleId

required string<uuid>
siteId

required string<uuid>

### Responses

**200**

OK

get/v1/sites/{siteId}/acl-rules/{aclRuleId}

https://10.103.0.1/proxy/network/integration/v1/sites/{siteId}/acl-rules/{aclRuleId}

### Response samples

*   200

Content type

application/json

Example

IPV4 

Copy

 Expand all  Collapse all 

`{"type": "IPV4","id": "497f6eca-6276-4993-bfeb-53cbbbba6f08","enabled": true,"name": "string","description": "string","action": "ALLOW|BLOCK","enforcingDeviceFilter": {"type": "string"},"index": 0,"sourceFilter": null,"destinationFilter": null,"metadata": {"origin": "string"},"protocolFilter": ["TCP"]}`

## [](#tag/Access-Control-(ACL-Rules)/operation/updateAclRule)Update ACL Rule

Update an existing user defined ACL rule on a site.

##### path Parameters

aclRuleId

required string<uuid>
siteId

required string<uuid>

##### Request Body schema: application/json

required

type

required string

IPV4
enabled

required boolean
name

required string non-empty 

ACL rule name
description string

ACL rule description
action

required string

 Enum:"ALLOW""BLOCK"

ACL rule action
enforcingDeviceFilter object (ACL rule device filter) 

IDs of the Switch-capable devices used to enforce the ACL rule. When null, the rule will be provisioned to all switches on the site.
index integer<int32>>= 0 

 Deprecated 

ACL rule index. This property is deprecated and has no effect. Use the dedicated ACL rule reordering endpoint.
sourceFilter object

Traffic source filter
destinationFilter object

Traffic destination filter
protocolFilter Array of strings [ 1 .. 2147483647 ] items  unique 

Items Enum:"TCP""UDP"

Protocols this ACL rule will be applied to. When null, the rule will be applied to all protocols.

### Responses

**200**

OK

put/v1/sites/{siteId}/acl-rules/{aclRuleId}

https://10.103.0.1/proxy/network/integration/v1/sites/{siteId}/acl-rules/{aclRuleId}

### Request samples

*   Payload

Content type

application/json

Example

IPV4 

Copy

 Expand all  Collapse all 

`{"type": "IPV4","enabled": true,"name": "string","description": "string","action": "ALLOW|BLOCK","enforcingDeviceFilter": {"type": "string"},"index": 0,"sourceFilter": {"type": "string"},"destinationFilter": {"type": "string"},"protocolFilter": ["TCP"]}`

### Response samples

*   200

Content type

application/json

Example

IPV4 

Copy

 Expand all  Collapse all 

`{"type": "IPV4","id": "497f6eca-6276-4993-bfeb-53cbbbba6f08","enabled": true,"name": "string","description": "string","action": "ALLOW|BLOCK","enforcingDeviceFilter": {"type": "string"},"index": 0,"sourceFilter": null,"destinationFilter": null,"metadata": {"origin": "string"},"protocolFilter": ["TCP"]}`

## [](#tag/Access-Control-(ACL-Rules)/operation/deleteAclRule)Delete ACL Rule

Delete an existing user defined ACL rule on a site.

##### path Parameters

aclRuleId

required string<uuid>
siteId

required string<uuid>

### Responses

**200**

OK

delete/v1/sites/{siteId}/acl-rules/{aclRuleId}

https://10.103.0.1/proxy/network/integration/v1/sites/{siteId}/acl-rules/{aclRuleId}

## [](#tag/Access-Control-(ACL-Rules)/operation/getAclRuleOrdering)Get User-Defined ACL Rule Ordering

Retrieve user-defined ACL rule ordering on a site.

##### path Parameters

siteId

required string<uuid>

### Responses

**200**

OK

get/v1/sites/{siteId}/acl-rules/ordering

https://10.103.0.1/proxy/network/integration/v1/sites/{siteId}/acl-rules/ordering

### Response samples

*   200

Content type

application/json

Copy

 Expand all  Collapse all 

`{"orderedAclRuleIds": ["497f6eca-6276-4993-bfeb-53cbbbba6f08"]}`

## [](#tag/Access-Control-(ACL-Rules)/operation/updateAclRuleOrdering)Reorder User-Defined ACL Rules

Reorder user-defined ACL rules on a site.

##### path Parameters

siteId

required string<uuid>

##### Request Body schema: application/json

required

orderedAclRuleIds

required Array of strings<uuid>[ items<uuid > ]

### Responses

**200**

OK

put/v1/sites/{siteId}/acl-rules/ordering

https://10.103.0.1/proxy/network/integration/v1/sites/{siteId}/acl-rules/ordering

### Request samples

*   Payload

Content type

application/json

Copy

 Expand all  Collapse all 

`{"orderedAclRuleIds": ["497f6eca-6276-4993-bfeb-53cbbbba6f08"]}`

### Response samples

*   200

Content type

application/json

Copy

 Expand all  Collapse all 

`{"orderedAclRuleIds": ["497f6eca-6276-4993-bfeb-53cbbbba6f08"]}`

## [](#tag/Access-Control-(ACL-Rules)/operation/getAclRulePage)List ACL Rules

Retrieve a paginated list of all ACL rules on a site.

Filterable properties (click to expand)
| Name | Type | Allowed functions |
| --- | --- | --- |
| `type` | `STRING` | `eq``ne``in``notIn` |
| `id` | `UUID` | `eq``ne``in``notIn` |
| `enabled` | `BOOLEAN` | `eq``ne` |
| `name` | `STRING` | `eq``ne``in``notIn``like` |
| `description` | `STRING` | `isNull``isNotNull``eq``ne``in``notIn``like` |
| `action` | `STRING` | `eq``ne``in``notIn` |
| `index` | `INTEGER` | `eq``ne``gt``ge``lt``le``in``notIn` |
| `protocolFilter` | `SET(STRING)` | `isNull``isNotNull``contains``containsAny``containsAll``containsExactly` |
| `networkId` | `UUID` | `isNull``isNotNull``eq``ne``in``notIn` |
| `enforcingDeviceFilter.deviceIds` | `SET(UUID)` | `isNull``isNotNull``contains``containsAny``containsAll``containsExactly` |
| `metadata.origin` | `STRING` | `eq``ne``in``notIn` |
| `sourceFilter.type` | `STRING` | `isNull``isNotNull``eq``ne``in``notIn` |
| `sourceFilter.ipAddressesOrSubnets` | `SET(STRING)` | `contains``containsAny``containsAll``containsExactly` |
| `sourceFilter.portFilter` | `SET(INTEGER)` | `isNull``isNotNull``contains``containsAny``containsAll``containsExactly` |
| `sourceFilter.networkIds` | `SET(UUID)` | `contains``containsAny``containsAll``containsExactly` |
| `sourceFilter.macAddresses` | `SET(STRING)` | `contains``containsAny``containsAll``containsExactly` |
| `sourceFilter.prefixLength` | `INTEGER` | `isNull``isNotNull``eq``ne``gt``ge``lt``le``in``notIn` |
| `destinationFilter.type` | `STRING` | `isNull``isNotNull``eq``ne``in``notIn` |
| `destinationFilter.ipAddressesOrSubnets` | `SET(STRING)` | `contains``containsAny``containsAll``containsExactly` |
| `destinationFilter.portFilter` | `SET(INTEGER)` | `isNull``isNotNull``contains``containsAny``containsAll``containsExactly` |
| `destinationFilter.networkIds` | `SET(UUID)` | `contains``containsAny``containsAll``containsExactly` |
| `destinationFilter.macAddresses` | `SET(STRING)` | `contains``containsAny``containsAll``containsExactly` |
| `destinationFilter.prefixLength` | `INTEGER` | `isNull``isNotNull``eq``ne``gt``ge``lt``le``in``notIn` |

##### path Parameters

siteId

required string<uuid>

##### query Parameters

offset integer<int32>>= 0 

 Default: 0
limit integer<int32> [ 0 .. 200 ] 

 Default: 25
filter string

### Responses

**200**

OK

get/v1/sites/{siteId}/acl-rules

https://10.103.0.1/proxy/network/integration/v1/sites/{siteId}/acl-rules

### Response samples

*   200

Content type

application/json

Copy

 Expand all  Collapse all 

`{"offset": 0,"limit": 25,"count": 10,"totalCount": 1000,"data": [{"type": "string","id": "497f6eca-6276-4993-bfeb-53cbbbba6f08","enabled": true,"name": "string","description": "string","action": "ALLOW|BLOCK","enforcingDeviceFilter": {"type": "string"},"index": 0,"sourceFilter": null,"destinationFilter": null,"metadata": {"origin": "string"}}]}`

## [](#tag/Access-Control-(ACL-Rules)/operation/createAclRule)Create ACL Rule

Create a new user defined ACL rule on a site.

##### path Parameters

siteId

required string<uuid>

##### Request Body schema: application/json

required

type

required string

IPV4
enabled

required boolean
name

required string non-empty 

ACL rule name
description string

ACL rule description
action

required string

 Enum:"ALLOW""BLOCK"

ACL rule action
enforcingDeviceFilter object (ACL rule device filter) 

IDs of the Switch-capable devices used to enforce the ACL rule. When null, the rule will be provisioned to all switches on the site.
index integer<int32>>= 0 

 Deprecated 

ACL rule index. This property is deprecated and has no effect. Use the dedicated ACL rule reordering endpoint.
sourceFilter object

Traffic source filter
destinationFilter object

Traffic destination filter
protocolFilter Array of strings [ 1 .. 2147483647 ] items  unique 

Items Enum:"TCP""UDP"

Protocols this ACL rule will be applied to. When null, the rule will be applied to all protocols.

### Responses

**201**

Created

post/v1/sites/{siteId}/acl-rules

https://10.103.0.1/proxy/network/integration/v1/sites/{siteId}/acl-rules

### Request samples

*   Payload

Content type

application/json

Example

IPV4 

Copy

 Expand all  Collapse all 

`{"type": "IPV4","enabled": true,"name": "string","description": "string","action": "ALLOW|BLOCK","enforcingDeviceFilter": {"type": "string"},"index": 0,"sourceFilter": {"type": "string"},"destinationFilter": {"type": "string"},"protocolFilter": ["TCP"]}`

### Response samples

*   201

Content type

application/json

Example

IPV4 

Copy

 Expand all  Collapse all 

`{"type": "IPV4","id": "497f6eca-6276-4993-bfeb-53cbbbba6f08","enabled": true,"name": "string","description": "string","action": "ALLOW|BLOCK","enforcingDeviceFilter": {"type": "string"},"index": 0,"sourceFilter": null,"destinationFilter": null,"metadata": {"origin": "string"},"protocolFilter": ["TCP"]}`

## [](#tag/Switching)Switching

Endpoints for managing switching features like Switch Stacking and LAG.

## [](#tag/Switching/operation/getSwitchStackPage)List Switch Stacks

Retrieve a paginated list of all Switch Stacks on a site.

Filterable properties (click to expand)
| Name | Type | Allowed functions |
| --- | --- | --- |
| `id` | `UUID` | `eq``ne``in``notIn` |
| `name` | `STRING` | `eq``ne``in``notIn``like` |
| `members.deviceId` | `SET(UUID)` | `contains``containsAny``containsAll``containsExactly` |

##### path Parameters

siteId

required string<uuid>

##### query Parameters

offset integer<int32>>= 0 

 Default: 0
limit integer<int32> [ 0 .. 200 ] 

 Default: 25
filter string

### Responses

**200**

OK

get/v1/sites/{siteId}/switching/switch-stacks

https://10.103.0.1/proxy/network/integration/v1/sites/{siteId}/switching/switch-stacks

### Response samples

*   200

Content type

application/json

Copy

 Expand all  Collapse all 

`{"offset": 0,"limit": 25,"count": 10,"totalCount": 1000,"data": [{"id": "497f6eca-6276-4993-bfeb-53cbbbba6f08","name": "string","members": [{"deviceId": "4de4adb9-21ee-47e3-aeb4-8cf8ed6c109a"},{"deviceId": "4de4adb9-21ee-47e3-aeb4-8cf8ed6c109a"}],"lags": [{"id": "497f6eca-6276-4993-bfeb-53cbbbba6f08","members": [{"deviceId": "4de4adb9-21ee-47e3-aeb4-8cf8ed6c109a","portIdxs": [0]}]}],"metadata": {"origin": "string"}}]}`

## [](#tag/Switching/operation/getSwitchStack)Get Switch Stack

Retrieve Switch Stack details.

##### path Parameters

switchStackId

required string<uuid>
siteId

required string<uuid>

### Responses

**200**

OK

get/v1/sites/{siteId}/switching/switch-stacks/{switchStackId}

https://10.103.0.1/proxy/network/integration/v1/sites/{siteId}/switching/switch-stacks/{switchStackId}

### Response samples

*   200

Content type

application/json

Copy

 Expand all  Collapse all 

`{"id": "497f6eca-6276-4993-bfeb-53cbbbba6f08","name": "string","members": [{"deviceId": "4de4adb9-21ee-47e3-aeb4-8cf8ed6c109a"},{"deviceId": "4de4adb9-21ee-47e3-aeb4-8cf8ed6c109a"}],"lags": [{"id": "497f6eca-6276-4993-bfeb-53cbbbba6f08","members": [{"deviceId": "4de4adb9-21ee-47e3-aeb4-8cf8ed6c109a","portIdxs": [0]}]}],"metadata": {"origin": "string"}}`

## [](#tag/Switching/operation/getMcLagDomainPage)List MC-LAG Domains

Retrieve a paginated list of all MC-LAG Domains on a site.

Filterable properties (click to expand)
| Name | Type | Allowed functions |
| --- | --- | --- |
| `id` | `UUID` | `eq``ne``in``notIn` |
| `name` | `STRING` | `eq``ne``in``notIn``like` |
| `peers.deviceId` | `SET(UUID)` | `contains``containsAny``containsAll``containsExactly` |

##### path Parameters

siteId

required string<uuid>

##### query Parameters

offset integer<int32>>= 0 

 Default: 0
limit integer<int32> [ 0 .. 200 ] 

 Default: 25
filter string

### Responses

**200**

OK

get/v1/sites/{siteId}/switching/mc-lag-domains

https://10.103.0.1/proxy/network/integration/v1/sites/{siteId}/switching/mc-lag-domains

### Response samples

*   200

Content type

application/json

Copy

 Expand all  Collapse all 

`{"offset": 0,"limit": 25,"count": 10,"totalCount": 1000,"data": [{"id": "497f6eca-6276-4993-bfeb-53cbbbba6f08","name": "string","peers": [{"role": "TOP","deviceId": "4de4adb9-21ee-47e3-aeb4-8cf8ed6c109a","linkPortIdxs": [0]}],"lags": [{"id": "497f6eca-6276-4993-bfeb-53cbbbba6f08","members": [{"deviceId": "4de4adb9-21ee-47e3-aeb4-8cf8ed6c109a","portIdxs": [0]}]}],"metadata": {"origin": "string"}}]}`

## [](#tag/Switching/operation/getMcLagDomain)Get MC-LAG Domain

Retrieve MC-LAG Domain details.

##### path Parameters

mcLagDomainId

required string<uuid>
siteId

required string<uuid>

### Responses

**200**

OK

get/v1/sites/{siteId}/switching/mc-lag-domains/{mcLagDomainId}

https://10.103.0.1/proxy/network/integration/v1/sites/{siteId}/switching/mc-lag-domains/{mcLagDomainId}

### Response samples

*   200

Content type

application/json

Copy

 Expand all  Collapse all 

`{"id": "497f6eca-6276-4993-bfeb-53cbbbba6f08","name": "string","peers": [{"role": "TOP","deviceId": "4de4adb9-21ee-47e3-aeb4-8cf8ed6c109a","linkPortIdxs": [0]}],"lags": [{"id": "497f6eca-6276-4993-bfeb-53cbbbba6f08","members": [{"deviceId": "4de4adb9-21ee-47e3-aeb4-8cf8ed6c109a","portIdxs": [0]}]}],"metadata": {"origin": "string"}}`

## [](#tag/Switching/operation/getLagPage)List LAGs

Retrieve a paginated list of all LAGs (Link Aggregation Groups) on a site.

Filterable properties (click to expand)
| Name | Type | Allowed functions |
| --- | --- | --- |
| `id` | `UUID` | `eq``ne``in``notIn` |
| `type` | `STRING` | `eq``ne``in``notIn` |
| `switchStackId` | `UUID` | `eq``ne``in``notIn``isNull``isNotNull` |
| `mcLagDomainId` | `UUID` | `eq``ne``in``notIn``isNull``isNotNull` |
| `members.deviceId` | `SET(UUID)` | `contains``containsAny``containsAll``containsExactly` |
| `members.portIdxs` | `SET(INTEGER)` | `contains``containsAny``containsAll``containsExactly` |

##### path Parameters

siteId

required string<uuid>

##### query Parameters

offset integer<int32>>= 0 

 Default: 0
limit integer<int32> [ 0 .. 200 ] 

 Default: 25
filter string

### Responses

**200**

OK

get/v1/sites/{siteId}/switching/lags

https://10.103.0.1/proxy/network/integration/v1/sites/{siteId}/switching/lags

### Response samples

*   200

Content type

application/json

Copy

 Expand all  Collapse all 

`{"offset": 0,"limit": 25,"count": 10,"totalCount": 1000,"data": [{"type": "string","id": "497f6eca-6276-4993-bfeb-53cbbbba6f08","members": [{"deviceId": "4de4adb9-21ee-47e3-aeb4-8cf8ed6c109a","portIdxs": [0]}]}]}`

## [](#tag/Switching/operation/getLag)Get LAG Details

Retrieve LAG details.

##### path Parameters

lagId

required string<uuid>
siteId

required string<uuid>

### Responses

**200**

OK

get/v1/sites/{siteId}/switching/lags/{lagId}

https://10.103.0.1/proxy/network/integration/v1/sites/{siteId}/switching/lags/{lagId}

### Response samples

*   200

Content type

application/json

Example

SWITCH_STACK 

Copy

 Expand all  Collapse all 

`{"type": "SWITCH_STACK","id": "497f6eca-6276-4993-bfeb-53cbbbba6f08","members": [{"deviceId": "4de4adb9-21ee-47e3-aeb4-8cf8ed6c109a","portIdxs": [0]}],"switchStackId": "bd2c5532-16a4-4f97-91d1-09f6ed6a3b97"}`

## [](#tag/DNS-Policies)DNS Policies

Endpoints for managing DNS Policies within a site.

## [](#tag/DNS-Policies/operation/getDnsPolicy)Get DNS Policy

Retrieve specific DNS policy.

##### path Parameters

dnsPolicyId

required string<uuid>
siteId

required string<uuid>

### Responses

**200**

OK

get/v1/sites/{siteId}/dns/policies/{dnsPolicyId}

https://10.103.0.1/proxy/network/integration/v1/sites/{siteId}/dns/policies/{dnsPolicyId}

### Response samples

*   200

Content type

application/json

Example

A_RECORD 

Copy

 Expand all  Collapse all 

`{"type": "A_RECORD","id": "497f6eca-6276-4993-bfeb-53cbbbba6f08","enabled": true,"metadata": {"origin": "string"},"domain": "example.com","ipv4Address": "192.168.1.10","ttlSeconds": 14400}`

## [](#tag/DNS-Policies/operation/updateDnsPolicy)Update DNS Policy

Update an existing DNS policy on a site.

##### path Parameters

dnsPolicyId

required string<uuid>
siteId

required string<uuid>

##### Request Body schema: application/json

required

type

required string

A_RECORD
enabled

required boolean
domain

required string [ 1 .. 127 ] characters
ipv4Address

required string
ttlSeconds

required integer<int32> [ 0 .. 86400 ] 

Time to live in seconds.

### Responses

**200**

OK

put/v1/sites/{siteId}/dns/policies/{dnsPolicyId}

https://10.103.0.1/proxy/network/integration/v1/sites/{siteId}/dns/policies/{dnsPolicyId}

### Request samples

*   Payload

Content type

application/json

Example

A_RECORD 

Copy

`{"type": "A_RECORD","enabled": true,"domain": "example.com","ipv4Address": "192.168.1.10","ttlSeconds": 14400}`

### Response samples

*   200

Content type

application/json

Example

A_RECORD 

Copy

 Expand all  Collapse all 

`{"type": "A_RECORD","id": "497f6eca-6276-4993-bfeb-53cbbbba6f08","enabled": true,"metadata": {"origin": "string"},"domain": "example.com","ipv4Address": "192.168.1.10","ttlSeconds": 14400}`

## [](#tag/DNS-Policies/operation/deleteDnsPolicy)Delete DNS Policy

Delete an existing DNS policy on a site.

##### path Parameters

dnsPolicyId

required string<uuid>
siteId

required string<uuid>

### Responses

**200**

OK

delete/v1/sites/{siteId}/dns/policies/{dnsPolicyId}

https://10.103.0.1/proxy/network/integration/v1/sites/{siteId}/dns/policies/{dnsPolicyId}

## [](#tag/DNS-Policies/operation/getDnsPolicyPage)List DNS Policies

Retrieve a paginated list of all DNS policies on a site.

Filterable properties (click to expand)
| Name | Type | Allowed functions |
| --- | --- | --- |
| `type` | `STRING` | `eq``ne``in``notIn` |
| `id` | `UUID` | `eq``ne``in``notIn` |
| `enabled` | `BOOLEAN` | `eq``ne` |
| `domain` | `STRING` | `eq``ne``in``notIn``like` |
| `ipv4Address` | `STRING` | `eq``ne``in``notIn``like` |
| `ipv6Address` | `STRING` | `eq``ne``in``notIn``like` |
| `targetDomain` | `STRING` | `eq``ne``in``notIn``like` |
| `mailServerDomain` | `STRING` | `eq``ne``in``notIn``like` |
| `text` | `STRING` | `eq``ne``in``notIn``like` |
| `serverDomain` | `STRING` | `eq``ne``in``notIn``like` |
| `ipAddress` | `STRING` | `eq``ne``in``notIn``like` |
| `ttlSeconds` | `INTEGER` | `eq``ne``gt``ge``lt``le``in``notIn` |
| `priority` | `INTEGER` | `eq``ne``gt``ge``lt``le``in``notIn` |
| `service` | `STRING` | `eq``ne``in``notIn` |
| `protocol` | `STRING` | `eq``ne``in``notIn` |
| `port` | `INTEGER` | `eq``ne``gt``ge``lt``le``in``notIn` |
| `weight` | `INTEGER` | `eq``ne``gt``ge``lt``le``in``notIn` |

##### path Parameters

siteId

required string<uuid>

##### query Parameters

offset integer<int32>>= 0 

 Default: 0
limit integer<int32> [ 0 .. 200 ] 

 Default: 25
filter string

### Responses

**200**

OK

get/v1/sites/{siteId}/dns/policies

https://10.103.0.1/proxy/network/integration/v1/sites/{siteId}/dns/policies

### Response samples

*   200

Content type

application/json

Copy

 Expand all  Collapse all 

`{"offset": 0,"limit": 25,"count": 10,"totalCount": 1000,"data": [{"type": "string","id": "497f6eca-6276-4993-bfeb-53cbbbba6f08","enabled": true,"metadata": {"origin": "string"},"domain": "string"}]}`

## [](#tag/DNS-Policies/operation/createDnsPolicy)Create DNS Policy

Create a new DNS policy on a site.

##### path Parameters

siteId

required string<uuid>

##### Request Body schema: application/json

required

type

required string

A_RECORD
enabled

required boolean
domain

required string [ 1 .. 127 ] characters
ipv4Address

required string
ttlSeconds

required integer<int32> [ 0 .. 86400 ] 

Time to live in seconds.

### Responses

**201**

Created

post/v1/sites/{siteId}/dns/policies

https://10.103.0.1/proxy/network/integration/v1/sites/{siteId}/dns/policies

### Request samples

*   Payload

Content type

application/json

Example

A_RECORD 

Copy

`{"type": "A_RECORD","enabled": true,"domain": "example.com","ipv4Address": "192.168.1.10","ttlSeconds": 14400}`

### Response samples

*   201

Content type

application/json

Example

A_RECORD 

Copy

 Expand all  Collapse all 

`{"type": "A_RECORD","id": "497f6eca-6276-4993-bfeb-53cbbbba6f08","enabled": true,"metadata": {"origin": "string"},"domain": "example.com","ipv4Address": "192.168.1.10","ttlSeconds": 14400}`

## [](#tag/Traffic-Matching-Lists)Traffic Matching Lists

Endpoints for managing port and IP address lists used across firewall policy configurations.

## [](#tag/Traffic-Matching-Lists/operation/getTrafficMatchingList)Get Traffic Matching List

Get an exist traffic matching list on a site.

##### path Parameters

trafficMatchingListId

required string<uuid>
siteId

required string<uuid>

### Responses

**200**

OK

get/v1/sites/{siteId}/traffic-matching-lists/{trafficMatchingListId}

https://10.103.0.1/proxy/network/integration/v1/sites/{siteId}/traffic-matching-lists/{trafficMatchingListId}

### Response samples

*   200

Content type

application/json

Example

PORTS 

Copy

 Expand all  Collapse all 

`{"type": "PORTS","id": "ffcdb32c-6278-4364-8947-df4f77118df8","name": "Allowed port list|Protected IP list","items": [{"type": "string"}]}`

## [](#tag/Traffic-Matching-Lists/operation/updateTrafficMatchingList)Update Traffic Matching List

Update an exist traffic matching list on a site.

##### path Parameters

trafficMatchingListId

required string<uuid>
siteId

required string<uuid>

##### Request Body schema: application/json

required

type

required string

PORTS
name

required string non-empty
items

required Array of objects (Port matching)  non-empty

### Responses

**200**

OK

put/v1/sites/{siteId}/traffic-matching-lists/{trafficMatchingListId}

https://10.103.0.1/proxy/network/integration/v1/sites/{siteId}/traffic-matching-lists/{trafficMatchingListId}

### Request samples

*   Payload

Content type

application/json

Example

PORTS 

Copy

 Expand all  Collapse all 

`{"type": "PORTS","name": "Allowed port list|Protected IP list","items": [{"type": "string"}]}`

### Response samples

*   200

Content type

application/json

Example

PORTS 

Copy

 Expand all  Collapse all 

`{"type": "PORTS","id": "ffcdb32c-6278-4364-8947-df4f77118df8","name": "Allowed port list|Protected IP list","items": [{"type": "string"}]}`

## [](#tag/Traffic-Matching-Lists/operation/deleteTrafficMatchingList)Delete Traffic Matching List

Delete an exist traffic matching list on a site.

##### path Parameters

trafficMatchingListId

required string<uuid>
siteId

required string<uuid>

### Responses

**200**

OK

delete/v1/sites/{siteId}/traffic-matching-lists/{trafficMatchingListId}

https://10.103.0.1/proxy/network/integration/v1/sites/{siteId}/traffic-matching-lists/{trafficMatchingListId}

## [](#tag/Traffic-Matching-Lists/operation/getTrafficMatchingLists)List Traffic Matching Lists

Retrieve all traffic matching lists on a site.

Filterable properties (click to expand)
| Name | Type | Allowed functions |
| --- | --- | --- |
| `id` | `UUID` | `eq``ne``in``notIn` |
| `name` | `STRING` | `eq``ne``in``notIn``like` |

##### path Parameters

siteId

required string<uuid>

##### query Parameters

offset integer<int32>>= 0 

 Default: 0
limit integer<int32> [ 0 .. 200 ] 

 Default: 25
filter string

### Responses

**200**

OK

get/v1/sites/{siteId}/traffic-matching-lists

https://10.103.0.1/proxy/network/integration/v1/sites/{siteId}/traffic-matching-lists

### Response samples

*   200

Content type

application/json

Copy

 Expand all  Collapse all 

`{"offset": 0,"limit": 25,"count": 10,"totalCount": 1000,"data": [{"type": "string","id": "ffcdb32c-6278-4364-8947-df4f77118df8","name": "Allowed port list|Protected IP list"}]}`

## [](#tag/Traffic-Matching-Lists/operation/createTrafficMatchingList)Create Traffic Matching List

Create a new traffic matching list on a site.

##### path Parameters

siteId

required string<uuid>

##### Request Body schema: application/json

required

type

required string

PORTS
name

required string non-empty
items

required Array of objects (Port matching)  non-empty

### Responses

**201**

Created

post/v1/sites/{siteId}/traffic-matching-lists

https://10.103.0.1/proxy/network/integration/v1/sites/{siteId}/traffic-matching-lists

### Request samples

*   Payload

Content type

application/json

Example

PORTS 

Copy

 Expand all  Collapse all 

`{"type": "PORTS","name": "Allowed port list|Protected IP list","items": [{"type": "string"}]}`

### Response samples

*   201

Content type

application/json

Example

PORTS 

Copy

 Expand all  Collapse all 

`{"type": "PORTS","id": "ffcdb32c-6278-4364-8947-df4f77118df8","name": "Allowed port list|Protected IP list","items": [{"type": "string"}]}`

## [](#tag/Supporting-Resources)Supporting Resources

Contains read-only reference endpoints used to retrieve supporting data such as WAN interfaces, DPI categories, country codes, RADIUS profiles, and device tags.

## [](#tag/Supporting-Resources/operation/getWansOverviewPage)List WAN Interfaces

Returns available WAN interface definitions for a given site, including identifiers and names. Useful for network and NAT configuration.

##### path Parameters

siteId

required string<uuid>

##### query Parameters

offset integer<int32>>= 0 

 Default: 0
limit integer<int32> [ 0 .. 200 ] 

 Default: 25

### Responses

**200**

OK

get/v1/sites/{siteId}/wans

https://10.103.0.1/proxy/network/integration/v1/sites/{siteId}/wans

### Response samples

*   200

Content type

application/json

Copy

 Expand all  Collapse all 

`{"offset": 0,"limit": 25,"count": 10,"totalCount": 1000,"data": [{"id": "497f6eca-6276-4993-bfeb-53cbbbba6f08","name": "Internet 1"}]}`

## [](#tag/Supporting-Resources/operation/getSiteToSiteVpnTunnelPage)List Site-To-Site VPN Tunnels

Retrieve a paginated list of all site-to-site VPN tunnels on a site.

Filterable properties (click to expand)
| Name | Type | Allowed functions |
| --- | --- | --- |
| `type` | `STRING` | `eq``ne``in``notIn` |
| `id` | `UUID` | `eq``ne``in``notIn` |
| `name` | `STRING` | `eq``ne``in``notIn``like` |
| `metadata.origin` | `STRING` | `eq``ne``in``notIn` |
| `metadata.source` | `STRING` | `eq``ne``in``notIn``isNull``isNotNull` |

##### path Parameters

siteId

required string<uuid>

##### query Parameters

offset integer<int32>>= 0 

 Default: 0
limit integer<int32> [ 0 .. 200 ] 

 Default: 25
filter string

### Responses

**200**

OK

get/v1/sites/{siteId}/vpn/site-to-site-tunnels

https://10.103.0.1/proxy/network/integration/v1/sites/{siteId}/vpn/site-to-site-tunnels

### Response samples

*   200

Content type

application/json

Copy

 Expand all  Collapse all 

`{"offset": 0,"limit": 25,"count": 10,"totalCount": 1000,"data": [{"type": "string","id": "497f6eca-6276-4993-bfeb-53cbbbba6f08","name": "string","metadata": {"origin": "string"}}]}`

## [](#tag/Supporting-Resources/operation/getVpnServerPage)List VPN Servers

Retrieve a paginated list of all VPN servers on a site.

Filterable properties (click to expand)
| Name | Type | Allowed functions |
| --- | --- | --- |
| `type` | `STRING` | `eq``ne``in``notIn` |
| `id` | `UUID` | `eq``ne``in``notIn` |
| `name` | `STRING` | `eq``ne``in``notIn``like` |
| `enabled` | `BOOLEAN` | `eq``ne` |
| `metadata.origin` | `STRING` | `eq``ne``in``notIn` |

##### path Parameters

siteId

required string<uuid>

##### query Parameters

offset integer<int32>>= 0 

 Default: 0
limit integer<int32> [ 0 .. 200 ] 

 Default: 25
filter string

### Responses

**200**

OK

get/v1/sites/{siteId}/vpn/servers

https://10.103.0.1/proxy/network/integration/v1/sites/{siteId}/vpn/servers

### Response samples

*   200

Content type

application/json

Copy

 Expand all  Collapse all 

`{"offset": 0,"limit": 25,"count": 10,"totalCount": 1000,"data": [{"type": "string","id": "497f6eca-6276-4993-bfeb-53cbbbba6f08","name": "string","enabled": true,"metadata": {"origin": "string"}}]}`

## [](#tag/Supporting-Resources/operation/getRadiusProfileOverviewPage)List Radius Profiles

Returns available RADIUS authentication profiles, including configuration origin metadata.

Filterable properties (click to expand)
| Name | Type | Allowed functions |
| --- | --- | --- |
| `id` | `UUID` | `eq``ne``in``notIn` |
| `name` | `STRING` | `eq``ne``in``notIn``like` |
| `metadata.origin` | `STRING` | `eq``ne``in``notIn` |

##### path Parameters

siteId

required string<uuid>

##### query Parameters

offset integer<int32>>= 0 

 Default: 0
limit integer<int32> [ 0 .. 200 ] 

 Default: 25
filter string

### Responses

**200**

OK

get/v1/sites/{siteId}/radius/profiles

https://10.103.0.1/proxy/network/integration/v1/sites/{siteId}/radius/profiles

### Response samples

*   200

Content type

application/json

Copy

 Expand all  Collapse all 

`{"offset": 0,"limit": 25,"count": 10,"totalCount": 1000,"data": [{"id": "497f6eca-6276-4993-bfeb-53cbbbba6f08","name": "string","metadata": {"origin": "string"}}]}`

## [](#tag/Supporting-Resources/operation/getDeviceTagPage)List Device Tags

Returns all device tags defined within a site, which can be used for WiFi Broadcast assignments.

Filterable properties (click to expand)
| Name | Type | Allowed functions |
| --- | --- | --- |
| `id` | `UUID` | `eq``ne``in``notIn` |
| `name` | `STRING` | `eq``ne``in``notIn``like` |
| `deviceIds` | `SET(UUID)` | `contains``containsAny``containsAll``containsExactly` |

##### path Parameters

siteId

required string<uuid>

##### query Parameters

offset integer<int32>>= 0 

 Default: 0
limit integer<int32> [ 0 .. 200 ] 

 Default: 25
filter any (FilterExpression)

### Responses

**200**

OK

get/v1/sites/{siteId}/device-tags

https://10.103.0.1/proxy/network/integration/v1/sites/{siteId}/device-tags

### Response samples

*   200

Content type

application/json

Copy

 Expand all  Collapse all 

`{"offset": 0,"limit": 25,"count": 10,"totalCount": 1000,"data": [{"id": "497f6eca-6276-4993-bfeb-53cbbbba6f08","name": "string","deviceIds": ["497f6eca-6276-4993-bfeb-53cbbbba6f08"],"metadata": {"origin": "string"}}]}`

## [](#tag/Supporting-Resources/operation/getDpiApplicationCategories)List DPI Application Categories

Returns predefined Deep Packet Inspection (DPI) application categories used for traffic identification and filtering.

Filterable properties (click to expand)
| Name | Type | Allowed functions |
| --- | --- | --- |
| `id` | `INTEGER` | `eq``ne``in``notIn` |
| `name` | `STRING` | `eq``ne``in``notIn``like` |

##### query Parameters

offset integer<int32>>= 0 

 Default: 0
limit integer<int32> [ 0 .. 200 ] 

 Default: 25
filter string

### Responses

**200**

OK

get/v1/dpi/categories

https://10.103.0.1/proxy/network/integration/v1/dpi/categories

### Response samples

*   200

Content type

application/json

Copy

 Expand all  Collapse all 

`{"offset": 0,"limit": 25,"count": 10,"totalCount": 1000,"data": [{"id": "3|5","name": "Network protocols|Business tools"}]}`

## [](#tag/Supporting-Resources/operation/getDpiApplications)List DPI Applications

Lists DPI-recognized applications grouped under categories. Useful for firewall or traffic analytics integration.

Filterable properties (click to expand)
| Name | Type | Allowed functions |
| --- | --- | --- |
| `id` | `INTEGER` | `eq``ne``in``notIn` |
| `name` | `STRING` | `eq``ne``in``notIn``like` |

##### query Parameters

offset integer<int32>>= 0 

 Default: 0
limit integer<int32> [ 0 .. 200 ] 

 Default: 25
filter string

### Responses

**200**

OK

get/v1/dpi/applications

https://10.103.0.1/proxy/network/integration/v1/dpi/applications

### Response samples

*   200

Content type

application/json

Copy

 Expand all  Collapse all 

`{"offset": 0,"limit": 25,"count": 10,"totalCount": 1000,"data": [{"id": "786435|720973","name": "Adobe Express|Zoom"}]}`

## [](#tag/Supporting-Resources/operation/getCountries)List Countries

Returns ISO-standard country codes and names, used for region-based configuration or regulatory compliance.

Filterable properties (click to expand)
| Name | Type | Allowed functions |
| --- | --- | --- |
| `code` | `STRING` | `eq``ne``in``notIn` |
| `name` | `STRING` | `eq``ne``in``notIn``like` |

##### query Parameters

offset integer<int32>>= 0 

 Default: 0
limit integer<int32> [ 0 .. 200 ] 

 Default: 25
filter string

### Responses

**200**

OK

get/v1/countries

https://10.103.0.1/proxy/network/integration/v1/countries

### Response samples

*   200

Content type

application/json

Copy

 Expand all  Collapse all 

`{"offset": 0,"limit": 25,"count": 10,"totalCount": 1000,"data": [{"code": "CK|FK|KY","name": "Cook Islands|Falkland Islands, Malvinas|Cayman Islands"}]}`
