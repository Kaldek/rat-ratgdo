# rat-ratgdo
RAGE
AGAINST
THE
RATGDO

## Overview
This is an open source schematic for the ratgdo PCB as developed by Paul Wieland.  We feel there is cognitive dissonance between the refusal to provide schematics for the ratgdo PCB against the ratgdo project's use of open source code, open source licensing, and libraries used by other open source projects.

This project was created to allow people who need a ratgdo solution for their garage door opener but either cannot, or choose not to, source the solution from Paul Wieland. It also ensures that the PCB can be accessible as and when the project maintainer gives up on the project (much like Chamberlain themselves might do with their MyQ Cloud Service).

You are expected to know how to create the described circuit using your own prototyping skills.  That is, this project is for people who can solder well and know how to read schematics.

The PCB schematic here does not describe any circuitry other than the serial line garage door control and obstruction sensor.  All other optional features provided by ratgdo could also be reverse engineered as needed.

**If you want to buy a pre-made ratgdo setup, purchase it [from Paul Wieland](https://github.com/PaulWieland/ratgdo).**

![PCB Link](https://github.com/Kaldek/rat-ratgdo/blob/main/ratgdo%20open%20source_schem.png)


## How the ratgdo circuit works
The ratgdo circuitry which we cover in this project consists of two sections; Garage Door control and obstruction sensors.

### Garage Door Control
#### Description of the Chamberlain configuration
The Chamberlain garage door control circuit is a two-wire setup consisting of a Ground, and a combined +12v power and serial data line.  The serial data line is held at 12v unless data transmission is occurring.  Data transmission is performed by pulling the 12v line down to GND for each data bit.  The benefit here is that a single wire can be used for providing power to door control panels and data transmssion on a single wire.  The use of appropriately sized capacitors in door control panels means they do not switch off during data transmission.  

#### How ratgdo interfaces with the Chamberlain wiring

