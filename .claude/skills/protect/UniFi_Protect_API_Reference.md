Title: Control Plane

URL Source: blob:1e4d0fddcd564421a97daa1a7dc12f169406c2f70ec744b48464ea2c45bd833b

Markdown Content:
# Control Plane

S3-UNVR

[Protect](/protect "Protect")[Access](/access "Access")

![Image 1: Adam reed](blob:http://localhost/9b4fa6bae9f443900f54f79b408e0441)

*   Information about application 
    *   get Get application information 

*   Viewer information & management 
    *   get Get viewer details 
    *   patch Patch viewer settings 
    *   get Get all viewers 

*   Live view management 
    *   get Get live view details 
    *   patch Patch live view configuration 
    *   get Get all live views 
    *   post Create live view 

*   WebSocket updates 
    *   get Get update messages about devices 
    *   get Get Protect event messages 

*   Camera PTZ control & management 
    *   post Start a camera PTZ patrol 
    *   post Stop active camera PTZ patrol 
    *   post Move PTZ camera to preset 

*   Alarm manager integration 
    *   post Send a webhook to the alarm manager 

*   Light information & management 
    *   get Get light details 
    *   patch Patch light settings 
    *   get Get all lights 

*   Camera information & management 
    *   get Get camera details 
    *   patch Patch camera settings 
    *   get Get all cameras 
    *   post Create RTSPS streams for camera 
    *   del Delete camera RTSPS stream 
    *   get Get RTSPS streams for camera 
    *   get Get camera snapshot 
    *   post Permanently disable camera microphone 
    *   post Create talkback session for camera 

*   Sensor information & management 
    *   get Get sensor details 
    *   patch Patch sensor settings 
    *   get Get all sensors 

*   NVR information & management 
    *   get Get NVR details 

*   Device asset file management 
    *   post Upload device asset file 
    *   get Get device asset files 

*   Chime information & management 
    *   get Get chime details 
    *   patch Patch chime settings 
    *   get Get all chimes 

[![Image 2: redocly logo](https://cdn.redoc.ly/redoc/logo-mini.svg)API docs by Redocly](https://redocly.com/redoc/)

# UniFi Protect API (7.0.104)

Download OpenAPI specification:[Download](blob:https://10.103.0.2/343e4fd6-b6b2-4211-95c2-0200b8eb384d)

## [](#tag/Information-about-application)Information about application

## [](#tag/Information-about-application/paths/~1v1~1meta~1info/get)Get application information

Get generic information about the Protect application

### Responses

**200**

Success response

**default**

Error response

get/v1/meta/info

https://YOUR_CONSOLE_IP/proxy/protect/integration/v1/meta/info

### Response samples

*   200
*   default

Content type

application/json

Copy

`{"applicationVersion": "1.0.0"}`

## [](#tag/Viewer-information-and-management)Viewer information & management

## [](#tag/Viewer-information-and-management/paths/~1v1~1viewers~1{id}/get)Get viewer details

Get detailed information about a specific viewer

##### path Parameters

id

required string (viewerId) 

 Examples: 66d025b301ebc903e80003ea 672094f900e26303e800062a

The primary key of viewer

### Responses

**200**

Success response

**default**

Error response

get/v1/viewers/{id}

https://YOUR_CONSOLE_IP/proxy/protect/integration/v1/viewers/{id}

### Response samples

*   200
*   default

Content type

application/json

Copy

`{"id": "66d025b301ebc903e80003ea","modelKey": "viewer","state": "CONNECTED","name": "string","mac": "string","liveview": "66d025b301ebc903e80003ea","streamLimit": 0}`

## [](#tag/Viewer-information-and-management/paths/~1v1~1viewers~1{id}/patch)Patch viewer settings

Patch the settings for a specific viewer

##### path Parameters

id

required string (viewerId) 

 Examples: 66d025b301ebc903e80003ea 672094f900e26303e800062a

The primary key of viewer

##### Request Body schema: application/json

required

name string (name) 

The name of the model
liveview liveviewId (string) or null

### Responses

**200**

Success response

**default**

Error response

patch/v1/viewers/{id}

https://YOUR_CONSOLE_IP/proxy/protect/integration/v1/viewers/{id}

### Request samples

*   Payload

Content type

application/json

Copy

`{"name": "string","liveview": "66d025b301ebc903e80003ea"}`

### Response samples

*   200
*   default

Content type

application/json

Copy

`{"id": "66d025b301ebc903e80003ea","modelKey": "viewer","state": "CONNECTED","name": "string","mac": "string","liveview": "66d025b301ebc903e80003ea","streamLimit": 0}`

## [](#tag/Viewer-information-and-management/paths/~1v1~1viewers/get)Get all viewers

Get detailed information about all viewers

### Responses

**200**

Success response

**default**

Error response

get/v1/viewers

https://YOUR_CONSOLE_IP/proxy/protect/integration/v1/viewers

### Response samples

*   200
*   default

Content type

application/json

Copy

 Expand all  Collapse all 

`[{"id": "66d025b301ebc903e80003ea","modelKey": "viewer","state": "CONNECTED","name": "string","mac": "string","liveview": "66d025b301ebc903e80003ea","streamLimit": 0}]`

## [](#tag/Live-view-management)Live view management

## [](#tag/Live-view-management/paths/~1v1~1liveviews~1{id}/get)Get live view details

Get detailed information about a specific live view

##### path Parameters

id

required string (liveviewId) 

 Examples: 66d025b301ebc903e80003ea 672094f900e26303e800062a

The primary key of liveview

### Responses

**200**

Success response

**default**

Error response

get/v1/liveviews/{id}

https://YOUR_CONSOLE_IP/proxy/protect/integration/v1/liveviews/{id}

### Response samples

*   200
*   default

Content type

application/json

Copy

 Expand all  Collapse all 

`{"id": "66d025b301ebc903e80003ea","modelKey": "liveview","name": "string","isDefault": true,"isGlobal": true,"owner": "66d025b301ebc903e80003ea","layout": 1,"slots": [{"cameras": ["66d025b301ebc903e80003ea"],"cycleMode": "motion","cycleInterval": 0}]}`

## [](#tag/Live-view-management/paths/~1v1~1liveviews~1{id}/patch)Patch live view configuration

Patch the configuration about a specific live view

##### path Parameters

id

required string (liveviewId) 

 Examples: 66d025b301ebc903e80003ea 672094f900e26303e800062a

The primary key of liveview

##### Request Body schema: application/json

required

id

required string (liveviewId) 

The primary key of liveview
modelKey

required string (liveviewModelKey) 

The model key of the liveview

 Value: "liveview"
name

required string

The name of this live view.
isDefault

required boolean

Whether this live view is the default one for all viewers.
isGlobal

required boolean

Whether this live view is global and available system-wide to all users
owner

required string (userId) 

The primary key of user
layout

required number [ 1 .. 26 ] 

The number of slots this live view contains. Which as a consequence also affects the layout of the live view.
slots

required Array of objects

List of cameras visible in each given slot. And cycling settings for each slot if it has multiple cameras listed.

### Responses

**200**

Success response

**default**

Error response

patch/v1/liveviews/{id}

https://YOUR_CONSOLE_IP/proxy/protect/integration/v1/liveviews/{id}

### Request samples

*   Payload

Content type

application/json

Copy

 Expand all  Collapse all 

`{"id": "66d025b301ebc903e80003ea","modelKey": "liveview","name": "string","isDefault": true,"isGlobal": true,"owner": "66d025b301ebc903e80003ea","layout": 1,"slots": [{"cameras": ["66d025b301ebc903e80003ea"],"cycleMode": "motion","cycleInterval": 0}]}`

### Response samples

*   200
*   default

Content type

application/json

Copy

 Expand all  Collapse all 

`{"id": "66d025b301ebc903e80003ea","modelKey": "liveview","name": "string","isDefault": true,"isGlobal": true,"owner": "66d025b301ebc903e80003ea","layout": 1,"slots": [{"cameras": ["66d025b301ebc903e80003ea"],"cycleMode": "motion","cycleInterval": 0}]}`

## [](#tag/Live-view-management/paths/~1v1~1liveviews/get)Get all live views

Get detailed information about all live views

### Responses

**200**

Success response

**default**

Error response

get/v1/liveviews

https://YOUR_CONSOLE_IP/proxy/protect/integration/v1/liveviews

### Response samples

*   200
*   default

Content type

application/json

Copy

 Expand all  Collapse all 

`[{"id": "66d025b301ebc903e80003ea","modelKey": "liveview","name": "string","isDefault": true,"isGlobal": true,"owner": "66d025b301ebc903e80003ea","layout": 1,"slots": [{"cameras": ["66d025b301ebc903e80003ea"],"cycleMode": "motion","cycleInterval": 0}]}]`

## [](#tag/Live-view-management/paths/~1v1~1liveviews/post)Create live view

Create a new live view

##### Request Body schema: application/json

required

id

required string (liveviewId) 

The primary key of liveview
modelKey

required string (liveviewModelKey) 

The model key of the liveview

 Value: "liveview"
name

required string

The name of this live view.
isDefault

required boolean

Whether this live view is the default one for all viewers.
isGlobal

required boolean

Whether this live view is global and available system-wide to all users
owner

required string (userId) 

The primary key of user
layout

required number [ 1 .. 26 ] 

The number of slots this live view contains. Which as a consequence also affects the layout of the live view.
slots

required Array of objects

List of cameras visible in each given slot. And cycling settings for each slot if it has multiple cameras listed.

### Responses

**200**

Success response

**default**

Error response

post/v1/liveviews

https://YOUR_CONSOLE_IP/proxy/protect/integration/v1/liveviews

### Request samples

*   Payload

Content type

application/json

Copy

 Expand all  Collapse all 

`{"id": "66d025b301ebc903e80003ea","modelKey": "liveview","name": "string","isDefault": true,"isGlobal": true,"owner": "66d025b301ebc903e80003ea","layout": 1,"slots": [{"cameras": ["66d025b301ebc903e80003ea"],"cycleMode": "motion","cycleInterval": 0}]}`

### Response samples

*   200
*   default

Content type

application/json

Copy

 Expand all  Collapse all 

`{"id": "66d025b301ebc903e80003ea","modelKey": "liveview","name": "string","isDefault": true,"isGlobal": true,"owner": "66d025b301ebc903e80003ea","layout": 1,"slots": [{"cameras": ["66d025b301ebc903e80003ea"],"cycleMode": "motion","cycleInterval": 0}]}`

## [](#tag/WebSocket-updates)WebSocket updates

## [](#tag/WebSocket-updates/paths/~1v1~1subscribe~1devices/get)Get update messages about devices

A WebSocket subscription which broadcasts all changes happening to Protect-managed hardware devices

### Responses

**200**

Success response

**default**

Error response

get/v1/subscribe/devices

https://YOUR_CONSOLE_IP/proxy/protect/integration/v1/subscribe/devices

### Response samples

*   200
*   default

Content type

application/json

Example

add 

Copy

 Expand all  Collapse all 

`{"type": "add","item": {"id": "66d025b301ebc903e80003ea","modelKey": "nvr","name": "string","doorbellSettings": {"defaultMessageText": "string","defaultMessageResetTimeoutMs": 0,"customMessages": ["string"],"customImages": [{"preview": "string","sprite": "string"}]}}}`

## [](#tag/WebSocket-updates/paths/~1v1~1subscribe~1events/get)Get Protect event messages

A WebSocket subscription that broadcasts Protect events

### Responses

**200**

Success response

**default**

Error response

get/v1/subscribe/events

https://YOUR_CONSOLE_IP/proxy/protect/integration/v1/subscribe/events

### Response samples

*   200
*   default

Content type

application/json

Example

add 

Copy

 Expand all  Collapse all 

`{"type": "add","item": {"id": "66d025b301ebc903e80003ea","modelKey": "event","type": "ring","start": 1445408038748,"end": 1445408048748,"device": "66d025b301ebc903e80003ea"}}`

## [](#tag/Camera-PTZ-control-and-management)Camera PTZ control & management

## [](#tag/Camera-PTZ-control-and-management/paths/~1v1~1cameras~1{id}~1ptz~1patrol~1start~1{slot}/post)Start a camera PTZ patrol

Start a camera PTZ patrol

##### path Parameters

id

required string (cameraId) 

 Examples: 66d025b301ebc903e80003ea 672094f900e26303e800062a

The primary key of camera
slot

required string (activePatrolSlotString) 

 Examples: 0 1 2 3 4

The slot number (0-4) of the patrol that is currently running, or null if no patrol is running

### Responses

**204**

The camera PTZ patrol was started successfully.

**default**

Error response

post/v1/cameras/{id}/ptz/patrol/start/{slot}

https://YOUR_CONSOLE_IP/proxy/protect/integration/v1/cameras/{id}/ptz/patrol/start/{slot}

### Response samples

*   default

Content type

application/json

Copy

 Expand all  Collapse all 

`{"error": "Unexpected API error occurred","name": "API_ERROR","cause": {"error": "Unexpected functionality error","name": "UNKNOWN_ERROR"}}`

## [](#tag/Camera-PTZ-control-and-management/paths/~1v1~1cameras~1{id}~1ptz~1patrol~1stop/post)Stop active camera PTZ patrol

Stop active camera PTZ patrol

##### path Parameters

id

required string (cameraId) 

 Examples: 66d025b301ebc903e80003ea 672094f900e26303e800062a

The primary key of camera

### Responses

**204**

The camera PTZ patrol was stopped successfully.

**default**

Error response

post/v1/cameras/{id}/ptz/patrol/stop

https://YOUR_CONSOLE_IP/proxy/protect/integration/v1/cameras/{id}/ptz/patrol/stop

### Response samples

*   default

Content type

application/json

Copy

 Expand all  Collapse all 

`{"error": "Unexpected API error occurred","name": "API_ERROR","cause": {"error": "Unexpected functionality error","name": "UNKNOWN_ERROR"}}`

## [](#tag/Camera-PTZ-control-and-management/paths/~1v1~1cameras~1{id}~1ptz~1goto~1{slot}/post)Move PTZ camera to preset

Adjust the PTZ camera position to a specified preset

##### path Parameters

id

required string (cameraId) 

 Examples: 66d025b301ebc903e80003ea 672094f900e26303e800062a

The primary key of camera
slot

required string

 Examples: -1 0 2 8 9

The slot number (0-4) of the preset to move the camera to

### Responses

**204**

The PTZ camera was moved to the given preset successfully.

**default**

Error response

post/v1/cameras/{id}/ptz/goto/{slot}

https://YOUR_CONSOLE_IP/proxy/protect/integration/v1/cameras/{id}/ptz/goto/{slot}

### Response samples

*   default

Content type

application/json

Copy

 Expand all  Collapse all 

`{"error": "Unexpected API error occurred","name": "API_ERROR","cause": {"error": "Unexpected functionality error","name": "UNKNOWN_ERROR"}}`

## [](#tag/Alarm-manager-integration)Alarm manager integration

## [](#tag/Alarm-manager-integration/paths/~1v1~1alarm-manager~1webhook~1{id}/post)Send a webhook to the alarm manager

Send a webhook to the alarm manager to trigger configured alarms

##### path Parameters

id

required string (alarmTriggerId) 

 Examples: AnyRandomString

User defined string used to trigger only specific alarms. Alarm should be configured with the same ID to be triggered.

### Responses

**204**

Webhook was sent to alarm manager successfully

**400**

Bad request response

post/v1/alarm-manager/webhook/{id}

https://YOUR_CONSOLE_IP/proxy/protect/integration/v1/alarm-manager/webhook/{id}

### Response samples

*   400

Content type

application/json

Copy

 Expand all  Collapse all 

`{"error": "'id' is required","name": "BAD_REQUEST","cause": {"error": "Unexpected functionality error","name": "UNKNOWN_ERROR"}}`

## [](#tag/Light-information-and-management)Light information & management

## [](#tag/Light-information-and-management/paths/~1v1~1lights~1{id}/get)Get light details

Get detailed information about a specific light

##### path Parameters

id

required string (lightId) 

 Examples: 66d025b301ebc903e80003ea 672094f900e26303e800062a

The primary key of light

### Responses

**200**

Success response

**default**

Error response

get/v1/lights/{id}

https://YOUR_CONSOLE_IP/proxy/protect/integration/v1/lights/{id}

### Response samples

*   200
*   default

Content type

application/json

Copy

 Expand all  Collapse all 

`{"id": "66d025b301ebc903e80003ea","modelKey": "light","state": "CONNECTED","name": "string","mac": "string","lightModeSettings": {"mode": "always","enableAt": "fulltime"},"lightDeviceSettings": {"isIndicatorEnabled": true,"pirDuration": 0,"pirSensitivity": 100,"ledLevel": 1},"isDark": true,"isLightOn": true,"isLightForceEnabled": true,"lastMotion": 0,"isPirMotionDetected": true,"camera": "66d025b301ebc903e80003ea"}`

## [](#tag/Light-information-and-management/paths/~1v1~1lights~1{id}/patch)Patch light settings

Patch the settings for a specific light

##### path Parameters

id

required string (lightId) 

 Examples: 66d025b301ebc903e80003ea 672094f900e26303e800062a

The primary key of light

##### Request Body schema: application/json

required

name string (name) 

The name of the model
isLightForceEnabled boolean (isLightForceEnabled) 

Whether the light has its main LED currently force-enabled.
lightModeSettings object (lightModeSettings) 

Settings for when and how your light gets activated
lightDeviceSettings object (lightDeviceSettings) 

Hardware settings for light device.

### Responses

**200**

Success response

**default**

Error response

patch/v1/lights/{id}

https://YOUR_CONSOLE_IP/proxy/protect/integration/v1/lights/{id}

### Request samples

*   Payload

Content type

application/json

Copy

 Expand all  Collapse all 

`{"name": "string","isLightForceEnabled": true,"lightModeSettings": {"mode": "always","enableAt": "fulltime"},"lightDeviceSettings": {"isIndicatorEnabled": true,"pirDuration": 0,"pirSensitivity": 100,"ledLevel": 1}}`

### Response samples

*   200
*   default

Content type

application/json

Copy

 Expand all  Collapse all 

`{"id": "66d025b301ebc903e80003ea","modelKey": "light","state": "CONNECTED","name": "string","mac": "string","lightModeSettings": {"mode": "always","enableAt": "fulltime"},"lightDeviceSettings": {"isIndicatorEnabled": true,"pirDuration": 0,"pirSensitivity": 100,"ledLevel": 1},"isDark": true,"isLightOn": true,"isLightForceEnabled": true,"lastMotion": 0,"isPirMotionDetected": true,"camera": "66d025b301ebc903e80003ea"}`

## [](#tag/Light-information-and-management/paths/~1v1~1lights/get)Get all lights

Get detailed information about all lights

### Responses

**200**

Success response

**default**

Error response

get/v1/lights

https://YOUR_CONSOLE_IP/proxy/protect/integration/v1/lights

### Response samples

*   200
*   default

Content type

application/json

Copy

 Expand all  Collapse all 

`[{"id": "66d025b301ebc903e80003ea","modelKey": "light","state": "CONNECTED","name": "string","mac": "string","lightModeSettings": {"mode": "always","enableAt": "fulltime"},"lightDeviceSettings": {"isIndicatorEnabled": true,"pirDuration": 0,"pirSensitivity": 100,"ledLevel": 1},"isDark": true,"isLightOn": true,"isLightForceEnabled": true,"lastMotion": 0,"isPirMotionDetected": true,"camera": "66d025b301ebc903e80003ea"}]`

## [](#tag/Camera-information-and-management)Camera information & management

## [](#tag/Camera-information-and-management/paths/~1v1~1cameras~1{id}/get)Get camera details

Get detailed information about a specific camera

##### path Parameters

id

required string (cameraId) 

 Examples: 66d025b301ebc903e80003ea 672094f900e26303e800062a

The primary key of camera

### Responses

**200**

Success response

**default**

Error response

get/v1/cameras/{id}

https://YOUR_CONSOLE_IP/proxy/protect/integration/v1/cameras/{id}

### Response samples

*   200
*   default

Content type

application/json

Copy

 Expand all  Collapse all 

`{"id": "66d025b301ebc903e80003ea","modelKey": "camera","state": "CONNECTED","name": "string","mac": "string","isMicEnabled": true,"osdSettings": {"isNameEnabled": true,"isDateEnabled": true,"isLogoEnabled": true,"isDebugEnabled": true,"overlayLocation": "topLeft"},"ledSettings": {"isEnabled": true,"welcomeLed": true,"floodLed": true},"lcdMessage": {"type": "LEAVE_PACKAGE_AT_DOOR","resetAt": 0,"text": "string"},"micVolume": 100,"activePatrolSlot": 0,"videoMode": "default","hdrType": "auto","featureFlags": {"supportFullHdSnapshot": true,"hasHdr": true,"smartDetectTypes": ["person"],"smartDetectAudioTypes": ["alrmSmoke"],"videoModes": ["default"],"hasMic": true,"hasLedStatus": true,"hasSpeaker": true},"smartDetectSettings": {"objectTypes": ["person"],"audioTypes": ["alrmSmoke"]}}`

## [](#tag/Camera-information-and-management/paths/~1v1~1cameras~1{id}/patch)Patch camera settings

Patch the settings for a specific camera

##### path Parameters

id

required string (cameraId) 

 Examples: 66d025b301ebc903e80003ea 672094f900e26303e800062a

The primary key of camera

##### Request Body schema: application/json

required

name string

The name of the camera
osdSettings object (osdSettings) 

On Screen Display settings.
ledSettings object (ledSettings) 

LED settings.
lcdMessage lcdMessage (object) or lcdMessage (object) or lcdMessage (object) or lcdMessage (object) (lcdMessage)
micVolume number (micVolume)  [ 0 .. 100 ] 

Mic volume: a number from 0-100.
videoMode string (videoMode) 

 Enum:"default""highFps""sport""slowShutter""lprReflex""lprNoneReflex"

Current video mode of the camera
hdrType string (videoMode) 

 Enum:"auto""on""off"

High Dynamic Range (HDR) mode setting.
smartDetectSettings object (smartDetectSettings) 

Smart detection settings for the camera.

### Responses

**200**

Success response

**default**

Error response

patch/v1/cameras/{id}

https://YOUR_CONSOLE_IP/proxy/protect/integration/v1/cameras/{id}

### Request samples

*   Payload

Content type

application/json

Copy

 Expand all  Collapse all 

`{"name": "string","osdSettings": {"isNameEnabled": true,"isDateEnabled": true,"isLogoEnabled": true,"isDebugEnabled": true,"overlayLocation": "topLeft"},"ledSettings": {"isEnabled": true,"welcomeLed": true,"floodLed": true},"lcdMessage": {"type": "DO_NOT_DISTURB","resetAt": 0},"micVolume": 100,"videoMode": "default","hdrType": "auto","smartDetectSettings": {"objectTypes": ["person"],"audioTypes": ["alrmSmoke"]}}`

### Response samples

*   200
*   default

Content type

application/json

Copy

 Expand all  Collapse all 

`{"id": "66d025b301ebc903e80003ea","modelKey": "camera","state": "CONNECTED","name": "string","mac": "string","isMicEnabled": true,"osdSettings": {"isNameEnabled": true,"isDateEnabled": true,"isLogoEnabled": true,"isDebugEnabled": true,"overlayLocation": "topLeft"},"ledSettings": {"isEnabled": true,"welcomeLed": true,"floodLed": true},"lcdMessage": {"type": "LEAVE_PACKAGE_AT_DOOR","resetAt": 0,"text": "string"},"micVolume": 100,"activePatrolSlot": 0,"videoMode": "default","hdrType": "auto","featureFlags": {"supportFullHdSnapshot": true,"hasHdr": true,"smartDetectTypes": ["person"],"smartDetectAudioTypes": ["alrmSmoke"],"videoModes": ["default"],"hasMic": true,"hasLedStatus": true,"hasSpeaker": true},"smartDetectSettings": {"objectTypes": ["person"],"audioTypes": ["alrmSmoke"]}}`

## [](#tag/Camera-information-and-management/paths/~1v1~1cameras/get)Get all cameras

Get detailed information about all cameras

### Responses

**200**

Success response

**default**

Error response

get/v1/cameras

https://YOUR_CONSOLE_IP/proxy/protect/integration/v1/cameras

### Response samples

*   200
*   default

Content type

application/json

Copy

 Expand all  Collapse all 

`[{"id": "66d025b301ebc903e80003ea","modelKey": "camera","state": "CONNECTED","name": "string","mac": "string","isMicEnabled": true,"osdSettings": {"isNameEnabled": true,"isDateEnabled": true,"isLogoEnabled": true,"isDebugEnabled": true,"overlayLocation": "topLeft"},"ledSettings": {"isEnabled": true,"welcomeLed": true,"floodLed": true},"lcdMessage": {"type": "LEAVE_PACKAGE_AT_DOOR","resetAt": 0,"text": "string"},"micVolume": 100,"activePatrolSlot": 0,"videoMode": "default","hdrType": "auto","featureFlags": {"supportFullHdSnapshot": true,"hasHdr": true,"smartDetectTypes": ["person"],"smartDetectAudioTypes": ["alrmSmoke"],"videoModes": ["default"],"hasMic": true,"hasLedStatus": true,"hasSpeaker": true},"smartDetectSettings": {"objectTypes": ["person"],"audioTypes": ["alrmSmoke"]}}]`

## [](#tag/Camera-information-and-management/paths/~1v1~1cameras~1{id}~1rtsps-stream/post)Create RTSPS streams for camera

Returns RTSPS stream URLs for specified quality levels

##### path Parameters

id

required string (cameraId) 

 Examples: 66d025b301ebc903e80003ea 672094f900e26303e800062a

The primary key of camera

##### Request Body schema: application/json

required

qualities

required Array of strings (createdQualities)  non-empty 

Items Enum:"high""medium""low""package"

Array of quality levels of RTSPS streams

### Responses

**200**

Success response

**default**

Error response

post/v1/cameras/{id}/rtsps-stream

https://YOUR_CONSOLE_IP/proxy/protect/integration/v1/cameras/{id}/rtsps-stream

### Request samples

*   Payload

Content type

application/json

Copy

 Expand all  Collapse all 

`{"qualities": ["high","medium"]}`

### Response samples

*   200
*   default

Content type

application/json

Copy

`{"high": "rtsps://192.168.1.1:7441/5nPr7RCmueGTKMP7?enableSrtp","medium": "rtsps://192.168.1.1:7441/AbUgnDb5IqIEMidk?enableSrtp"}`

## [](#tag/Camera-information-and-management/paths/~1v1~1cameras~1{id}~1rtsps-stream/delete)Delete camera RTSPS stream

Remove the RTSPS stream for a specified camera

##### path Parameters

id

required string (cameraId) 

 Examples: 66d025b301ebc903e80003ea 672094f900e26303e800062a

The primary key of camera

##### query Parameters

qualities

required Array of removedQualities (strings) or channelQuality (string) (removedQualities) 

 Examples: qualities=high&qualities=medium

The array of quality levels for the RTSPS streams to be removed.

### Responses

**204**

RTSPS stream successfully removed

**default**

Error response

delete/v1/cameras/{id}/rtsps-stream

https://YOUR_CONSOLE_IP/proxy/protect/integration/v1/cameras/{id}/rtsps-stream

### Response samples

*   default

Content type

application/json

Copy

 Expand all  Collapse all 

`{"error": "Unexpected API error occurred","name": "API_ERROR","cause": {"error": "Unexpected functionality error","name": "UNKNOWN_ERROR"}}`

## [](#tag/Camera-information-and-management/paths/~1v1~1cameras~1{id}~1rtsps-stream/get)Get RTSPS streams for camera

Returns existing RTSPS stream URLs for camera

##### path Parameters

id

required string (cameraId) 

 Examples: 66d025b301ebc903e80003ea 672094f900e26303e800062a

The primary key of camera

### Responses

**200**

Success response

**default**

Error response

get/v1/cameras/{id}/rtsps-stream

https://YOUR_CONSOLE_IP/proxy/protect/integration/v1/cameras/{id}/rtsps-stream

### Response samples

*   200
*   default

Content type

application/json

Copy

`{"high": "rtsps://192.168.1.1:7441/5nPr7RCmueGTKMP7?enableSrtp","medium": "rtsps://192.168.1.1:7441/AbUgnDb5IqIEMidk?enableSrtp","low": null,"package": null}`

## [](#tag/Camera-information-and-management/paths/~1v1~1cameras~1{id}~1snapshot/get)Get camera snapshot

Get a snapshot image from a specific camera

##### path Parameters

id

required string (cameraId) 

 Examples: 66d025b301ebc903e80003ea 672094f900e26303e800062a

The primary key of camera

##### query Parameters

highQuality string (forceHighQuality) 

 Default: "false"

 Enum:"true""false"

Whether to force 1080P or higher resolution snapshot

### Responses

**200**

Camera snapshot

**default**

Error response

get/v1/cameras/{id}/snapshot

https://YOUR_CONSOLE_IP/proxy/protect/integration/v1/cameras/{id}/snapshot

### Response samples

*   default

Content type

application/json

Copy

 Expand all  Collapse all 

`{"error": "Unexpected API error occurred","name": "API_ERROR","cause": {"error": "Unexpected functionality error","name": "UNKNOWN_ERROR"}}`

## [](#tag/Camera-information-and-management/paths/~1v1~1cameras~1{id}~1disable-mic-permanently/post)Permanently disable camera microphone

Disable the microphone for a specific camera. This action cannot be undone unless the camera is reset.

##### path Parameters

id

required string (cameraId) 

 Examples: 66d025b301ebc903e80003ea 672094f900e26303e800062a

The primary key of camera

### Responses

**200**

Success response

**default**

Error response

post/v1/cameras/{id}/disable-mic-permanently

https://YOUR_CONSOLE_IP/proxy/protect/integration/v1/cameras/{id}/disable-mic-permanently

### Response samples

*   200
*   default

Content type

application/json

Copy

 Expand all  Collapse all 

`{"id": "66d025b301ebc903e80003ea","modelKey": "camera","state": "CONNECTED","name": "string","mac": "string","isMicEnabled": true,"osdSettings": {"isNameEnabled": true,"isDateEnabled": true,"isLogoEnabled": true,"isDebugEnabled": true,"overlayLocation": "topLeft"},"ledSettings": {"isEnabled": true,"welcomeLed": true,"floodLed": true},"lcdMessage": {"type": "LEAVE_PACKAGE_AT_DOOR","resetAt": 0,"text": "string"},"micVolume": 100,"activePatrolSlot": 0,"videoMode": "default","hdrType": "auto","featureFlags": {"supportFullHdSnapshot": true,"hasHdr": true,"smartDetectTypes": ["person"],"smartDetectAudioTypes": ["alrmSmoke"],"videoModes": ["default"],"hasMic": true,"hasLedStatus": true,"hasSpeaker": true},"smartDetectSettings": {"objectTypes": ["person"],"audioTypes": ["alrmSmoke"]}}`

## [](#tag/Camera-information-and-management/paths/~1v1~1cameras~1{id}~1talkback-session/post)Create talkback session for camera

Returns the talkback stream URL and audio configuration for a specific camera

##### path Parameters

id

required string (cameraId) 

 Examples: 66d025b301ebc903e80003ea 672094f900e26303e800062a

The primary key of camera

### Responses

**200**

Success response

**default**

Error response

post/v1/cameras/{id}/talkback-session

https://YOUR_CONSOLE_IP/proxy/protect/integration/v1/cameras/{id}/talkback-session

### Response samples

*   200
*   default

Content type

application/json

Copy

`{"url": "rtp://192.168.1.123:7004","codec": "opus","samplingRate": 24000,"bitsPerSample": 16}`

## [](#tag/Sensor-information-and-management)Sensor information & management

## [](#tag/Sensor-information-and-management/paths/~1v1~1sensors~1{id}/get)Get sensor details

Get detailed information about a specific sensor

##### path Parameters

id

required string (sensorId) 

 Examples: 66d025b301ebc903e80003ea 672094f900e26303e800062a

The primary key of sensor

### Responses

**200**

Success response

**default**

Error response

get/v1/sensors/{id}

https://YOUR_CONSOLE_IP/proxy/protect/integration/v1/sensors/{id}

### Response samples

*   200
*   default

Content type

application/json

Copy

 Expand all  Collapse all 

`{"id": "66d025b301ebc903e80003ea","modelKey": "sensor","state": "CONNECTED","name": "string","mac": "string","mountType": "garage","batteryStatus": {"percentage": 0,"isLow": true},"stats": {"light": {"value": 0,"status": "high"},"humidity": {"value": 0,"status": "high"},"temperature": {"value": 0,"status": "high"}},"lightSettings": {"isEnabled": true,"margin": 0,"lowThreshold": 1,"highThreshold": 0},"humiditySettings": {"isEnabled": true,"margin": 0,"lowThreshold": 1,"highThreshold": 0},"temperatureSettings": {"isEnabled": true,"margin": 0,"lowThreshold": -39,"highThreshold": 0},"isOpened": true,"openStatusChangedAt": 0,"isMotionDetected": true,"motionDetectedAt": 0,"motionSettings": {"isEnabled": true,"sensitivity": 100},"alarmTriggeredAt": 0,"alarmSettings": {"isEnabled": true},"leakDetectedAt": 0,"externalLeakDetectedAt": 0,"leakSettings": {"isInternalEnabled": true,"isExternalEnabled": true},"tamperingDetectedAt": 0}`

## [](#tag/Sensor-information-and-management/paths/~1v1~1sensors~1{id}/patch)Patch sensor settings

Patch the settings for a specific sensor

##### path Parameters

id

required string (sensorId) 

 Examples: 66d025b301ebc903e80003ea 672094f900e26303e800062a

The primary key of sensor

##### Request Body schema: application/json

required

name string (name) 

The name of the model
lightSettings object (lightSettings) 

Ambient light sensor settings.
humiditySettings object (humiditySettings) 

Relative humidity sensor settings.
temperatureSettings object (temperatureSettings) 

Temperature sensor settings.
motionSettings object (motionSettings) 

Motion sensor settings.
alarmSettings object (alarmSettings) 

Smoke and carbon monoxide alarm sensor settings.

### Responses

**200**

Success response

**default**

Error response

patch/v1/sensors/{id}

https://YOUR_CONSOLE_IP/proxy/protect/integration/v1/sensors/{id}

### Request samples

*   Payload

Content type

application/json

Copy

 Expand all  Collapse all 

`{"name": "string","lightSettings": {"isEnabled": true,"margin": 0,"lowThreshold": 1,"highThreshold": 0},"humiditySettings": {"isEnabled": true,"margin": 0,"lowThreshold": 1,"highThreshold": 0},"temperatureSettings": {"isEnabled": true,"margin": 0,"lowThreshold": -39,"highThreshold": 0},"motionSettings": {"isEnabled": true,"sensitivity": 100},"alarmSettings": {"isEnabled": true}}`

### Response samples

*   200
*   default

Content type

application/json

Copy

 Expand all  Collapse all 

`{"id": "66d025b301ebc903e80003ea","modelKey": "sensor","state": "CONNECTED","name": "string","mac": "string","mountType": "garage","batteryStatus": {"percentage": 0,"isLow": true},"stats": {"light": {"value": 0,"status": "high"},"humidity": {"value": 0,"status": "high"},"temperature": {"value": 0,"status": "high"}},"lightSettings": {"isEnabled": true,"margin": 0,"lowThreshold": 1,"highThreshold": 0},"humiditySettings": {"isEnabled": true,"margin": 0,"lowThreshold": 1,"highThreshold": 0},"temperatureSettings": {"isEnabled": true,"margin": 0,"lowThreshold": -39,"highThreshold": 0},"isOpened": true,"openStatusChangedAt": 0,"isMotionDetected": true,"motionDetectedAt": 0,"motionSettings": {"isEnabled": true,"sensitivity": 100},"alarmTriggeredAt": 0,"alarmSettings": {"isEnabled": true},"leakDetectedAt": 0,"externalLeakDetectedAt": 0,"leakSettings": {"isInternalEnabled": true,"isExternalEnabled": true},"tamperingDetectedAt": 0}`

## [](#tag/Sensor-information-and-management/paths/~1v1~1sensors/get)Get all sensors

Get detailed information about all sensors

### Responses

**200**

Success response

**default**

Error response

get/v1/sensors

https://YOUR_CONSOLE_IP/proxy/protect/integration/v1/sensors

### Response samples

*   200
*   default

Content type

application/json

Copy

 Expand all  Collapse all 

`[{"id": "66d025b301ebc903e80003ea","modelKey": "sensor","state": "CONNECTED","name": "string","mac": "string","mountType": "garage","batteryStatus": {"percentage": 0,"isLow": true},"stats": {"light": {"value": 0,"status": "high"},"humidity": {"value": 0,"status": "high"},"temperature": {"value": 0,"status": "high"}},"lightSettings": {"isEnabled": true,"margin": 0,"lowThreshold": 1,"highThreshold": 0},"humiditySettings": {"isEnabled": true,"margin": 0,"lowThreshold": 1,"highThreshold": 0},"temperatureSettings": {"isEnabled": true,"margin": 0,"lowThreshold": -39,"highThreshold": 0},"isOpened": true,"openStatusChangedAt": 0,"isMotionDetected": true,"motionDetectedAt": 0,"motionSettings": {"isEnabled": true,"sensitivity": 100},"alarmTriggeredAt": 0,"alarmSettings": {"isEnabled": true},"leakDetectedAt": 0,"externalLeakDetectedAt": 0,"leakSettings": {"isInternalEnabled": true,"isExternalEnabled": true},"tamperingDetectedAt": 0}]`

## [](#tag/NVR-information-and-management)NVR information & management

## [](#tag/NVR-information-and-management/paths/~1v1~1nvrs/get)Get NVR details

Get detailed information about the NVR

### Responses

**200**

Success response

**default**

Error response

get/v1/nvrs

https://YOUR_CONSOLE_IP/proxy/protect/integration/v1/nvrs

### Response samples

*   200
*   default

Content type

application/json

Copy

 Expand all  Collapse all 

`{"id": "66d025b301ebc903e80003ea","modelKey": "nvr","name": "string","doorbellSettings": {"defaultMessageText": "string","defaultMessageResetTimeoutMs": 0,"customMessages": ["string"],"customImages": [{"preview": "string","sprite": "string"}]}}`

## [](#tag/Device-asset-file-management)Device asset file management

## [](#tag/Device-asset-file-management/paths/~1v1~1files~1{fileType}/post)Upload device asset file

Upload a new device asset file

##### path Parameters

fileType

required string (assetFileType) 

 Value:"animations"

Device asset file type

##### Request Body schema: multipart/form-data

string<binary>

A binary file with one of these MIME types: image/gif, image/jpeg, image/png, audio/mpeg, audio/mp4, audio/wave, audio/x-caf}

### Responses

**200**

Processed and persisted device asset

**default**

Error response

post/v1/files/{fileType}

https://YOUR_CONSOLE_IP/proxy/protect/integration/v1/files/{fileType}

### Response samples

*   200
*   default

Content type

application/json

Copy

`{"name": "string","type": "animations","originalName": "string","path": "string"}`

## [](#tag/Device-asset-file-management/paths/~1v1~1files~1{fileType}/get)Get device asset files

Get a list of all device asset files

##### path Parameters

fileType

required string (assetFileType) 

 Value:"animations"

Device asset file type

### Responses

**200**

Device asset list

**default**

Error response

get/v1/files/{fileType}

https://YOUR_CONSOLE_IP/proxy/protect/integration/v1/files/{fileType}

### Response samples

*   200
*   default

Content type

application/json

Copy

 Expand all  Collapse all 

`[{"name": "string","type": "animations","originalName": "string","path": "string"}]`

## [](#tag/Chime-information-and-management)Chime information & management

## [](#tag/Chime-information-and-management/paths/~1v1~1chimes~1{id}/get)Get chime details

Get detailed information about a specific chime

##### path Parameters

id

required string (chimeId) 

 Examples: 66d025b301ebc903e80003ea 672094f900e26303e800062a

The primary key of chime

### Responses

**200**

Success response

**default**

Error response

get/v1/chimes/{id}

https://YOUR_CONSOLE_IP/proxy/protect/integration/v1/chimes/{id}

### Response samples

*   200
*   default

Content type

application/json

Copy

 Expand all  Collapse all 

`{"id": "66d025b301ebc903e80003ea","modelKey": "chime","state": "CONNECTED","name": "string","mac": "string","cameraIds": ["66d025b301ebc903e80003ea"],"ringSettings": [{"cameraId": "string","repeatTimes": 1,"ringtoneId": "string","volume": 100}]}`

## [](#tag/Chime-information-and-management/paths/~1v1~1chimes~1{id}/patch)Patch chime settings

Patch the settings for a specific chime

##### path Parameters

id

required string (chimeId) 

 Examples: 66d025b301ebc903e80003ea 672094f900e26303e800062a

The primary key of chime

##### Request Body schema: application/json

required

name string

The name of the chime
cameraIds Array of strings (cameraId) 

The list of (doorbell-only) cameras which this chime is paired to.
ringSettings Array of objects (ringSettings) 

List of custom ringtone settings for (doorbell-only) cameras paired to this chime.

### Responses

**200**

Success response

**default**

Error response

patch/v1/chimes/{id}

https://YOUR_CONSOLE_IP/proxy/protect/integration/v1/chimes/{id}

### Request samples

*   Payload

Content type

application/json

Copy

 Expand all  Collapse all 

`{"name": "string","cameraIds": ["66d025b301ebc903e80003ea"],"ringSettings": [{"cameraId": "string","repeatTimes": 1,"ringtoneId": "string","volume": 100}]}`

### Response samples

*   200
*   default

Content type

application/json

Copy

 Expand all  Collapse all 

`{"id": "66d025b301ebc903e80003ea","modelKey": "chime","state": "CONNECTED","name": "string","mac": "string","cameraIds": ["66d025b301ebc903e80003ea"],"ringSettings": [{"cameraId": "string","repeatTimes": 1,"ringtoneId": "string","volume": 100}]}`

## [](#tag/Chime-information-and-management/paths/~1v1~1chimes/get)Get all chimes

Get detailed information about all chimes

### Responses

**200**

Success response

**default**

Error response

get/v1/chimes

https://YOUR_CONSOLE_IP/proxy/protect/integration/v1/chimes

### Response samples

*   200
*   default

Content type

application/json

Copy

 Expand all  Collapse all 

`[{"id": "66d025b301ebc903e80003ea","modelKey": "chime","state": "CONNECTED","name": "string","mac": "string","cameraIds": ["66d025b301ebc903e80003ea"],"ringSettings": [{"cameraId": "string","repeatTimes": 1,"ringtoneId": "string","volume": 100}]}]`
