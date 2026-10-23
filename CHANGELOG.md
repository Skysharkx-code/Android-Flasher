# Changelog

Format based on [Keep a Changelog](https://keepachangelog.com/).

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
