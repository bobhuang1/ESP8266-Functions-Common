# BacklightController

Auto-dims a PWM-driven LCD backlight based on a photoresistor, using a
rolling 10-sample average to avoid flicker from momentary light changes.

## Usage

```cpp
#include <BacklightController.h>

#define BACKLIGHT_PIN 0

BacklightController backlight;

void setup() {
  backlight.begin(BACKLIGHT_PIN); // reads the photoresistor on A0 by default
}

void loop() {
  backlight.update(); // call every loop iteration (or on your own timer)
  // ...
}
```

`update(biasLevel, dynamicLevel)` - `biasLevel` (try 50-200) shifts the
brightness threshold to match your specific photoresistor/enclosure;
`dynamicLevel` (100 = unscaled) scales the overall response.

`turnOff(minimumLevel)` forces a fixed dim level regardless of the sensor
(e.g. for a scheduled night-time dim). `selfTest()` ramps through the full
PWM range once, useful as a visible power-on check.
