# rat-ratgdo
RAGE
AGAINST
THE
RATGDO

## Overview
This is an open source schematic for the ratgdo PCB as developed by Paul Wieland.  We feel there is cognitive dissonance between the refusal to provide schematics for the ratgdo PCB against the ratgdo project's use of open source code, open source licensing, and libraries used by other open source projects, as well as the inception for the ratgdo project which was to ensure that nobody is at the mercy of a third party.

This project was created to allow people who need a ratgdo solution for their garage door opener but either cannot, or choose not to, source the solution from Paul Wieland and be locked into that 3rd party. It also ensures that the PCB can be recreated as and when the project maintainer gives up on the project (much like Chamberlain themselves might do with their MyQ Cloud Service).

You are expected to know how to create the described circuit using your own prototyping skills.  That is, this project is for people who can solder well and know how to read schematics.

The PCB schematic here does not describe any circuitry other than the serial line garage door control and obstruction sensor.  All other optional features provided by ratgdo could also be reverse engineered as needed.

**If you want to buy a pre-made ratgdo setup, purchase it [from Paul Wieland](https://github.com/PaulWieland/ratgdo).**

![PCB Link](https://github.com/Kaldek/rat-ratgdo/blob/main/ratgdo%20open%20source_schem.png)

## Components needed
Aside from the obvious requirement of an ESP8266 flashed with the native ratgdo or the [ESPHome version](https://github.com/ratgdo/esphome-ratgdo), you will need 2x 10Kohm (kilo-ohm) resistors and 2x 2n7000 N-channel MOSFETs.  It is also recommended you aquire a suitable 3-post screw terminal and some red, white, and black wire for connecting to the door opener.  These connections are very low current, so you can get away with fairly thin wire.


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

