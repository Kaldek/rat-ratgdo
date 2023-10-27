## Prototype notes for powering ESP module with red wire
We are testing use of an LM2596 voltage regulator (buck converter) module to power the ESP without needing a separate USB power supply.

### Overview of module setup
One of the many sources for pre-built LM2596 voltage regulator modules will be sourced, either as unit pre-set to 3.3v output or an adjustable unit that has been set to output 3.3v.
The existing input capacitor (usually rated at 100uF) will be replaced with a low-ESR 1000uF capacitor.

Pinouts are planned as follows:
| LM2596 Module Pin | GDO wire |
| ----------------- | -------- |
| +Input           | Red Wire |
| -Input           | White Wire |
| +Output (3.3v)   | ESP 3V3 |
| -Output          | White Wire |

The ground pins of the input and output side of the module will be connected to the GND (white) wire of the Garage Door Opener (GDO).  The + Input pin of the module will be connected to the red wire of the GDO.

### Why a 1000uF capacitor?
Serial data transmission at 9600 bits per second pulls the 12v line low for 104µs per bit.  We have not yet investigate the "common" data transmission sizes, although this is likely documented in the ratgdo source code, and are placing a capacitor large enough to deal with the following considerations at times when the +12v line is being pulled low:
- Increases in power consumption during WiFi transmission
- Increases in power consumption during its own serial transmission
- Increases in power consumption during processing of received serial data
- Higher power consumption of ESP-32 modules
