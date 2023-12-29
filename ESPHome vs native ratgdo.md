# Why we recommend ESPHome
Paul Wieland is the original creator of the ratgdo project and deserves all credit for its creation. However, when using custom schematics and PCBs, the ESPHome fork of ratgdo is more suitable.

The benefits of using ESPHome are:
- Web interface provides better out of the box diagnostic and logging
- Pins used for ratgdo functions can be altered without coding expertise
- ESPHome automatic discovery
- Easy support for 4MB Flash ESP8266 modules, allowing for OTA updates via ESPHome Add-On in HA

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

## ESPHome OTA support for 4MB flash modules
The default guidance is to use the ESPHome ratgdo YAML files for the Wemos D1 Mini Lite.  As a result, ESPHome will assume your device has 1MB Flash even if it has 4MB (e.g., Wemos D1 Mini has 4MB Flash whereas the Wemos D1 Mini Lite only has 1MB).

ESPHome added support for modules with 4MB flash via support for the 2.5i version of Paul Wieland's ratgdo product, which includes an ESP-12F module that has 4MB flash.  If your module has 4MB flash, just select the v2.5i board when installing ESPHome.

### Initial re-flash after updating YAML
If you are re-flashging an existing deployment of ESPHome-ratgdo for your module with 4MB flash, you will initially need to re-flash your module either using USB or via stripping out all of the ratgdo references from the YAML so that the OTA file fits in the remaining spare flash space.  This is because that the change to your YAML only effectively applies after one successful installation of the YAML with the reference to the d1_mini.

## ESPHome OTA support for 2MB flash modules
Currently the esphome-ratgdo repository does not directly support configurations for ESP8266 modules with 2MB flash (such as the Tuya TYWE3L and TYWE3S), as the list of modules supported in their provided YAML files reference the "D" pin numbering of Wemos modules.  These modules will work with any configurations for modules with 1MB flash though.

We have forked the esphome-ratgdo repository, and provide unofficial support for 2MB modules via the following device YAML you can use in the ESPHome Dashboard:

```yaml
substitutions:
  name: ratgdov25-2MB
  friendly_name: RAT-RATGDO-2MB
  id_prefix: ratratgdov25

packages:
  ratratgdo.esphome:
    url: https://github.com/Kaldek/esphome-ratgdo
    ref: main
    file: rat-ratgdo-2MB.yaml

esphome:
  name: ${name}
  name_add_mac_suffix: false
  friendly_name: ${friendly_name}
```
