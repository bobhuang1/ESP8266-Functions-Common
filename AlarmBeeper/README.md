# AlarmBeeper

Buzzer/alarm-pin helpers for either wiring polarity (active-high or
active-low), with short/long/off presets plus a repeated-pulse pattern for
things like a "wrong answer" buzz or a Geiger-counter click.

## Usage

```cpp
#include <AlarmBeeper.h>

#define ALARM_PIN 5
const bool ACTIVE_HIGH = true; // set false if your buzzer/LED is wired active-low

pinMode(ALARM_PIN, OUTPUT);
beepOff(ALARM_PIN, ACTIVE_HIGH);      // silence on boot

beepShort(ALARM_PIN, ACTIVE_HIGH);    // correct-answer beep, 150ms
beepLong(ALARM_PIN, ACTIVE_HIGH);     // alert beep, 2000ms
beepPattern(ALARM_PIN, ACTIVE_HIGH, 8, 30, 30); // "wrong answer" buzz
beepPattern(ALARM_PIN, ACTIVE_HIGH, 1, 1, 0);   // single 1ms click (e.g. Geiger counter)

beep(ALARM_PIN, ACTIVE_HIGH, 500);    // any custom duration
```

## Migrating from the old per-sketch copies

This replaces `shortBeep`/`longBeep`/`noBeep`/`wrongBeep`/`shortGeigerBeep`,
which were duplicated with slightly different signatures across several
sketches (some took `(pin, activeHigh)`, some took just `(pin)` and baked the
polarity into a compile-time `#ifdef USE_HIGH_ALARM`). Every call site now
passes both parameters explicitly.
