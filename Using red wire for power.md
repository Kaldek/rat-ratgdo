# Prototype notes for powering ESP module
We are testing use of an LM2596 voltage regulator (buck converter) module to power the ESP without needing a separate USB power supply.  

## Can we use the red wire to power the ESP?
Testing using this method has failed.  The LM2596 and ESP8266 combined draw too much current from the red wire for the ESP to be powered from the E-Serial lines of the Chamberlain/MyQ GDO. 

## Using the GDO Battery Backup connector to power the ratgdo
GDO models that are compatible with the Standby Battery Power System (e.g., the PN 475LM, [disassembly video](https://www.youtube.com/watch?v=qWsHb-kiO6w)) or have one built-in, can use the same 2-pin connector to power their LM2596 module.  Just be sure that you first adjust the output of your LM2596 voltage regulator to 5v **before** you connect it to your ESP **module**.  More information on the connector can be found in [this thread](https://www.garagejournal.com/forum/threads/battery-backuo-connector-for-liftmaster-8500-garage-door-opener.514321/).

### Overview of module setup
One of the many common pre-built LM2596 voltage regulator modules can be used, either as unit pre-set to 5v output or an adjustable unit that has been set to output 5 volts.

![LM2596 module example](https://github.com/Kaldek/rat-ratgdo/blob/main/images/LM2596%20module.jpg)

### Pinouts

Pinouts are planned as follows for using the Battery Backup terminals on the GDO:
| LM2596 Module Pin | Garage Door Opener wires         |
| :---------------- | :------------------------------- |
| IN+               | Battery Backup Positive terminal |
| IN-               | Battery Backup Negative terminal |
| OUT+  (5v)        | ESP Module 5v                    |
| OUT-              | ESP Module GND                   |

#### Why 5v output and not 3.3v?
If using an ESP ***module*** these have an onboard 5v to 3.3v voltage regulator and do not expose a 3.3v "in" on their connectors.  3.3v would only be used if you are powering a "naked" ESP8266.
