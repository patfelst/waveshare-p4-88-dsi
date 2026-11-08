# ESPHome firmware

Use the bring-up build to check the panel, touch and backlight before adding a dashboard.

| File | Use |
| --- | --- |
| [esphome-bringup.zip](esphome-bringup.zip) | Test project for ESPHome 2026.9.0, with YAML, local MIPI DSI driver, license and example secrets. |
| [p4-88-bringup.yaml](p4-88-bringup.yaml) | Readable test card source. The ZIP includes the required driver. |
| [waveshare-p4-db-88-test.yaml](waveshare-p4-db-88-test.yaml) | Reference copy of `/config/esphome/waveshare-p4-db-88-test.yaml` with example Home Assistant entities. |

## First boot

1. Extract the ZIP and keep `waveshare_pr_components/` beside `p4-88-bringup.yaml`. Use ESPHome **2026.9.0** for the included backport.
2. Copy `secrets.example.yaml` to `secrets.yaml`, set your Wi-Fi credentials and generate your own API encryption key with `openssl rand -base64 32`.
3. Run `esphome config p4-88-bringup.yaml`, then `esphome run p4-88-bringup.yaml` to flash over USB-C.
4. Look for a portrait test card and native touch coordinates in the log. The project unit responds at `0x45` for panel control and `0x5D` for Goodix touch. The onboard audio codec may also appear at `0x18`.
5. For Wi-Fi, use compatible ESP-Hosted firmware on the C5 coprocessor. This configuration pins the host side to `2.12.13` and does not update the C5.

The included local driver supplies the OTA7290B command table and skips software reset. The display test card was previously verified on the project hardware. The DSI PHY’s internal 2.5 V rail is separate from the external 5 V LCD supply.

## Dashboard reference

The full dashboard uses LVGL in 1920 × 480 landscape and adds Home Assistant sensors, rack temperature history, backlight control and sleep logic. To build it directly, supply `rack_history_bars.h`, the referenced fonts and artwork, a matching C5 firmware binary, and your own Home Assistant entity mappings. The test project is independent of those files.

[Back to the build guide](../README.md#4-load-esphome)
