## Prototype notes for powering ESP module with red wire
We are testing use of an LM2596 voltage regulator (buck converter) module to power the ESP without needing a separate USB power supply.  The MyQ wired control panels source their power this way, so it should be also possible to do this for the ratgdo ESP module.

**NOTE:** *Testing so far has failed.  The LM2596 and ESP8266 combined draw too much current from the +12v line for the ESP to be powered from the E-Serial lines of the Chamberlain/MyQ GDO.  However, if your GDO has the backup battery or a backup battery connector, this can be used to power the LM2596 and the ESP.*

### Overview of module setup
One of the many common pre-built LM2596 voltage regulator modules will be sourced, either as unit pre-set to 5v output or an adjustable unit that has been set to output 5 volts.

![LM2596 module example](https://github.com/Kaldek/rat-ratgdo/blob/main/images/LM2596%20module.jpg)

### Pinouts

Pinouts are planned as follows for using the Battery Backup terminals on the GDO:
| LM2596 Module Pin | Gaeage Door Opener wires         |
| ----------------- | -------------------------------- |
| IN+               | Battery Backup Positive terminal |
| IN-               | Battery Backup Negative terminal |
| OUT+  (5v)        | ESP Module 5v                    |
| OUT-              | ESP Module GND                   |

#### Why 5v output and not 3.3v?
If using an ESP ***module*** these have an onboard 5v to 3.3v voltage regulator and do not expose a 3.3v "in" on their connectors.  3.3v would only be used if you are powering a "naked" ESP8266.

# Alternative if you have an opener that supports battery backup
Models that are compatible with the Standby Battery Power System (PN 475LM, [disassembly video](https://www.youtube.com/watch?v=qWsHb-kiO6w)) can use the same 2-pin connector to power their LM2596 module.  Just be sure that you first adjust the output to 5v or 3v3 **before** you connect it to your ESP.  More information on the connector can be found in [this thread](https://www.garagejournal.com/forum/threads/battery-backuo-connector-for-liftmaster-8500-garage-door-opener.514321/).  

~~### Input Capacitor Replacement
The input capacitor of the voltage regulator module (usually rated 100uF) will be replaced with a 1000uF Low-ESR capacitor.
*This is not needed if you are pulling 12v power from the backup battery connector*~~

~~#### Why a 1000uF capacitor?
Serial data transmission at 9600 bits per second pulls the 12v line low for 104µs per bit.  We have not yet investigated the "common" data transmission sizes, although this is likely documented in the ratgdo source code, and are placing a capacitor large enough to deal with the following considerations at times when the +12v line is being pulled low:~~
~~- Extended data transmission~~
~~- Increases in power consumption during WiFi transmission~~
~~- Increases in power consumption during its own serial transmission~~
~~- Increases in power consumption during processing of received serial data~~
~~- Higher power consumption of ESP-32 modules for all of the above scenarios~~



