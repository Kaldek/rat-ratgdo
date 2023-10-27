# rat-ratgdo
RAGE
AGAINST
THE
RATGDO

Credit and inspiration for this work goes to [@rlowens](https://github.com/rlowens) to let me know that there are other technical folks out there who needed this other than myself.

## Overview
This is an open source schematic for the ratgdo, based on the v2.5 [ratgdo](https://github.com/PaulWieland/ratgdo) PCB and code.  We feel there is cognitive dissonance between the refusal to provide (and deleting requests for) schematics for the ratgdo PCB against the ratgdo project's use of open source code, open source licensing, and libraries used by other open source projects, as well as the inception reasoning for the ratgdo project which was to ensure that nobody is at the mercy of a third party.

This project was created to allow people who need a ratgdo solution for their garage door opener but either cannot, or choose not to, source the solution from Paul Wieland and be locked into that 3rd party. It also ensures that the PCB can be recreated as and when the project maintainer gives up on the project (much like Chamberlain themselves might do with their MyQ Cloud Service).

You are expected to know how to create the described circuit using your own prototyping skills.  That is, this project is for people who can solder well and know how to read schematics.

The PCB schematic here does not describe any circuitry other than the serial line garage door control and obstruction sensor.  All other optional features provided by ratgdo could also be reverse engineered as needed.

**If you want to buy a pre-made ratgdo setup, purchase it [from Paul Wieland](https://github.com/PaulWieland/ratgdo).**

![PCB Link](https://github.com/Kaldek/rat-ratgdo/blob/main/ratgdo%20open%20source_schem_v6.png)

See images of a [working breadboard prototype](https://github.com/Kaldek/rat-ratgdo/blob/main/Breadboard_working.png) and the [subsequent soldered prototype using a D1 shield](https://github.com/Kaldek/rat-ratgdo/blob/main/Simple%20prototype%20using%20D1%20shield.jpg).  Both of these prototypes are using 2n7000 MOSFETs exclusively, but that is because they are prototypes and long term reliability of data transmission when using the 2n7000 has not been confirmed.  See the section below on needed components for context.

## Components needed
This schematic assumes the use of through-hole components.  If you wish to use SMD components, please refer to the [FAQ](https://github.com/Kaldek/rat-ratgdo/blob/main/FAQ.md#what-if-i-want-to-use-sot-23-smd-components).

Aside from the obvious requirement of a [supported ESP-32 or ESP8266 board](https://github.com/Kaldek/rat-ratgdo/blob/main/Supported%20Boards.md) flashed with the native ratgdo or the [ESPHome version](https://github.com/ratgdo/esphome-ratgdo), you will need 3x 10 kohm (kilo-ohm) resistors, one 2n7000 N-channel MOSFET (TO-92 package is easiest) and one IRLB8721 MOSFET (likely only available in TO-220 package).  It is also recommended you aquire a suitable 3-post screw terminal and some red, white, and black wire for connecting to the door opener.  These connections are very low current, so you can get away with fairly thin wire.

**Note:** If you cannot source an IRLB8721 MOSFET, you can use another 2n7000 MOSFET, however this is borderline for switching using 3.3v logic.

**WARNING!** This schematic alone will not work with a "raw" ESP8266 without the D1 mini board.  Certain models of ESP8266 will not boot without some pullup resistors.  This is not to say that we can't add these distinctions and clarifications in future (e.g., which ESP model to buy such as ESP-12S vs ESP-12F) but for now, this project lacks that guidance.


## INSTALL REQUIREMENTS
Due the schematics we have provided and how they are laid out, you must use the ESPHome fork of ratgdo and select the blue v2.5 board on the ESPhome [web installer page](https://ratgdo.github.io/esphome-ratgdo/).  For ESP-32 based boards the above does not currently apply and you should use the v2.0 installer and follow our ESP-32 schematic we provide in the Supported Boards page.

For the ESP8266 based boards, if installing using Paul Wieland's native ratgdo installer and you install for a v2.0 board, you must wire your TX to D4 (GPIO2) rather than D1 (GPIO5). 

## How the ratgdo circuit works
The ratgdo circuitry which we cover in this project consists of two sections; Garage Door control and obstruction sensors.

### Garage Door Control
#### Description of the Chamberlain configuration
The Chamberlain garage door control circuit is a two-wire setup consisting of a Ground, and a combined +12v power and serial data line.  The serial data line is held at 12v unless data transmission is occurring.  Data transmission is performed by pulling the 12v line down to GND for each data bit.  The benefit here is that a single wire can be used for providing power to door control panels and data transmssion on a single wire.  The use of appropriately sized capacitors in door control panels means they do not switch off during data transmission.  

#### How ratgdo interfaces with the Chamberlain wiring
The ratgdo taps into the red control wire and uses this single wire for transmission of commands onto the serial bus, and for receiving data from the serial bus.  As it is a single wire protocol and operates half-duplex, the ratgdo circuitry uses N-channel MOSFETs wired in a manner which allows the ESP8266 to never transmit when the serial line is already low.  

In operation, this means that the ratgdo is mostly listening to the serial bus to ensure it is keeping track of the state of the door.  For example, if the door is manually opened (or opened by use of the MyQ app even), these events are broadcast onto the serial bus by the door controller.  ratgdo is therefore able to know that the state of the door has changed.  In turn, this means that when controlling ratgdo via Home Assistant, the status of the door in HA is always up to date.


### Obstruction sensor
The ratgdo monitors the real-time state of the obstruction sensors directly.  We assume this is because obstruction status is not broadcast in real time by the door controller, and safety is paramount.  It allows you to set up real-time alerts in Home Assistant for any time the door is obstructed.  The ratgdo taps into the obstruction sensor power wire which also acts as a communication bus, however the signalling is only an ongoing status of whether the obstruction sensors are working and whether there is an obstruction. 

