# Changelog

All notable changes to avocado-bsp-lattepanda-3-delta are documented in this
file. The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [0.1.0]

### Added
- Initial release: standalone board support for the LattePanda 3 Delta SBC
  (Intel Celeron N5105 / x86-64-v2), for the 2026 feeds (kernel 6.18).
- Board hardware: the Intel I226-V 2.5GbE port (`igc`), AX201 Wi-Fi with the
  kernel regulatory database (`wireless-regdb-static`) and Bluetooth (Intel
  firmware, `bluetooth.service` enabled), UHD graphics and display firmware,
  HD Audio with the ALC269VB analog codec and HDMI/DP audio, the onboard
  Leonardo co-processor (`cdc_acm`), the CMOS RTC (`rtc_cmos`), I2C character
  devices, and a VT console getty beside the COM1 serial getty.
