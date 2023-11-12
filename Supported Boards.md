## ESP8266 D1 Mini boards
All ESP8266-based D1 Mini boards are supported, and are the hardware which the ratgdo project uses for the pre-made kits available from Paul Wieland.

![D1 Mini Board](https://github.com/Kaldek/rat-ratgdo/blob/main/images/D1%20Mini%20board.jpg)

![ESP8266 D1 Mini Schematic](https://github.com/Kaldek/rat-ratgdo/blob/main/schematics/ratgdo%20open%20source%20D1%20Mini_schem_v6.png)

Note that the D1 Mini **module** takes 5v and an on-board regulator drops this down to the 3.3v power used by the ESP8266 chip.

## ESP-32 based boards
All ESP-32 boards are directly supported by the ESPHome Ratgdo port, using the [ESP32 D1 Mini YAML file for install settings for a version 2.0 ratgdo board](https://github.com/ratgdo/esphome-ratgdo/blob/main/static/v2board_esp32_d1_mini.yaml).

**CRITICAL**: You must install using the v2.0 install for ESP-32 boards as v2.5 is additional and unnecessary work for these boards at the current time.

**Note:** The pin numbers mentioned in the linked YAML file are only directly valid for the D1 Mini ESP32 boards.  Regardless of ESP-32 board used, these are the **actual** ESP-32 pins used for v2.0:
| ratgdo function | ESP-32 pin |
| --------------- | ---------- |
| TX              | GPIO16     |
| RX              | GPIO21     |
| Obstruction     | GPIO23     |
| White/GND       | GND        |

For your particular board, you may need to determine which board traces map to these GPIO pins.

![Generic ESP-32 board pinout](https://github.com/Kaldek/rat-ratgdo/blob/main/schematics/ratgdo%20open%20source%20ESP-32_schem_v5.png)

Note that ESP-32 **modules** take 5v power from USB and an on-board regulator drops this down to the 3.3v used by the ESP-32 chip.

### ESP-32 D1 Mini boards
Supported by the ESPHome Ratgdo port, using the ESP32 D1 Mini YAML file for install settings, noting thepinout mappings below..  Note that the outer row of pins on the D1 Mini board are not used for ratgdo.

![ESP-32 D1 Mini Front view](https://github.com/Kaldek/rat-ratgdo/blob/main/images/ESP32%20D1%20Mini%20board-front.png)
![ESP-31 D1 Mini back view](https://github.com/Kaldek/rat-ratgdo/blob/main/images/ESP32%20D1%20Mini%20board-back.jpg)

GPIO pins for the ESP-32 D1 Mini are as follows:
- TX: IO16
- RX: IO21
- Obstruction: IO23

### ESP-WROOM-32
Supported by the ESPHome Ratgdo port, using the ESP32 D1 Mini YAML file for install settings, noting thepinout mappings below.

![ESP-WROOM-32 board](https://github.com/Kaldek/rat-ratgdo/blob/main/images/ESP-WROOM-32%20board.jpg)

GPIO pins for the ESP-WROOM-32 are as follows:
- TX: Find what maps to GPIO16
- RX: D21
- Obstruction: D23
