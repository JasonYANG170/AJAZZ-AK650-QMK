# AJAZZ AK650 QMK

[简体中文](README.md)

QMK firmware with VIA support for the AJAZZ AK650 tri-mode keyboard with a built-in display.

| Component | Specification |
| --- | --- |
| Main MCU | HFD80CP100 (QMK build target `SN32F299F`) |
| Wireless | WCH CH582F, with Bluetooth and a 2.4 GHz receiver |
| Wired connection | USB |
| Display | 128 × 128 color TFT LCD with a GC9107 driver |
| Controls | 66 keys and one clickable knob |
| Lighting | Per-key RGB backlighting |

## Firmware downloads

Open a successful run from the [firmware workflow](https://github.com/JasonYANG170/AJAZZ-AK650-QMK/actions/workflows/firmware.yml) and download `AK650-VIA` from **Artifacts**. The archive contains the firmware binary, the VIA layout JSON, and `SHA256SUMS`. The workflow publishes these files to [Releases](https://github.com/JasonYANG170/AJAZZ-AK650-QMK/releases) when a pushed tag matches the firmware version, for example `v0.3.5`.

## Build from source

Run these commands from the repository root on Ubuntu 22.04 or WSL2.

```bash
sudo apt update
sudo apt install -y git make gcc gcc-arm-none-eabi libnewlib-arm-none-eabi libhidapi-hidraw0 python3-venv
bash scripts/build.sh via
```

The build writes the firmware binary to `dist/AK650-via.bin`, its checksum to `dist/AK650-via.bin.sha256`, and the VIA layout to `dist/AK650-VIA.json`.

## Flash on Windows

1. Connect the AK650 with a USB data cable, switch it to wired mode, and back up your VIA configuration.
2. On firmware 0.3.5 or newer, hold the knob to open settings and select **Developer** (**开发模式**). Press the knob to open the confirmation screen, then hold it for about 1.2 seconds. In flashing mode the keyboard uses USB ID `0C45:7140`.
3. Run the following commands from the repository root to flash the image bundled in `firmware/`. The keyboard restarts automatically after verification.

   ```powershell
   py -m pip install hidapi
   py scripts/flash.py
   ```

To flash a locally built image, run the following command.

```powershell
py scripts/flash.py dist/AK650-via.bin --checksums dist/AK650-via.bin.sha256
```

On older QMK/VIA firmware, you can temporarily map Esc on the Fn layer to `QK_BOOT`, then press Fn+Esc to enter flashing mode. The procedure for entering ISP mode from the factory firmware is still unconfirmed.
