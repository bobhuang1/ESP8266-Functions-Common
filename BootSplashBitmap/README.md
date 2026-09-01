# BootSplashBitmap

A 66x64 boot splash bitmap, packaged as a u8g2-ready byte array for
display on power-up.

## Usage

```cpp
#include <BootSplashBitmap.h>

display.clearBuffer();
display.drawXBM(31, 0, 66, 64, garfield);
display.sendBuffer();
```

Source image, plus a few other small status icons for OLED sketches:
[GarfieldXBM](https://github.com/bobhuang1/GarfieldXBM).
