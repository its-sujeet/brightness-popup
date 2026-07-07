# Brightness Popup

A gamma-corrected brightness slider for **Intel-based Linux laptops** — toggle with a single keyboard shortcut. No daemon, no settings UI, no GNOME version lock. One hotkey, one sliding window, done.

```
Super+B → slide or tap a preset → Esc (or Super+B again) → gone
```

## Quick start

```bash
# 1. Clone the repo
git clone https://github.com/its-sujeet/brightness-popup.git
cd brightness-popup

# 2. Copy the scripts to your local bin
cp brightness brightness-indicator ~/.local/bin/
chmod +x ~/.local/bin/brightness ~/.local/bin/brightness-indicator

# 3. Install requirements (Ubuntu/Debian/Pop)
sudo apt install python3-gi gir1.2-ayatanaappindicator3-0.1

# 4. Make the backlight file writable (Intel graphics)
sudo chmod o+w /sys/class/backlight/intel_backlight/brightness

# 5. Bind the keyboard shortcut (GNOME)
gsettings set org.gnome.settings-daemon.plugins.media-keys custom-keybindings \
  "['/org/gnome/settings-daemon/plugins/media-keys/custom-keybindings/custom0/']"

gsettings set org.gnome.settings-daemon.plugins.media-keys.custom-keybinding:/org/gnome/settings-daemon/plugins/media-keys/custom-keybindings/custom0/ \
  name "Brightness Popup"

gsettings set org.gnome.settings-daemon.plugins.media-keys.custom-keybinding:/org/gnome/settings-daemon/plugins/media-keys/custom-keybindings/custom0/ \
  command "$HOME/.local/bin/brightness"

gsettings set org.gnome.settings-daemon.plugins.media-keys.custom-keybinding:/org/gnome/settings-daemon/plugins/media-keys/custom-keybindings/custom0/ \
  binding "<Super>b"
```

That's it. Press **Super+B** to open the slider, drag it, and press again (or Esc) to close.

## Why this exists

Backlight values are linear. Human perception is logarithmic. A normal slider dedicates 80% of its travel to the last 20% of perceivable brightness — meaning you breathe on it and the screen goes dark. This popup applies a **power-law curve** so the slider feels like it should: slide to halfway and it's *half as bright*, not almost black.

The slider ranges from **-40** to **100**. It writes directly to `/sys/class/backlight/intel_backlight/brightness` — the standard Intel graphics backlight interface on most modern Linux laptops.

| Slider | Hardware | Feel |
|--------|----------|------|
| -40 | 0 | True black |
| -30 | ~60 | Faint glow |
| -20 | ~270 | Deep dark |
| -10 | ~650 | Barely there |
| 0 | ~1220 | Tiniest usable |
| 25 | ~3700 | Dim room |
| 50 | ~7260 | Comfortable |
| 75 | ~12460 | Bright |
| 100 | 19200 | Max |

The negative range lets you descend into darkness in tiny, deliberate steps — each tick changes the hardware by just a handful of units.

## Files

| File | What it does |
|------|-------------|
| `brightness` | The hotkey popup — frameless GTK3 window, runs on Super+B |
| `brightness-indicator` | Optional tray icon with the same slider |
| `brightness-indicator.desktop` | Autostart entry for the tray version |

## Compatibility

Built for **Intel graphics laptops** with `/sys/class/backlight/intel_backlight/brightness`. Works on GNOME (Wayland & X11) — Ubuntu, Debian, Pop!_OS, Fedora. If your laptop uses a different backlight path (e.g. `amd_backlight`, `nvidia_0`), you'll need to modify the script or symlink the path.

## Tray indicator (optional)

If you prefer a persistent icon in your panel:

```bash
cp brightness-indicator.desktop ~/.config/autostart/
```

Then enable the "Ubuntu AppIndicators" GNOME Shell extension and log out & back in. The indicator appears in your top bar with the same slider and presets.

## Updating

```bash
cd brightness-popup
git pull
cp brightness brightness-indicator ~/.local/bin/
```

## License

MIT — do whatever you want.
