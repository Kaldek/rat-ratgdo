# Why we recommend ESPHome
Paul Wieland is the original creator of the ratgdo project and deserves all credit for its creation. However, when using custom schematics and PCBs, the ESPHome fork of ratgdo is more suitable.

The benefits of using ESPHome are:
- Web interface provides better out of the box diagnostic and logging
- Pins used for ratgdo functions can be altered without coding expertise
- ESPHome automatic discovery

## Web interface benefits
The ESPHome web interface provides full details on a single web page of all current states of the ratgdo data in real time.  It also provides a realtime console log and, if you access the web page early enought in the boot cycle of the ESP8266, you can see the hardware settings that have been applied.

[Example of ESPHome Web GUI](https://github.com/Kaldek/rat-ratgdo/blob/main/images/ESPHome%20Web%20Gui%20example.png)

## Custom Pins for ratgdo functions
The ESPHome fork of ratgdo allows you to edit configuration settings for the ratgdo device in a YAML file on your Home Assistant (HA) server.  This includes the GPIO pins that will be used for obstruction detection, TX, RX, and the dry contacts.  ESPHome will then perform an on-the-fly recompile of the ESPHome and ratgdo binary and push these settings to the ESP module **Over The Air**.

Note that this requires installation of the ESPHome Add-On to Home Assistant.

## ESPHome Automatic Discovery
Any devices on the network using ESPHome will advertise their presence on the local network.  This **includes** using Multicast DNS (MDNS) which means that if you have a suitable MDNS forwarder (such as the [avahi deaemon](https://www.avahi.org/) at https://www.avahi.org/) and your ratgdo is on a separate IP subnet, it will still be detected by Home Assistant.  Handy for those of us who segment our "IoT" networks.

ESPHome Automatic Discovery and registration into Home Assistant does not require the ESPHome Add-On.  The Add-On is required for advanced functions like the customised YAML files as mentioned above.

Automatic regestration also correctly registers the ratgdo in Home Assistant as a garage door without any further work.
