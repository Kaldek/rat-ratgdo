## ESP8266 D1 Mini boards
All ESP8266-based D1 Mini boards are supported, and are the hardware which the ratgdo project uses for the pre-made kits available from Paul Wieland.

![D1 Mini Board](https://github.com/Kaldek/rat-ratgdo/blob/main/D1%20Mini%20board.jpg)

![ESP8266 D1 Mini Schematic](https://github.com/Kaldek/rat-ratgdo/blob/main/ESP8266%20D1%20Mini%20schematic%20v1.png)

## ESP-32 based boards
All ESP-32 boards are directly supported by the ESPHome Ratgdo port, using the [ESP32 D1 Mini YAML file for install settings for a version 2.5 ratgdo board](https://github.com/ratgdo/esphome-ratgdo/blob/v25/static/v25board_esp32_d1_mini.yaml).

**Note:** The pin numbers mentioned in the linked YAML file are only directly valid for the D1 Mini ESP32 boards.  Regardless of ESP-32 board used, these are the **actual** ESP-32 pins used:
| ratgdo function | ESP-32 pin |
| --------------- | ---------- |
| TX              | GPIO22     |
| RX              | GPIO21     |
| Obstruction     | GPIO23     |

For your particular board, you may need to determine which board traces map to these GPIO pins.


### ESP-32 D1 Mini boards
Supported by the ESPHome Ratgdo port, using the ESP32 D1 Mini YAML file for install settings, noting thepinout mappings below..  Note that the outer row of pins on the D1 Mini board are not used for ratgdo.

![ESP-32 D1 Mini Front view](https://github.com/Kaldek/rat-ratgdo/blob/main/ESP32%20D1%20Mini%20board-front.png)
![ESP-31 D1 Mini back view](https://github.com/Kaldek/rat-ratgdo/blob/main/ESP32%20D1%20Mini%20board-back.jpg)

GPIO pins for the ESP-32 D1 Mini are as follows:
- TX: IO22
- RX: IO21
- Obstruction: IO23

### ESP-WROOM-32
Supported by the ESPHome Ratgdo port, using the ESP32 D1 Mini YAML file for install settings, noting thepinout mappings below.

![ESP-WROOM-32 board](https://github.com/Kaldek/rat-ratgdo/blob/main/ESP-WROOM-32%20board.jpg)

GPIO pins for the ESP-WROOM-32 are as follows:
- TX: D22
- RX: D21
- Obstruction: D23
