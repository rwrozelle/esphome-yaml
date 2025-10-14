# YAML examples

## IR Transmitter (Friedrich)
Hardware Components:
* chip: esp32-h2 super-mini dev board
* ir transmitter: 940nm IR infrared Emission Tube LED Lamp

## Media Player
Hardware Components:
* chip: esp32-s3-devkitc-1-n16r8
* dac: pcm5102a (buy with back connections pre-soldered for ease of use)

## Voice Assistant
Hardware Components:
* chip: esp32-s3-devkitc-1-n16r8
* dac: max98357a
* mic: inmp441
 
## Test Deep Sleep H2
Hardware Components:
* chip: esp32-h2 super-mini dev board

HA Helper:
* input_boolean.esphome_prevent_sleep

HA Automations:
```
alias: EspHome Prevent Sleep Off
description: ""
triggers:
  - trigger: state
    entity_id:
      - input_boolean.esphome_prevent_sleep
    from: "on"
    to: "off"
conditions: []
actions:
  - action: mqtt.publish
    metadata: {}
    data:
      evaluate_payload: false
      qos: "0"
      retain: true
      topic: esphome_sleep/ota_mode
      payload: "OFF"
mode: single
```
```
alias: EspHome Prevent Sleep On
description: ""
triggers:
  - trigger: state
    entity_id:
      - input_boolean.esphome_prevent_sleep
    from: "off"
    to: "on"
conditions: []
actions:
  - action: mqtt.publish
    metadata: {}
    data:
      evaluate_payload: false
      qos: "0"
      retain: true
      topic: esphome_sleep/ota_mode
      payload: "ON"
mode: single
```