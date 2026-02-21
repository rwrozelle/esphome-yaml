# Power Management Testing of esp32-h2 Super Mini
Testing the external power_management component under multiple yaml configurations using the Nordic Semiconductor Power Profiler Kit II (PPK2)

* Test Date 2026-02-21
* EspHome Version: 2026.3.0-dev20260221
* DUT: ESP32-C6FH4 (QFN32) (revision v0.2) (esp32-c6 DFRobot Fire Beetle v1.2
* DUT Features: Wi-Fi 6, BT 5 (LE), IEEE802.15.4, Single Core + LP Core, 160MHz, Crystal frequency: 40MHz
* Power Supply: 3.3V pin

## Initial Test
The initial test to establish a baseline power profile over a 1 minute band.

```yaml
external_components:
  # [power_management] new component that enables power management for esp_idf
  - source: github://pr#12325
    components: [power_management]
    refresh: 1h

esphome:
  name: test-esp32c6-1
  friendly_name: Test ESP32C6 1

esp32:
  #esp32-c6 Fire Beetle v1.2
  variant: esp32c6
  flash_size: 4MB
  framework:
    type: esp-idf
    log_level: NONE
    sdkconfig_options:
      #esp32-c6 Fire Beetle v1.2 has a 40Mhz external clock
      CONFIG_RTC_CLK_SRC_INT_RC: "n"
      CONFIG_RTC_CLK_SRC_EXT_CRYS: "y"

logger:
  level: NONE

network:
  enable_ipv6: true

power_management:
  enable_light_sleep: true
  power_down_flash: true
  power_down_peripherals: true
```
Test results in an average amperage of ~.07mA.  A pulse to 35mA occurs once per minute and is caused by the preferences component waking the chip up to store preferences in NVS.

![Alt text](./esp32-c6-pm.png)

## Add Wifi
Add wifi component

```yaml
wifi:
  fast_connect: true
  ssid: !secret wifi_ssid
  password: !secret wifi_password
```

Test results in an average amperage of ~12mA with peaks to ~240mA

![Alt text](./esp32-c6-pm-wifi.png)

## Add API Test
Add api component

```yaml
api:
  encryption:
    key: !secret api_encryption_key
```

Test results in an average amperage of ~18mA with peaks to ~240mA

![Alt text](./esp32-c6-pm-wifi-api.png)

## API Test, Without Power Management
Use api component, remove power_management component

Test results in an average amperage of ~35mA with peaks to ~255mA

![Alt text](./esp32-c6-wifi-api.png)
