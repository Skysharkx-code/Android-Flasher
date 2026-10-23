<div align="center">

# Android Flasher for Linux

**One-button flashing of custom recoveries, kernels and ROMs for Android — a small GUI for Fedora Linux.**

[Changelog](CHANGELOG.md)

![License](https://img.shields.io/badge/license-MIT-blue)
![Platform](https://img.shields.io/badge/platform-Fedora%20Linux-51A2DA)
![Python](https://img.shields.io/badge/python-3.9%2B-3776AB)

<img src="docs/screenshot-dark.png" alt="Android Flasher for Linux — Liquid Glass Dark" width="460">

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

- 🔌 Automatic platform-tools download and one-click **udev** setup (no root shell needed for USB access)
- 🧠 Smart file recognition: recovery/boot `.img`, `vbmeta`, ROM ZIP and **AnyKernel3 kernel ZIP** (detected by *content*, not by name)
- 🧪 **Test without writing** — boots an image once with `fastboot boot`
- 📏 Image-vs-partition size check with a clear warning
- 🧹 Optional data wipe, vbmeta (verity/verification off), reboot to system after install
- 🎨 Themes: White · Dark · Pure Black · Liquid Glass White · Liquid Glass Dark
- 🌍 Languages: Русский · English · Deutsch · 中文 · 日本語
- 📁 Native system file dialog (zenity / kdialog), starts in your Downloads folder

## Requirements

- Fedora Linux (developed on Fedora Workstation; other distros should work but are untested)
- Python 3.9+ with Tk: `sudo dnf install python3-tkinter`
- Optional: `zenity` (native file dialog), `polkit` (for the udev setup via `pkexec`)
- A phone with an **unlockable bootloader**

## Install

```bash
git clone https://github.com/<your-username>/fedora-android-flasher.git
cd fedora-android-flasher
chmod +x install.sh
./install.sh          # installs dependencies and adds an app-menu entry
```

Or run it directly without installing:

```bash
sudo dnf install python3-tkinter zenity
python3 fedora_flasher.py
```

**Update:** `git pull` and run `./install.sh` again. Your theme, language and downloaded platform-tools are kept.

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
| Chinese / Japanese shows boxes | `sudo dnf install google-noto-sans-cjk-fonts` |
| Stuck in a bootloop | Hold **Vol Down + Power** (fastboot) and flash a known-good image again |

## Project layout

```
fedora_flasher.py   # the whole application (Tkinter)
install.sh          # dependencies + app-menu entry
docs/               # screenshots
CHANGELOG.md
LICENSE
```

## Contributing

Issues and pull requests are welcome. When reporting a bug please include: device model, ROM/recovery name and version, the log from the program window, and your Fedora version.

New translations: strings live in the `TRANSLATIONS` dictionary in `fedora_flasher.py` (the Russian text is the key).

## License

[MIT](LICENSE)
