# Brightness Popup

A lightweight, gamma-corrected brightness control for Linux laptops — accessible via a single keyboard shortcut. No bloat, no settings daemon, no GNOME Shell version drama.

Press **Super+B**, slide. Press again, it's gone.

## Features

- **Gamma-corrected curve** — a 50% slider feels like 50% brightness. No more dead zones where 80–100% does nothing while 0–20% drops you into darkness.
- **Toggle behavior** — same key to show *and* hide. One process, no clones.
- **Frameless popup** — appears at cursor, dark themed, closes on Escape or 15s timeout.
- **Preset buttons** — 10/25/50/75/100% for quick jumps.
- **Panel indicator** — optional persistent tray icon with the same controls.
- **No sudo** — works with pre-granted permissions on standard backlight interfaces.

## Installation

```bash
# Requirements (typically pre-installed on Ubuntu/Pop/Debian)
sudo apt install python3-gi gir1.2-ayatanaappindicator3-0.1

# Install the scripts
cp brightness /home/youruser/.local/bin/
cp brightness-indicator /home/youruser/.local/bin/
chmod +x ~/.local/bin/brightness ~/.local/bin/brightness-indicator
```

### Keyboard shortcut (GNOME)

```bash
gsettings set org.gnome.settings-daemon.plugins.media-keys custom-keybindings \
  "['/org/gnome/settings-daemon/plugins/media-keys/custom-keybindings/custom0/']"

gsettings set org.gnome.settings-daemon.plugins.media-keys.custom-keybinding:/org/gnome/settings-daemon/plugins/media-keys/custom-keybindings/custom0/ \
  name "Brightness Popup"

gsettings set org.gnome.settings-daemon.plugins.media-keys.custom-keybinding:/org/gnome/settings-daemon/plugins/media-keys/custom-keybindings/custom0/ \
  command "/home/youruser/.local/bin/brightness"

gsettings set org.gnome.settings-daemon.plugins.media-keys.custom-keybinding:/org/gnome/settings-daemon/plugins/media-keys/custom-keybindings/custom0/ \
  binding "<Super>b"
```

### Panel indicator (optional)

Enable the Ubuntu AppIndicators GNOME Shell extension, then add `brightness-indicator` to your autostart:

```bash
cp brightness-indicator.desktop ~/.config/autostart/
```

## How the gamma curve works

Raw backlight values are linear, but human perception of brightness is logarithmic. A linear slider means 0–20% of the hardware range covers most of the visible adjustment, while 80–100% is practically indistinguishable.

Brightness Popup applies a **power-law curve** (γ = 0.55) to the slider's percentage before writing to hardware:

| Slider | Hardware value | Perceived brightness |
|--------|---------------|---------------------|
| 10%    | 28% of max    | ≈10%                |
| 25%    | 47% of max    | ≈25%                |
| 50%    | 68% of max    | ≈50%                |
| 75%    | 85% of max    | ≈75%                |
| 100%   | 100% of max   | ≈100%               |

The result: the slider feels **linear to your eye**, not to the hardware.

## Compatibility

- **Backlight interface**: `/sys/class/backlight/intel_backlight/brightness`
- **Desktop**: GNOME 40+ (Wayland & X11)
- **Distribution**: Ubuntu, Debian, Pop!_OS, Fedora (with python3-gi)

## License

MIT — do whatever you want with it.
