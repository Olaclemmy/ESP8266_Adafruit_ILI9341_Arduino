## Credits
Arduino-compatible fork of Sermus's non-Arduino port of modified Adafruit library for ILI9341-based TFT displays.

You may be wondering: why use this instead of mainline Adafruit_ILI9341? Easy answer: @Sermus's hardware SPI code runs like greased lightning on ESP8266. Very very fast.

Sermus's non-Arduino port: https://github.com/Sermus/ESP8266_Adafruit_ILI9341

Original Adafruit Arduino library: https://github.com/adafruit/Adafruit_ILI9341

The library is extended with text rendering routines got from here: http://www.instructables.com/id/Arduino-TFT-display-and-font-library/?ALLSTEPS

Original hspi code: https://github.com/Perfer/esp8266_ili9341

## SPI staff
In spite of the fact that according to the datasheet max ILI9341's clock speed is 10MHz mine robustly works at up to 40MHz so there is a SPI speed prescaler macro at the beginning of hspi.c.
Defining it to 1 means HSPI will be clocked at 40MHz, 4 means 10 MHz.

## Sample code
There's no sample code yet. ;_;

## Wiring
The code uses hardware HSPI with hardware controlled CS, so the wiring shall be as follows: 

ILI9341 pin -->	ESP8266 pin

MOSI 	-->	GPIO12 (nodeMCU: D7)

CLK 	-->	GPIO14 (nodeMCU: D5/CLK)

CS 	-->	GPIO15 (nodeMCU: D8)

DC 	-->	GPIO2 (nodeMCU: D4)

Reset to 3.3v

GND to ground

MISO may be left unconnected


