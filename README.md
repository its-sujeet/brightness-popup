# Brightness Popup

A gamma-corrected brightness slider for Linux laptops — toggle with a single keyboard shortcut. No daemon, no settings UI, no GNOME version lock. One hotkey, one sliding window, done.

```
Super+B → slide or tap a preset → Esc (or Super+B again) → gone
```

## Why this exists

Backlight values are linear. Human perception is logarithmic. A normal slider dedicates 80% of its travel to the last 20% of perceivable brightness — meaning you breathe on it and the screen goes dark. This popup applies a **power-law curve** so the slider feels like it should: slide to halfway and it's *half as bright*, not almost black.

| Slider | Hardware | What you see |
|--------|----------|-------------|
| 10% | 28% of max | Dimmest usable |
| 25% | 47% of max | Evening comfort |
| 50% | 68% of max | True midpoint |
| 75% | 85% of max | Bright room |
| 100% | 100% of max | Full |

## Files

| File | What it does |
|------|-------------|
| `brightness` | The hotkey popup — run once on Super+B |
| `brightness-indicator` | Optional tray icon with the same slider |
| `brightness-indicator.desktop` | Autostart entry for the tray version |

## Setup

Requirements — everything ships with Ubuntu/Debian/Pop, or install with:

```bash
sudo apt install python3-gi gir1.2-ayatanaappindicator3-0.1
```

**Hotkey** (GNOME):

```bash
gsettings set org.gnome.settings-daemon.plugins.media-keys custom-keybindings \
  "['/org/gnome/settings-daemon/plugins/media-keys/custom-keybindings/custom0/']"

gsettings set org.gnome.settings-daemon.plugins.media-keys.custom-keybinding:/org/gnome/settings-daemon/plugins/media-keys/custom-keybindings/custom0/ \
  name "Brightness Popup"

gsettings set org.gnome.settings-daemon.plugins.media-keys.custom-keybinding:/org/gnome/settings-daemon/plugins/media-keys/custom-keybindings/custom0/ \
  command "/path/to/brightness"

gsettings set org.gnome.settings-daemon.plugins.media-keys.custom-keybinding:/org/gnome/settings-daemon/plugins/media-keys/custom-keybindings/custom0/ \
  binding "<Super>b"
```

**Brightness file** must be writable:

```bash
sudo chmod o+w /sys/class/backlight/intel_backlight/brightness
```

## Tray indicator (optional)

If you prefer a persistent icon:

```bash
mkdir -p ~/.config/autostart
cp brightness-indicator.desktop ~/.config/autostart/
```

Then enable the "Ubuntu AppIndicators" GNOME Shell extension and log out & back in.

## License

MIT — do whatever you want.
