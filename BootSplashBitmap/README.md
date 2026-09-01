# BootSplashBitmap

A 66x64 boot splash bitmap (the "Garfield" image several of this account's
OLED sketches show on power-up), packaged as a u8g2-ready byte array.

## Usage

```cpp
#include <BootSplashBitmap.h>

display.clearBuffer();
display.drawXBM(31, 0, 66, 64, garfield);
display.sendBuffer();
```

Source image, plus a few other small status icons used across these
sketches: [GarfieldXBM](https://github.com/bobhuang1/GarfieldXBM).
