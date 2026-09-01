# WeatherDisplayHelpers

The "Meteocons" weather-icon bitmap fonts (u8g2 font format, two sizes) plus
two small helpers for displaying weather on an OLED: picking the day vs.
night icon glyph, and translating N/S/E/W wind directions to Chinese.

## Usage

```cpp
#include <WeatherDisplayHelpers.h>

display.setFont(Meteocon21); // or Meteocon36 for the larger size
display.drawStr(x, y, string2char(chooseMeteoconChar(currentWeather.iconMeteoCon)));

String windLabel = translateWindDirectionToChinese(currentWeather.wind_dir) + "3km/h";
```

`chooseMeteoconChar` expects a 2-character string (day glyph, then night
glyph) - see [esp8266-weather-WeatherApi](https://github.com/bobhuang1/esp8266-weather-WeatherApi)'s
`getMeteoconIcon()` for how those pairs are produced from a weather
condition code - and picks whichever is appropriate for the current local
time (day: 6:00-18:00).
