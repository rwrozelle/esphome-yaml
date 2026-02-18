# Power Management Testing of esp32-h2 Super Mini
Testing the external power_management component under multiple yaml configurations using the Nordic Semiconductor Power Profiler Kit II (PPK2)

* Test Date 2026-02-18
* EspHome Version: 2026.3.0-dev20260218
* DUT: esp32-h2 Super Mini - ESP32-H2 (revision v0.1)
* DUT Features: BT 5 (LE), IEEE802.15.4, Single Core, 96MHz, Crystal frequency:  32MHz

## Initial Test
The initial test was to establish a baseline power profile over a 1 minute band.

```yaml
external_components:
  # [power_management] new component that enables power management for esp_idf
  - source: github://pr#12325
    components: [power_management]
    refresh: 1h
  # [openthread] Provide action to control radio on/off when poll_period > 0
  - source: github://pr#11766
    components: [openthread]
    refresh: 1h

esphome:
  name: test-esp32h2-2
  friendly_name: Test ESP32H2 2

esp32:
  board: esp32-h2-devkitm-1
  variant: ESP32H2
  flash_size: 4MB
  framework:
    type: esp-idf
    log_level: NONE
    sdkconfig_options:
      CONFIG_RTC_CLK_SRC_INT_RC: "n"
      CONFIG_RTC_CLK_SRC_EXT_CRYS: "y"
      CONFIG_OPENTHREAD_LOG_LEVEL_DYNAMIC: "n"
      CONFIG_OPENTHREAD_LOG_LEVEL_NONE: "y"

logger:
  level: NONE

power_management:
  enable_light_sleep: true
  power_down_flash: true
  power_down_peripherals: true

network:
  enable_ipv6: true
```

![Alt text](./esp32-h2-pm.png)
