# Change Log

## v1.10.0

- Integrated hpm_sdk v1.10.0

- Updated:
  - Upgrade `CherryUSB` stack to 1.5.0

- Added:
  - Support zcc compiler
  - Support SEGGER Embedded Studio IDE, version 8.24

- Fixed:
  - Fix the issue that the vectoredd interrupt mode may not work properly in some conditions
  - Fix the issue that WDOG reset failed to work after executing `reset` command in shell

## v1.9.0

- Integrated hpm_sdk v1.6.0
- Samples:
  - blink_led
  - timer_demo
  - adc_example
  - mcan_example
  - ethernet_demo
  - ethernet_ptp_master_demo
  - ethernet_ptp_slave_demo
  - flashdb_demo
  - uart_dma_demo
  - usb_device_generic_hid
  - usb_host_msc_udisk
  - pwm_demo
