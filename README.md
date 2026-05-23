# Arduino IDE 2 Wayland + NVIDIA Fix for Omarchy

Fixes the immediate `Segmentation fault (core dumped)` crash when launching **Arduino IDE 2** on Omarchy (custom Arch Linux + Hyprland) with NVIDIA GPUs.

## Problem

Arduino IDE 2.x crashes right after printing the frontend configuration with a segmentation fault. This happens because the frontend fails to initialize the graphics context when running on Wayland.

## Solution

Force Arduino IDE to run under X11 by setting the appropriate environment variables.

## Files Included

- `arduino-ide` — Wrapper script for terminal use
- `arduino-ide-v2.desktop` — Desktop entry override for menu/Walker launching

## Installation

### Terminal Wrapper

```bash
cp arduino-ide ~/.local/bin/
chmod +x ~/.local/bin/arduino-ide
```

Ensure `~/.local/bin` is in your `$PATH`.

### Desktop Entry

```bash
cp arduino-ide.desktop ~/.local/share/applications/
```

Refresh Omarchy’s application list:

```bash
omarchy-refresh-applications
```

## Why This Approach

- Uses `XDG_SESSION_TYPE=x11` + unsets `WAYLAND_DISPLAY` to force XWayland
- Hardcodes the full path to `/usr/bin/arduino-ide` to avoid PATH conflicts
- Places the desktop file in `~/.local/share/applications/` so it overrides the system file without modifying package-owned files

## Notes & Troubleshooting

- Arduino IDE 2 can sometimes leave behind stale backend processes. If launching fails after a previous unclean exit, run:
  ```bash
  pkill -f arduino
  ```
  Then try again.
- This fix was developed and tested on Omarchy with NVIDIA GPUs in May 2026.

## License

MIT — feel free to use and share.
