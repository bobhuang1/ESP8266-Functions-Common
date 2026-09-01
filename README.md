# ESP8266-Functions-Common

A collection of small, focused Arduino libraries for common ESP8266/Arduino
sketch needs: string conversion, buzzer patterns, a math quiz generator,
backlight auto-dimming, WiFi connection management, weather display icons,
a device-fleet client, and a boot splash bitmap. Each library is
self-contained, so a sketch only needs to pull in the pieces it actually
uses.

## Libraries

| Library | What it does |
|---|---|
| [StringHelpers](StringHelpers) | `String` → `char*` conversion for display/Serial APIs |
| [AlarmBeeper](AlarmBeeper) | Buzzer short/long/off/pattern beeps, either wiring polarity |
| [MathQuizGenerator](MathQuizGenerator) | Random arithmetic question generator |
| [BacklightController](BacklightController) | Photoresistor-based auto-dimming backlight |
| [WiFiMultiConnect](WiFiMultiConnect) | Multi-SSID-with-fallback WiFi connect, or WiFiManager portal |
| [WeatherDisplayHelpers](WeatherDisplayHelpers) | Meteocons icon fonts, day/night icon pick, wind-direction translation |
| [DeviceFleetClient](DeviceFleetClient) | Client for a small device-fleet config/logging/OTA backend |
| [BootSplashBitmap](BootSplashBitmap) | A boot splash bitmap for display on power-up |

## Installing

Each subfolder here is a self-contained Arduino library (`library.properties`
+ `src/`). Copy whichever ones you need into your Arduino `libraries/`
folder (e.g. `Documents/Arduino/libraries/StringHelpers/`), or clone this
whole repo there and it'll show up as several libraries at once.

Sketches that use these libraries can also vendor copies of the specific
ones they need directly into their own repo, so each sketch stays
self-contained and buildable on its own without also cloning this one - see
a consuming sketch's README for an example. If you update a library here,
re-copy its `src/` files into any project that vendors it.
