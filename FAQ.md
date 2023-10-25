# FAQ

## Low Level Circuitry stuff
### How come the 12 volt serial line doesn't blow the ESP8266 GPIO lines?
It's fairly simple how this operates.  The really short version is that the MOSFETs are used as level shifters. 

The more complex description is that the TX line 3.3v output is used to trigger the Gate of a MOSFET which grounds out the serial line, pulling it LOW.  The circuit being closed is between the +12V serial line and GND.  The GPIO pin therefore never sees 12 volts.  

For the RX line, the Gate of the MOSFET on this part of the circuit is connected to the +12V serial line.  Whenever +12v is applied to the gate, it causes the +3.3v from the ESP8266 GPIO pin to be pulled to ground.  In this example, the voltage "push" (if you want to call it that) is going **from** the ESP8266 to the MOSFET and it is then switched to GND.  GPIO pins on microcontrollers are designed to handle this "pull to ground" as a means of signalling/logic control, so there's no risk of the ESP8266 cooking itself.  An important note to add to the RX circuitry is that when the door controller is transmitting data on the serial line, it is pulling the +12V line to GND.  When the +12V line is pulled to ground, this switches off the MOSFET used in the RX circuit and therefore the GPIO pin on the ESP8266 goes high.

So, regardless of what device is transmitting on the serial line (whether that is the ESP8266, the door opener itself, or a door control panel, the transmitting device pulls the +12v line to ground.

### What bitrate is the MyQ serial communication?
As per the code from the ratgdo project, it is 9600 bits per second.

### Could I power the ESP8266 from the +12v serial line?
Absolutely you can try this; this is how the MyQ wall controllers work.  All you need is suitable voltage regulation from 12v to 3.3v and big enough capacitors to keep the ESP8266 powered when the 12v line is being pulled low during serial transmission pulses.

We cannot speak for the maximum current capability of the +12v line, however it is likely to be high enough to power one door control panel and the ESP8266.  If you have additional wall control panels, you may exceed the capabilities of the +12v power which is available.

### What if I want to use SOT-23 SMD components?
If you want to use SOT-23 SMD components rather than through-hole, the following MOSFETs are recommended:
- RX line: 2N7002
- TX line: AO3400A
