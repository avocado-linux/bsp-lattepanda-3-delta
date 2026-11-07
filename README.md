# bsp-lattepanda-3-delta

> Standalone board support for the LattePanda 3 Delta SBC (Intel Celeron
> N5105, Jasper Lake / x86-64-v2), for the 2026 feeds.

Self-contained: it does not depend on `avocado-bsp-intel-x86-64-v2`.

| Hardware | Driver / package | Notes |
|---|---|---|
| 64 GB eMMC | `sdhci-pci` (built in) | The device number (`mmcblk0`/`mmcblk1`) can change between kernels; use `PARTLABEL`s |
| 2.5GbE port | `igc` | An Intel I226-V, although its MAC carries a Realtek OUI (`00:e0:4c`) |
| Wi-Fi (AX201, CNVi) | `iwlwifi`/`iwlmvm`, `wireless-regdb-static` | The kernel loads `regulatory.db`, which only the `-static` package ships |
| Bluetooth (AX201) | `btusb`/`btintel`, `linux-firmware-ibt` | `bluetooth.service` is enabled by this extension |
| UHD graphics, HDMI, DP (USB-C) | `i915`, `linux-firmware-i915` | eDP is disabled when no panel is fitted |
| Analog audio (ALC269VB) and HDMI/DP audio | `snd-hda-codec-alc269`, `snd-hda-codec-intelhdmi` | Since 6.17 each codec is its own module; without them `snd_hda_intel` binds and creates no card |
| Leonardo co-processor (ATmega32U4) | `cdc_acm` | `/dev/ttyACM0` |
| CMOS RTC | `rtc_cmos` | Fit the coin cell, or the clock resets at every power cut |
| TPM 2.0 (Intel PTT) | built in | `/dev/tpmrm0` |
| COM1 (DB9, RS-232) | `8250` (built in) | `ttyS0`, the serial console at 115200 |
| PCH I2C buses | `i2c-dev` | `/dev/i2c-*` |

| GPIO pins | `pinctrl-jasperlake` | `/dev/gpiochip0` |
| Header I2C (5 buses), SPI, UART | `intel-lpss-pci`, `i2c-designware-platform`, `spi-pxa2xx-platform`, `8250-dw`, `idma64` | The LPSS PCI functions create platform devices, so the platform drivers (not the `-pci` ones) bind them |
| Watchdog | `iTCO_wdt`, `wdat_wdt` | `/dev/watchdog0` |
| Thermal, power | `x86_pkg_temp_thermal`, `intel-rapl-msr`, `intel-pmc-core` | This board has no PCH thermal PCI function |
| microSD | `rtsx-usb`, `rtsx-usb-sdmmc` | A Realtek RTS5129 on USB |
| Video decode/encode | `intel-media-driver` (iHD) | `vainfo`: H.264/HEVC/VP9 decode, low-power encode |
| Vulkan | `mesa-vulkan-drivers` (ANV), `vulkan-loader` | Vulkan 1.4 on "Intel(R) UHD Graphics (JSL)" |

The platform drivers and the media stack need meta-avocado's Intel platform
kernel fragment (`intel-platform.cfg`) and x86 media packages: the published
2026 feeds before those changes do not carry these modules, and installing
this extension against them fails.

## Accessories, not checked

- M.2 M-key (NVMe or SATA) and B-key (SATA or a 4G/5G modem) slots: the NVMe
  and AHCI drivers are built in; nothing was fitted on the validated unit.
- eDP panel and touch.
