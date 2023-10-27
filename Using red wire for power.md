## Prototype notes for powering ESP module with red wire
We are testing use of an LM2596 voltage regulator (buck converter) module to power the ESP without needing a separate USB power supply.

### Overview of module setup
One of the many common pre-built LM2596 voltage regulator modules will be sourced, either as unit pre-set to 3.3v output or an adjustable unit that has been set to output 3.3v.

![LM2596 module example](https://github.com/Kaldek/rat-ratgdo/blob/main/LM2596%20module.jpg)

### Pinouts

Pinouts are planned as follows:
| LM2596 Module Pin | GDO wire |
| ----------------- | -------- |
| IN+         | Red Wire |
| IN-           | White Wire |
| OUT+ (3.3v)   | ESP 3V3 |
| OUT-          | White Wire |

### Input Capacitor Replacement
The input capacitor of the voltage regulator module (usually rated 100uF) will be replaced with a 1000uF Low-ESR capacitor.

#### Why a 1000uF capacitor?
Serial data transmission at 9600 bits per second pulls the 12v line low for 104µs per bit.  We have not yet investigated the "common" data transmission sizes, although this is likely documented in the ratgdo source code, and are placing a capacitor large enough to deal with the following considerations at times when the +12v line is being pulled low:
- Extended data transmission
- Increases in power consumption during WiFi transmission
- Increases in power consumption during its own serial transmission
- Increases in power consumption during processing of received serial data
- Higher power consumption of ESP-32 modules
