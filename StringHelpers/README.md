# StringHelpers

`char* string2char(String command)` - converts an Arduino `String` to the
`char*` many display/graphics libraries (e.g. u8g2's `drawStr`/`getStrWidth`)
and `Serial`/`WiFi` APIs expect.

## Usage

```cpp
#include <StringHelpers.h>

display.drawStr(0, 0, string2char(myString));
```

The returned pointer aliases `command`'s internal buffer - use it immediately
rather than storing it, since it becomes invalid once `command` is modified
or goes out of scope (this is a property of `String::c_str()` itself, not
specific to this wrapper).
