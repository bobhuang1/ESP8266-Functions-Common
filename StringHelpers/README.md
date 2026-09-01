# StringHelpers

- `char* string2char(String command)` - converts an Arduino `String` to the
  `char*` many display/graphics libraries (e.g. u8g2's
  `drawStr`/`getStrWidth`) and `Serial`/`WiFi` APIs expect.
- `String intToTwoDigitString(int value)` - zero-pads a 0-9 value to two
  digits, e.g. for `HH:MM:SS` display (`5` -> `"05"`, `12` -> `"12"`).

## Usage

```cpp
#include <StringHelpers.h>

display.drawStr(0, 0, string2char(myString));

String time = intToTwoDigitString(hour) + ":" + intToTwoDigitString(minute);
```

The returned pointer aliases `command`'s internal buffer - use it immediately
rather than storing it, since it becomes invalid once `command` is modified
or goes out of scope (this is a property of `String::c_str()` itself, not
specific to this wrapper).
