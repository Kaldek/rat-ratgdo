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
esphome-ratgdo does not list any 4MB-flash ESP8266 modules in the repo.  As a result, selecting an ESP8266 based board will result in ESPHome assuming your device has 1MB Flash even if it has 4MB (e.g., Wemos D1 Mini has 4MB Flash whereas the Wemos D1 Mini Lite only has 1MB).

When ESPHome has a version update, it is not possible to perform an OTA update via the ESPHome dashboard if it thinks your module only has 1MB of Flash.  Ergo, if your module has 4MB flash you must define this in your YAML file.

To do this, ensure your YAML file has the following lines added (preferably near the top of the file for good syntax consistency or "for the next guy"):
```yaml
esp8266:
  board: d1_mini
```

### Why does this work?
Even though the YAML files pulled from the esphome-ratgdo repo have a direct reference to the "d1_mini_lite" in their configuration, adding reference to the "d1_mini" in your YAML **overrides** the settings from the esphome-ratgdo repo.

### Initial re-flash after updating YAML
After making the change, you will initially need to re-flash your module either using USB or via stripping out all of the ratgdo references from the YAML so that the OTA file fits in the remaining spare flash space.  This is because that the change to your YAML only effectively applies after one successful installation of the YAML with the reference to the d1_mini.

## ESPHome OTA support for 2MB flash modules
Currently the esphome-ratgdo repository does not support configurations for ESP8266 modules with 2MB flash (such as the Tuya TYWE3L and TYWE3S), as the list of modules supported in their provided YAML files reference the "D" pin numbering of Wemos modules.

We have forked the esphome-ratgdo repository, and provide unofficial support for 2MB modules via the following YAML:

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
