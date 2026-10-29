# Changelog

Format based on [Keep a Changelog](https://keepachangelog.com/).

## [1.4.0] — 2026-10-29

### Added
- 7 more languages: Українська, Polski, Nederlands, Italiano, Türkçe, Bahasa Indonesia, 한국어 (15 in total).
- Animated theme and language menus (fade + slide) with rounded corners.
- The background picture/GIF is chosen with the native system file dialog (zenity/kdialog).

### Changed
- `install.sh` starts the app through a small launcher that logs errors to `~/.cache/android-flasher.log`.
- The main file is now `Android_Flasher.py`.

## [1.3.0] — 2026-10-28

### Added
- **Custom Image** theme: your own picture or animated GIF as the window background (PNG, JPG, WEBP, BMP, GIF).
- **Liquid Glass Image** theme: the same background with frosted-glass panels (blur, tint, highlight, border).
- *Theme → Choose picture/GIF…* to change the background; the choice is saved in `config.json`.
- Accent colour is picked from the background picture.
- Translations for the new strings in all 8 languages.

### Changed
- English is now the default language on first launch (a language you already picked is kept).
- `install.sh` and `build_windows.bat` install Pillow (optional, only needed for the image themes).

## [1.2.0] — 2026-10-23

### Added
- **Windows** support: Windows platform-tools, no console windows, DPI awareness, Segoe UI / Consolas fonts, `AndroidFlasher.bat` and `build_windows.bat` (builds a standalone `AndroidFlasher.exe`).
- **Debian/Ubuntu and Arch** support: `install.sh` detects apt / pacman / dnf and installs the right packages.
- Helpful messages with the matching system package command when platform-tools cannot be downloaded or the CPU is not x86_64.

### Changed
- Compatible with Python 3.8+.

## [1.1.0] — 2026-10-23

### Added
- Liquid Glass themes: **Pink**, **Blue**, **Mint**.
- Spanish, French and Portuguese (Brazil) translations (8 languages in total).
- Pixel / Material You style pill-shaped buttons (filled, tonal, text).
- Title set in the Fascinate font, installed automatically to `~/.local/share/fonts`.

### Changed
- Liquid Glass Dark is now neutral black instead of dark blue.
- The window title no longer shows the version number.

## [1.0.0] — 2026-10-23

### Added
- One-button automatic flow: find device → reboot to bootloader → unlock (if needed) → flash recovery → boot recovery → sideload ROM.
- Auto-download of official Google platform-tools; optional one-click udev setup.
- Automatic target partition detection (`boot`, `vendor_boot`, `init_boot`, `recovery`, `dtbo`).
- Flash to both A/B slots, bootloader unlock, data wipe, vbmeta (verity/verification off).
- Separate **Kernel** row: AnyKernel3-style ZIPs are detected by their content.
- «Test without writing» (`fastboot boot`) and image-vs-partition size warning.
- Native system file dialog (zenity/kdialog) with fallback to Tk.
- 5 themes: White, Dark, Pure Black, Liquid Glass White, Liquid Glass Dark.
- 5 languages: Русский, English, Deutsch, 中文, 日本語.
