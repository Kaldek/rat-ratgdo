# FAQ

## Low Level Circuitry stuff
### How does the "Red Wire" on these Garage Door Openers work?
Each Garage Door Opener has two wires which connect between the GDO and any wired door control panels (the smart panels with displays and menu controls on them).  These are a +12v wire and a Ground so that DC power is provided to the control panel.

https://github.com/Kaldek/rat-ratgdo/blob/main/GDO%20wiring%20pinout%20example.png

However, that same +12v wire is **also** the data line which carries data between the GDO and the control panel.  This works because each serial data pulse is a brief "pull to ground" of that +12v line.  These pulses can be read by the door control panel (and, indeed read by the GDO if the data transmission is coming from the control panel and being sent to control the door).  This is how the ratgdo both receives and transmits data as well.  In essence, it appears to be just one more control device on the "bus".

### If the +12V line is constantly pulsed low during data transmission, how come the door control panel doesn't turn off?
There's two answers to this and both are simple:
- The data transmission is actually quite rare
- The door control panel either has some beefy capacitors in it or a supercapacitor.  Either way, the brief interruptions to power do not affect the control panel.

### How come the 12 volt serial line doesn't blow the ESP8266 GPIO lines?
It's fairly simple how this operates.  The really short version is that the MOSFETs are used as level shifters. 

The more complex description is that the TX line 3.3v output is used to trigger the Gate of a MOSFET which grounds out the serial line, pulling it LOW.  The circuit being closed is between the +12V serial line and GND.  The GPIO pin therefore never sees 12 volts.  

For the RX line, the Gate of the MOSFET on this part of the circuit is connected to the +12V serial line.  Whenever +12v is applied to the gate, it causes the +3.3v from the ESP8266 GPIO pin to be pulled to ground.  In this example, the voltage "push" (if you want to call it that) is going **from** the ESP8266 to the MOSFET and it is then switched to GND.  GPIO pins on microcontrollers are designed to handle this "pull to ground" as a means of signalling/logic control, so there's no risk of the ESP8266 cooking itself.  An important note to add to the RX circuitry is that when the door controller is transmitting data on the serial line, it is pulling the +12V line to GND.  When the +12V line is pulled to ground, this switches off the MOSFET used in the RX circuit and therefore the GPIO pin on the ESP8266 goes high.

So, regardless of what device is transmitting on the serial line (whether that is the ESP8266, the door opener itself, or a door control panel, the transmitting device pulls the +12v line to ground.

### Why are the two MOSFETs different?
For the RX line where the gate is driven by a 12v signal, the 2n7000 is appropriate.  For the TX line where the gate is driven by a 3.3v signal, the 2n7000 is borderline as it needs 3v to fully switch and 3.3v is risky.  For through-hole designs, the IRLB8721 is our recommended part as it only needs 2.5 volts to fully switch, and the 3.3 volt signal from the ESP8266 is therefore plenty.  See below if you're looking to use SMD parts.

### How come the obstruction sensor circuit uses two resistors?
The obstruction sensor circuit uses two 10k resistors, one in series and one in parallel to ground.  This causes the resistors to act as a voltage divider, bringing the voltage "seen" on pin D7 (GPIO13) down to ~3.3 volts. 

### What bitrate is the MyQ serial communication?
As per the code from the ratgdo project, it is 9600 bits per second.

### Could I power the ESP8266 from the +12v serial line?
Absolutely you can try this; this is how the MyQ wall controllers work.  All you need is suitable voltage regulation from 12v to 3.3v and big enough capacitors to keep the ESP8266 powered when the 12v line is being pulled low during serial transmission pulses.

We cannot speak for the maximum current capability of the +12v line, however it is likely to be high enough to power one door control panel and the ESP8266.  If you have additional wall control panels, you may exceed the capabilities of the +12v power which is available.

### What if I want to use SOT-23 SMD components?
If you want to use SOT-23 SMD components rather than through-hole, the following MOSFETs are recommended:
- RX line: 2N7002
- TX line: AO3400A

For resistors, knock yourself out.  Any size you like as long as it's 10 kohm.
