# ESP8266-Functions-Common

A collection of small, focused Arduino libraries factored out of code that
used to be duplicated (often with subtly different, independently-drifted
signatures) across this account's ESP8266/Arduino sketches. Originally one
monolithic `GarfieldCommon.h`/`.cpp` file (this repo used to be named
`ESP8266-Garfield-Common`); split up here so a sketch only needs to pull in
the pieces it actually uses.

## Libraries

| Library | What it does | Used by |
|---|---|---|
| [StringHelpers](StringHelpers) | `String` → `char*` conversion for display/Serial APIs | every sketch |
| [AlarmBeeper](AlarmBeeper) | Buzzer short/long/off/pattern beeps, either wiring polarity | every sketch with a buzzer |
| [MathQuizGenerator](MathQuizGenerator) | Random arithmetic question generator | the MathToy sketches |
| [BacklightController](BacklightController) | Photoresistor-based auto-dimming backlight | sketches with a backlight pin |
| [WiFiMultiConnect](WiFiMultiConnect) | Multi-SSID-with-fallback WiFi connect, or WiFiManager portal | every WiFi sketch |
| [WeatherDisplayHelpers](WeatherDisplayHelpers) | Meteocons icon fonts, day/night icon pick, wind-direction translation | weather-display sketches |
| [DeviceFleetClient](DeviceFleetClient) | Client for a small custom device-fleet config/logging/OTA backend | the piano humidity controller |
| [BootSplashBitmap](BootSplashBitmap) | The boot splash bitmap several sketches show on power-up | sketches with the Garfield splash |

## Installing

Each subfolder here is a self-contained Arduino library (`library.properties`
+ `src/`). Copy whichever ones you need into your Arduino `libraries/`
folder (e.g. `Documents/Arduino/libraries/StringHelpers/`), or clone this
whole repo there and it'll show up as several libraries at once.

The consuming sketches in this account currently **vendor** copies of the
specific libraries they need directly into their own repo (so each sketch
repo is self-contained and buildable on its own without also cloning this
one) - see each sketch's README for exactly which ones. If you update a
library here, re-copy its `src/` files into any sketch that vendors it.

## What got dropped, and why

A few things from the old `GarfieldCommon` were **not** carried forward,
because no current sketch in this account actually calls them (verified by
grepping every `.ino` here for each function name before dropping it):

- **SMTP mailer** (`SMTPSend`/`SMTPSendMail`/`SMTPReceive`/`SMTPFail`) - sent
  auth credentials over an unencrypted connection with no TLS support, and
  nothing currently calls it. Available in this repo's git history if you
  need it for another project, but consider a TLS-capable library (e.g.
  [ESP-Mail-Client](https://github.com/mobizt/ESP-Mail-Client)) instead.
- **SPIFFS poem reader** (`convertPoemNumberToFileName`, `readPoemFromSPIFFS`)
  and **timer-settings-website client** (`readTimerSettingsSPIFFS`,
  `readTimerWebSite`) - tied to features (a poem-of-the-day display, a
  "Blynk Relay" project) not present in any current sketch.
- `activeSymbole`/`inactiveSymbole` bitmap arrays and the unused
  `weatherBeginHour`/`weatherEndHour` constants - never referenced anywhere.
- `listSPIFFSFiles()` - a pure debug convenience (prints the SPIFFS file
  listing to Serial at boot, and only when `DEBUG` is defined) called by two
  sketches, but it didn't fit thematically into any of the 8 libraries
  above. Each of those two sketches' setup() just doesn't print the listing
  anymore; nothing else depended on it running.

If one of your own private sketches depends on any of these, they're still
in this repo's git history before the split - `git log --all --oneline` /
`git show <commit>:GarfieldCommon.cpp` will find them.
