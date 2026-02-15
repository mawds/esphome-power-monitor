# Power monitor for Home Assisstant / ESP Home

Based on https://esphome.io/cookbook/power_meter/

Monitors electric (GPIO10) and gas (GPIO11) consumption

Current verson of pulse_monitor didn't work (crashed/locked the ESP32 after a few mins), so followed [these instructions](https://github.com/esphome/issues/issues/4807) and used old version of the code for it

## ESP device

Used [one of these](https://www.aliexpress.com/item/1005007036161205.html) - "ESP32-C6-DevKitC-1 ESP32-C6 Core Board WIFI6 BLE Zigbee Ultra-low Power Consumption Compatible ESP32 Series Modules" 

ESPHome doesn't currently support Zigbee for ESP32s, so using it with WiFi

`powermonitor.yaml` will need modifying if using a different board

## Initial flash

```bash
docker run --rm --privileged -v "${PWD}/config":/config --device=/dev/ttyACM0 -it ghcr.io/esphome/esphome run powermonitor.yaml
```

Setup `secrets.yaml` with required keys
Plug ESP32 into USB
Connect to http://localhost:6052
Flash over USB.  Once the initial flash is done you can use WiFi

(Note that the config dir cannot be the root of a git repo, or the build process will fail)

## Home assistant

Add ESPHome integration.  
It should pickup the ESP32 automatically.  Add it.

Use a [utility_meter](https://www.home-assistant.io/integrations/utility_meter) helper for the meter sensors (and hide the original sensors); this will preseve the totals on restart, otherwise they reset to 0 whenever the ESP32 is rebooted.

## UV environment

This is for esptool.  This is another way of getting the initial instance of esphome onto the device


