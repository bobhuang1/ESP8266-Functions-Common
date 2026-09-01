# MathQuizGenerator

Generates a random arithmetic question and its worked answer, for
math-practice toy projects such as
[Nano-MathToyMix100-12864](https://github.com/bobhuang1/Nano-MathToyMix100-12864)
and [ESP12-MathToy-SPI-New12864](https://github.com/bobhuang1/ESP12-MathToy-SPI-New12864).

## Usage

```cpp
#include <MathQuizGenerator.h>

void setup() {
  randomSeed(analogRead(A5)); // seed once, from an unconnected analog pin
}

String answer;
String question = generateMathQuestion(answer);              // default: ceiling 100, no division
String question2 = generateMathQuestion(answer, 10000, true); // higher ceiling, includes division
```

`question` is the display-ready prompt (e.g. `"12+7X3=?"`); `answer` is
filled in with the same expression fully worked out (e.g. `"12+7X3=33"`).
