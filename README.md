## What's This?
Arduino-compatible fork of Sermus's non-Arduino port of modified Adafruit Arduino library for ILI9341-based TFT displays.

You may be wondering: why use this instead of mainline Adafruit_ILI9341? Easy answer: @Sermus's hardware HSPI code runs like greased lightning on ESP8266. Very very fast.

It's about **7.5x faster for pixel fills** (rectangles, screen clears, etc.) and about **3.5x faster for text** than the mainline Adafruit_ILI9341 on a 160MHz ESP8266.

This library has re-added some standard Adafruit_GFX functions, such as text handling.

## Usage

Include the _AS versions of Adafruit_ILI9341 and Adafruit_GFX in your sketch:

```
#include "Adafruit_GFX_AS.h"
#include "Adafruit_ILI9341_fast_as.h"
```

Then use this constructor:
`Adafruit_ILI9341 tft = Adafruit_ILI9341();
`

You must use these pins:


## Wiring

The code uses hardware HSPI with hardware controlled CS, so this wiring must be used:

ILI9341 pin -->	ESP8266 pin

MOSI 	-->	GPIO12 (nodeMCU: D7)

CLK 	-->	GPIO14 (nodeMCU: D5/CLK)

CS 	-->	GPIO15 (nodeMCU: D8)

DC 	-->	GPIO2 (nodeMCU: D4)

Reset to 3.3v

GND to ground

MISO may be left unconnected




## Caveats

* The read commands are not implemented. If you need to read the status registers from the LCD, the regular [Adafruit_ILI9341](https://github.com/adafruit/Adafruit_ILI9341) may be the best bet.
* Have not tested fonts. Give it a try and let me know how it goes.
* Your LCD may not be compatible with the default speed; see ["SPI speed"](#spi-speed) below.
* It will fail for non-ESP8266 environments.
* You must use the hardware SPI pins.
* Only tested with Arduino 1.6.9 and ESP8266 core 2.3.0.

## SPI speed

The datasheet says that the ILI9341's maximum clock speed is 10MHz, but despite this, they generally work at 40MHz. 

You may need to change it for your screen, so there is a SPI speed prescaler macro at the beginning of `hspi.c`.

Defining it to 1 means HSPI will be clocked at 40MHz; 2 means 20MHz; 4 means 10 MHz.


## Benchmark

Here are the results I get from `graphicstest` at 160MHz CPU, 40MHz SPI with a NodeMCU:

```
Benchmark                Time (microseconds)
Screen fill              173165
Text                     23050
Lines                    230294
Horiz/Vert Lines         14768
Rectangles (outline)     9918
Rectangles (filled)      359530
Circles (filled)         81641
Circles (outline)        100070
Triangles (outline)      72978
Triangles (filled)       133827
Rounded rects (outline)  36742
Rounded rects (filled)   401457
Done!```
and here's the stock Adafruit_ILI9341 on the same 160MHz NodeMCU:
```Benchmark                Time (microseconds)
Screen fill              1329932
Text                     92785
Lines                    921902
Horiz/Vert Lines         109557
Rectangles (outline)     70771
Rectangles (filled)      2761868
Circles (filled)         447556
Circles (outline)        402770
Triangles (outline)      292408
Triangles (filled)       930711
Rounded rects (outline)  169561
Rounded rects (filled)   3030970
Done!
```


## Credits

Sermus's non-Arduino port: [https://github.com/Sermus/ESP8266_Adafruit_ILI9341](https://github.com/Sermus/ESP8266_Adafruit_ILI9341)

Original Adafruit Arduino library: [https://github.com/adafruit/Adafruit_ILI9341](https://github.com/adafruit/Adafruit_ILI9341)

The library is extended with text rendering routines got from here: [http://www.instructables.com/id/Arduino-TFT-display-and-font-library/?ALLSTEPS](http://www.instructables.com/id/Arduino-TFT-display-and-font-library/?ALLSTEPS)

Original `hspi` code: [https://github.com/Perfer/esp8266_ili9341](https://github.com/Perfer/esp8266_ili9341)
