# ESP8266-Garfield-Common

Shared ESP8266/Arduino helper code used across several of this account's
"Garfield" clock/weather-station/monitor sketches (WiFi connection with
fallback SSIDs or WiFiManager captive portal, buzzer control, backlight
dimming from a photoresistor, a bitmap-font weather icon set, plain-SMTP mail
alerts, and a simple fleet-configuration mechanism that reads settings from a
remote server).

## Before you use this

**Every credential and server address in this file is a placeholder.**
Replace them with your own before flashing a device:

| In `GarfieldCommon.h` / `GarfieldCommon.cpp` | Replace with |
|---|---|
| `WEATHERAPI_APP_ID` | Your [WeatherAPI.com](https://www.weatherapi.com/) key |
| `WIFI_HOME_SSID` / `WIFI_HOME_PWD` | Your WiFi network name(s)/password(s) |
| `WIFI_TUNNEL_SSID` / `WIFI_TUNNEL_PWD` | An alternate/tunnel network, if you use the `isTunnel` mode |
| `MOTHER_SERVER` / `SETTINGS_SERVER` | Your own device-fleet settings backend, if you use `readMother`/`readValueWebSite`/etc. - otherwise you can remove those calls from your sketch entirely |
| `SMTP_SERVER` / `SMTP_AUTH_LOGIN_USER_BASE64` / `SMTP_AUTH_LOGIN_PASSWORD_BASE64` / `SMTP_FROM_ADDRESS` | Your own SMTP relay, if you use `SMTPSend` - see the security note below first |

An earlier version of this file had real values committed for all of the
above; they've been replaced with placeholders and the exposed WiFi/SMTP/API
credentials should be treated as compromised - rotate them if you were
reusing this exact code.

## Security notes

- **Prefer `USE_WIFI_MANAGER`** (`connectWIFI(true, ...)`) over hardcoding
  WiFi credentials in source at all - it puts up a captive config portal on
  first boot instead. The hardcoded-SSID-list path exists for headless
  devices that can't use a captive portal, and should never contain real
  credentials in source you intend to publish or share.
- **`SMTPSendMail` sends SMTP AUTH LOGIN credentials over a plain,
  unencrypted `WiFiClient` connection** - no TLS. Base64 is an encoding, not
  encryption; anyone on the network path can decode it. Most SMTP providers
  require STARTTLS/implicit TLS today, which this hand-rolled client doesn't
  implement. For anything beyond a private LAN with a mail server you
  control, use a library with TLS support (e.g.
  [ESP-Mail-Client](https://github.com/mobizt/ESP-Mail-Client)) instead.
- The fleet-configuration mechanism (`readMother`, `readValueWebSite`,
  `writeDataWebSite`, etc.) talks to a plain-HTTP backend with no
  authentication beyond the device's MAC address/serial number in the URL.
  Fine for a private hobby fleet on a network you trust; not something to
  expose on the public internet as-is.

## Weather icons

`Meteocon21`/`Meteocon36` are the ThingPulse/squix78-style "Meteocons"
bitmap fonts (u8g2 font format) used by this account's ESP8266 weather
sketches to draw condition icons. `chooseMeteocon()` picks the day or night
glyph from a 2-character icon-pair string (see
[esp8266-weather-WeatherApi](https://github.com/bobhuang1/esp8266-weather-WeatherApi)'s
`getMeteoconIcon()` for how those pairs are produced from a weather
condition code).

## The `garfield` bitmap

`GarfieldCommon.h` embeds a pre-converted C byte array (`garfield[]`) used as
a boot splash screen (`display.drawXBM(31, 0, 66, 64, garfield)`). The source
image and other icons used across these sketches (mute/speaker/nuclear
symbols) live in [GarfieldXBM](https://github.com/bobhuang1/GarfieldXBM).
