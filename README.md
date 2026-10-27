<div align="center">

# Android Flasher

**One-button flashing of custom recoveries, kernels and ROMs for Android — a small GUI for Linux (Fedora, Debian/Ubuntu, Arch) and Windows.**

[Changelog](CHANGELOG.md)

![License](https://img.shields.io/badge/license-MIT-blue)
![Platform](https://img.shields.io/badge/platform-Linux%20%7C%20Windows-51A2DA)
![Python](https://img.shields.io/badge/python-3.9%2B-3776AB)

</div>

> ⚠️ **Use at your own risk.** Flashing can erase all data and, with a wrong file, brick a device.
> Read your ROM's install notes first and make a backup. This project is not affiliated with Google, Xiaomi or any ROM project.

## What it does

Pick your files, press **Flash automatically** — the program does the rest:

1. Downloads the official Google **platform-tools** (or uses your system `adb`/`fastboot`).
2. Finds the phone and reboots it to the bootloader.
3. Unlocks the bootloader if it is locked (on request).
4. Detects the right partition for your image (`boot`, `vendor_boot`, `init_boot`, `recovery`, `dtbo`) and flashes it — to **both A/B slots** if you want.
5. Boots into recovery and installs your ROM / kernel ZIP via **ADB sideload**.

### Features

- 🔌 Automatic platform-tools download; one-click **udev** setup on Linux (USB access without a root shell)
- 🧠 Smart file recognition: recovery/boot `.img`, `vbmeta`, ROM ZIP and **AnyKernel3 kernel ZIP** (detected by *content*, not by name)
- 🧪 **Test without writing** — boots an image once with `fastboot boot`
- 📏 Image-vs-partition size check with a clear warning
- 🧹 Optional data wipe, vbmeta (verity/verification off), reboot to system after install
- 🎨 8 themes: White · Dark · Pure Black · Liquid Glass White / Dark / Pink / Blue / Mint
- 🔘 Pixel / Material You style pill-shaped buttons
- 🌍 8 languages: Русский · English · Deutsch · 中文 · 日本語 · Español · Français · Português
- 🔤 Title set in the [Fascinate](https://fonts.google.com/specimen/Fascinate) display font (fetched automatically)
- 📁 Native system file dialog (zenity / kdialog), starts in your Downloads folder

## Requirements

| System | What you need |
|---|---|
| **Fedora** (and derivatives) | `python3-tkinter` · optional: `zenity`, `polkit` |
| **Debian / Ubuntu** (Mint, Pop!_OS, Kali…) | `python3-tk` · `zenity` · `pkexec` |
| **Arch** (Manjaro, EndeavourOS…) | `python` · `tk` · `zenity` · `polkit` |
| **Windows 10 / 11** | Python 3.8+ from python.org (with *tcl/tk*) — or a standalone `AndroidFlasher.exe` built with `build_windows.bat` |

- A phone with an **unlockable bootloader**
- Developed and tested on **Fedora Workstation**. The other systems are supported through distribution detection and platform-specific code paths but are less tested — please open an issue if something breaks.

## Install

### Linux

```bash
git clone https://github.com/<your-username>/fedora-android-flasher.git
cd fedora-android-flasher
chmod +x install.sh
./install.sh          # detects apt / pacman / dnf, installs dependencies, adds an app-menu entry
```

Or run it directly without installing:

```bash
# Debian / Ubuntu
sudo apt install python3 python3-tk zenity
# Arch
sudo pacman -S python tk zenity
# Fedora
sudo dnf install python3-tkinter zenity

python3 fedora_flasher.py
```

**Update:** `git pull` and run `./install.sh` again. Your theme, language and downloaded platform-tools are kept (`~/.local/share/fedora-flasher/`).

### Windows

1. **From source:** install Python 3 from python.org (keep *tcl/tk and IDLE* checked) and double-click `AndroidFlasher.bat` (or run `py fedora_flasher.py`).
2. **Standalone exe (optional):** on a Windows PC run `build_windows.bat` → `dist\AndroidFlasher.exe`. It works without Python installed.
3. **USB driver:** install the Google USB Driver or your phone maker's driver so Windows recognizes the phone in fastboot mode.

Settings and downloaded platform-tools live in `%LOCALAPPDATA%\fedora-flasher\`.

> The title font is optional. If it cannot be downloaded the app simply uses the default font. To bundle it yourself, put `Fascinate-Regular.ttf` into a `fonts/` folder next to `fedora_flasher.py`.

## Usage

On the phone: enable **Developer options → USB debugging** (and **OEM unlocking**), connect it, and allow the debugging prompt.
The top of the window shows the device as `[adb: device]`.

### Step 1 — recovery

1. **Choose files…** → select the recovery / boot image (`.img`).
2. Options: *Wipe data* (needed when coming from stock firmware), *Flash to both slots* (A/B devices).
3. **Flash automatically** and confirm. Confirm the unlock on the phone if asked.

### Step 2 — ROM / kernel

1. Press **Reset**, then choose the ROM `.zip` (and/or a kernel `.zip` via **Kernel…**).
2. Leave *Wipe data* off.
3. **Flash automatically**. On the phone, in recovery, start **ADB sideload** — the program continues on its own.
4. Reboot to system from recovery.

> Sideload may stop at ~47% with `adb: failed to read command: Success` — this is normal on many recoveries. Check the result on the phone screen.

## How files are recognized

| File | Treated as |
|---|---|
| `*.img` with `vbmeta` in the name | vbmeta |
| other `*.img` | recovery / boot image |
| `*.zip` with `anykernel.sh` inside | kernel |
| `*.zip` with `payload.bin` / `system.new.dat*` | ROM |

## Known limitations

- **Samsung** uses Odin/Heimdall, not fastboot — not supported.
- **Xiaomi / Huawei / Sony** require the manufacturer's official unlock method first (e.g. Mi Unlock); the program cannot do that.
- ROMs are installed through **sideload** only; flashing factory images / dynamic partitions through `fastbootd` is not implemented.
- If several devices are connected, the first one is used.
- Developed and tested on **Xiaomi Mi A3 (laurel_sprout)**. Other fastboot devices may work, but are untested.

## Troubleshooting

| Problem | Fix |
|---|---|
| `adb: unauthorized` | Unlock the phone and tap *Allow USB debugging* |
| Phone not detected | Re-run the udev setup (`51-android-flasher.rules`), replug the cable |
| `Image too large` / flash fails | The image does not fit the partition — check which partition it is meant for |
| Chinese / Japanese shows boxes | Fedora: `google-noto-sans-cjk-fonts` · Debian/Ubuntu: `fonts-noto-cjk` · Arch: `noto-fonts-cjk` |
| Windows: phone not seen in fastboot | Install the Google USB Driver / your vendor's driver, then replug |
| Windows: SmartScreen / antivirus warning on the exe | A self-built PyInstaller exe is unsigned. Run from source instead (`AndroidFlasher.bat`) |
| ARM Linux: platform-tools can't be downloaded | Google's tools are x86_64 only — install `adb`/`fastboot` from your distro (`apt install adb fastboot` · `pacman -S android-tools`) |
| Stuck in a bootloop | Hold **Vol Down + Power** (fastboot) and flash a known-good image again |

## Project layout

```
fedora_flasher.py   # the whole application (Tkinter)
install.sh          # Linux: dependencies (apt / pacman / dnf) + app-menu entry
AndroidFlasher.bat  # Windows: run from source
build_windows.bat   # Windows: build AndroidFlasher.exe
fonts/              # optional: Fascinate-Regular.ttf
docs/               # screenshots
CHANGELOG.md
LICENSE
```

## Contributing

Issues and pull requests are welcome. When reporting a bug please include: device model, ROM/recovery name and version, the log from the program window, and your OS / distribution and version.

New translations: strings live in the `TRANSLATIONS` dictionary in `fedora_flasher.py` (the Russian text is the key).

## License

[MIT](LICENSE)

The title font **Fascinate** is © Astigmatic and licensed under the [SIL Open Font License 1.1](https://openfontlicense.org/); it is not part of this repository's MIT license.
