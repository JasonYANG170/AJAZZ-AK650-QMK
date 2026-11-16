# AJAZZ AK650 QMK

[English](README.en.md)

| 项目 | 规格 |
| --- | --- |
| 型号 | 黑爵 AK650 三模带屏版 |
| 主控 | 华奋达 HFD80CP100（QMK 构建目标 SN32F299F） |
| 无线芯片 | 沁恒 CH582F，支持蓝牙和 2.4 GHz 接收器 |
| 有线连接 | USB |
| 屏幕 | GC9107 驱动，128 × 128 彩色 TFT LCD |
| 键数 | 66 键，另有 1 个可按压旋钮 |
| RGB | 66 键独立 RGB 背光 |

## 构建

在 Ubuntu 22.04 或 WSL2 的仓库根目录运行。

```bash
sudo apt update
sudo apt install -y git make gcc gcc-arm-none-eabi libnewlib-arm-none-eabi libhidapi-hidraw0 python3-venv
bash scripts/build.sh via
```

固件和 VIA 布局分别生成到 `dist/AK650-via.bin` 和 `dist/AK650-VIA.json`。GitHub Actions 构建产物可在 **Artifacts** 下载，推送与固件版本一致的标签会生成 **Release**。

## 刷机

1. 用 USB 数据线连接 AK650，切到有线模式，先备份 VIA 配置。
2. 在 0.3.5 或更新的固件上，长按旋钮进入设置，选择「开发模式」，短按进入确认页，再长按约 1.2 秒。进入刷机模式后，USB ID 为 `0C45:7140`。
3. 在 Windows 的仓库根目录运行以下命令，默认刷入 `firmware/` 内随仓库提供的镜像，校验通过后键盘会自动重启。

```powershell
py -m pip install hidapi
py scripts/flash.py
```

刷入本地构建的镜像时，运行 `py scripts/flash.py dist/AK650-via.bin --checksums dist/AK650-via.bin.sha256`。

旧版 QMK/VIA 可临时把 Fn 层的 Esc 设为 `QK_BOOT`，再按 Fn+Esc 进入刷机模式。原厂固件的 ISP 进入方式待确认。
