# Changelog

Format based on [Keep a Changelog](https://keepachangelog.com/).

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
