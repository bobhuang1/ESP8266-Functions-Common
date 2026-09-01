# WiFiMultiConnect

Connects an ESP8266 to WiFi using either a small fallback list of known
networks, or a [WiFiManager](https://github.com/tzapu/WiFiManager) captive
config portal - plus the tiny build-timestamp/NTP constants nearly every
sketch in this account defines right after connecting.

## Usage

```cpp
#include <WiFiMultiConnect.h>

// Fill in your own networks - never commit real credentials.
const char* const WIFI_SSIDS[] = {"YOUR_SSID_1", "YOUR_SSID_2"};
const char* const WIFI_PASSWORDS[] = {"YOUR_PASSWORD_1", "YOUR_PASSWORD_2"};

void setup() {
  connectWiFi(WIFI_SSIDS, WIFI_PASSWORDS, 2);
  // - or, to avoid hardcoding credentials at all -
  // connectWiFiWithManager("ESP8266-Setup");

  configTime(TZ_SEC_FOR(8), DST_SEC_FOR(0), DefaultNtpServer); // UTC+8, no DST
}
```

`connectWiFi` scans for nearby networks, prefers whichever one from your list
it actually finds, and falls back through the rest of the list (in order,
wrapping around) if the preferred network doesn't connect within
`maxTriesPerNetwork` seconds (default 30).

`connectWiFiWithManager` puts up a captive config portal named `apName`
instead - connect to it with a phone, enter your real WiFi credentials
there, and WiFiManager stores them in flash for subsequent boots. This is
the better option whenever you don't need multiple hardcoded fallback
networks, since it means no WiFi password ever needs to live in your source
code at all.

## Migrating from the old per-sketch copies

Replaces the `connectWIFI` free function (and, in one sketch, an inlined
copy of the same logic that was never factored into a function at all) with
two clearly-named functions - `connectWiFi` for the fixed-list case,
`connectWiFiWithManager` for the portal case - instead of one function with
a `USE_WIFI_MANAGER` boolean silently switching between two different
credential sources.
