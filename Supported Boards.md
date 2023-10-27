## ESP8266 D1 Mini boards
All ESP8266-based D1 Mini boards are supported, and are the hardware which the ratgdo project uses for the pre-made kits available from Paul Wieland.

![D1 Mini Board](https://github.com/Kaldek/rat-ratgdo/blob/main/D1%20Mini%20board.jpg)

![ESP8266 D1 Mini Schematic](https://github.com/Kaldek/rat-ratgdo/blob/main/ESP8266%20D1%20Mini%20schematic%20v1.png)

## ESP-32 based boards
All ESP-32 boards are directly supported by the ESPHome Ratgdo port, using the [ESP32 D1 Mini YAML file for install settings for a version 2.5 ratgdo board](https://github.com/ratgdo/esphome-ratgdo/blob/v25/static/v25board_esp32_d1_mini.yaml).

### ESP-32 D1 Mini boards
Directly supported and uses the marked D1, D2, and D7 pins.  Note that the outer row of pins on the D1 Mini board are not connected or used

### ESP-WROOM-32
Supported by the ESPHome Ratgdo port, using the ESP32 D1 Mini YAML file for install settings, noting that the pinouts are custom.

![ESP-WROOM-32 board](https://github.com/Kaldek/rat-ratgdo/blob/main/ESP-WROOM-32%20board.jpg)

GPIO pins are as follows
- TX: GPIO22 (might be marked as D22)
- RX: GPIO21 (might be marked as D21
- OBST: GPIO23 (might be marked as D23
