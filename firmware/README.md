# ESPHome firmware

Use the bring-up build to check the panel, touch and backlight. Tap the test button to display “Touch OK”.

| File | Use |
| --- | --- |
| [p4-88-bringup.yaml](p4-88-bringup.yaml) | LVGL colour pattern and touch test for ESPHome 2026.9.0. |
| [MIPI DSI component source](waveshare_pr_components/mipi_dsi) | Local driver backport and panel definitions. See the [ESPHome license](ESPHome-LICENSE.txt). |
| [secrets.example.yaml](secrets.example.yaml) | Template for local Wi-Fi credentials and the API encryption key. |

### Why the custom component is included

The custom component backports support for the 8.8-inch OTA7290B panel to ESPHome 2026.9.0. It supplies the panel’s timings and initialization commands, honours its requirement to skip software reset, and retains the D-PHY clock fix used by the production ESP32-P4 board. The project’s hardware testing found that issuing software reset left the backlight on without a picture. Keep the included component beside the YAML so ESPHome loads this tested driver.

## First boot

1. Clone the repository or download the source from GitHub. In this folder, keep `waveshare_pr_components/` beside `p4-88-bringup.yaml`. Use ESPHome **2026.9.0** for this backport.
2. Copy `secrets.example.yaml` to `secrets.yaml`, set your network details and generate a unique API encryption key with `openssl rand -base64 32`. Keep `secrets.yaml` local.
3. Run the configuration check and then flash the board over its USB-C programming connection:

   ```sh
   esphome config p4-88-bringup.yaml
   esphome run p4-88-bringup.yaml
   ```

4. Check that a landscape LVGL colour pattern appears and that touches print coordinates in the log. The expected I²C addresses are `0x45` for panel control and `0x5D` for the Goodix touch controller on the project unit. The board's audio codec may also answer at `0x18`.
5. For network operation, the onboard ESP32-C5 must already have compatible ESP-Hosted coprocessor firmware. The test configuration pins the host component to `2.12.13` and does not update the C5. Treat a Wi-Fi failure and a display failure as separate checks.

The local MIPI DSI model supplies the OTA7290B startup commands and skips software reset. The earlier project test produced a visible card using this path. The DSI PHY's internal 2.5 V regulator is separate from the LCD's external 5 V input.

[Back to the build guide](../README.md#4-load-esphome)
