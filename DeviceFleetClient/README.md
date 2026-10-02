# DeviceFleetClient

Client for a small, custom, plain-HTTP backend that lets a fleet of ESP8266
devices pull per-device configuration (keyed by MAC address), log sensor
readings, check in on boot, and discover OTA firmware updates - without
needing to reflash any device to change its settings or point it at a new
server.

This talks to a backend **you host yourself**; it's not a public API. You'll
need to implement the endpoints below (any web stack works).

## Security warning

Everything here travels over **plain, unauthenticated HTTP**: the bootstrap file, the
per-device settings (including `token` and `alarmEmailAddress`), and the OTA firmware
URL. Anyone who can tamper with that traffic - a hostile Wi-Fi network, a DNS hijack, or
whoever controls the bootstrap host - can repoint the fleet at their own server and push
their own firmware to every device that follows the usage example below. The MAC address
is the only key used to fetch a device's settings.

Before relying on this outside a trusted LAN: serve both servers over HTTPS with a pinned
certificate, enable the ESP8266 core's signed OTA updates (`Updater` signing with a public
key compiled into the firmware), and authenticate settings requests with a per-device
secret rather than the MAC address.

`readSettings()` returns `false` and leaves `settings` unchanged when the server is
unreachable or the reply is not a complete 20-line record, so a network blip can no
longer zero the device's configuration.

## The two servers

- **Bootstrap server**: one static file (see [iot.txt.example](iot.txt.example)), fetched on every `readSettings()`
  call, that tells the device where the *real* settings server currently is.
  This indirection means you can move your settings server to a new host and
  repoint an entire already-deployed fleet at it, without touching any
  device. Format (one value per line):
  ```
  settings-server.example.com
  81
  /IOT/
  readsetting.html?mac=
  (unused - kept for file-format compatibility)
  writeboot.html?serial=
  writedata.html?serial=
  bin/
  ```
- **Settings server**: serves the endpoints below under the base URL from
  the bootstrap file (or the fallback you construct the client with, if the
  bootstrap server is unreachable).

## Endpoints your settings server needs to implement

| Purpose | URL pattern | Response |
|---|---|---|
| Read this device's settings | `readsetting.html?mac={MAC}` | 20 newline-separated values - see `DeviceFleetSettings` field order in `DeviceFleetClient.h` |
| Boot check-in | `writeboot.html?serial={n}` | any 200 response |
| Log a sensor reading | `writedata.html?serial={n}&int={insideTemp}&inh={insideHumidity}&outt={outsideTemp}&outh={outsideHumidity}&air={airQuality}` | any 200 response |
| OTA firmware | `bin/{firmwareBin}` | the `.bin` file, for `ESPhttpUpdate.update()` |

## Usage

```cpp
#include <DeviceFleetClient.h>
#include <ESP8266httpUpdate.h>

DeviceFleetClient fleet(
    "your-bootstrap-server.example.com", 80, "/iot.txt",
    "your-settings-server.example.com", 81, "/IOT/");

DeviceFleetSettings settings;

void setup() {
  fleet.readSettings(settings);
  if (settings.serialNumber < 0) {
    // unrecognized MAC address - show an error on-screen, then:
    stopApp(); // waits 2 minutes (so the error is visible), then restarts
  }

  fleet.writeBootNotification(settings.serialNumber);

  if (settings.firmwareVersion > CURRENT_VERSION) {
    ESPhttpUpdate.update(fleet.settingsServer(), fleet.settingsPort(), fleet.firmwareBinUrl(settings.firmwareBin));
  }
}

void loop() {
  // ... periodically:
  fleet.writeSensorData(settings.serialNumber, insideTemp, insideHumidity, outsideTemp, outsideHumidity);
}
```

## Design notes

Settings are read into a single `DeviceFleetSettings` struct passed by
reference; the settings-server address/port/base-URL and
bootstrap-resolution state live inside the `DeviceFleetClient` instance, so
callers don't need to track connection state themselves.

The HTTP response parser matches the status line against `" 200 "`
generically, rather than a literal HTTP-version-specific string, since a
server may respond with a different HTTP version than the client requested.
