# Troubleshooting

Specific problems encountered while building this rice, with their fixes.

For the narrative context behind these decisions, see [`docs/journey.md`](journey.md).

---

## 1. Custom CSS breaking website readability

**Symptom:** Global `userChrome.css`, `userContent.css`, or Stylus rules cause broken visuals on Gmail, Coursemology, GitHub, Figma, and other sites — dark icons, invisible text, or opaque overlay boxes.

**Cause:** Forcing transparency across all websites without per-site awareness. CSS has no knowledge of the compositor, so it can make windows see-through without any of the blur that makes the effect usable.

**Fix:** Clear both CSS files and stop using global rules.

```bash
# Find your active Zen profile's chrome folder
find ~/.config/zen -maxdepth 3 -type d -name chrome

# Empty both files in the active profile
: > userChrome.css
: > userContent.css
```

In the final setup, both files are empty. Browser styling is handled by Transparent Zen and Zen Internet instead.

---

## 2. Zen transparent but no blur effect

**Symptom:** Zen shows the wallpaper through the window but looks messy — no frosted-glass quality, text from the background bleeds into the UI.

**Cause:** CSS transparency is not the same as compositor blur. Without KWin blurring behind the window, the result is a clear hole, not glass.

**Fix:** Use KWin's blur and translucency effects instead of CSS. Enable both in System Settings → Desktop Effects. Then configure Better Blur DX on top (see #3).

---

## 3. Better Blur DX not blurring Zen

**Symptom:** Better Blur DX is installed and KWin blur is enabled, but Zen Browser does not get the blurred background effect.

**Cause:** Zen's window class is not in Better Blur DX's force-blur list by default.

**Fix:** Add the following classes in Better Blur DX settings under "Force blur":

```
zen
Navigator
```

Also enable in Better Blur DX:
- Blur decorations
- Blur docks
- Blur menus
- Translucency

---

## 4. KDE Store / Get New Stuff failing with HTTP2 errors

**Symptom:** RST_STREAM or connection errors when installing widgets or themes through the KDE Store interface.

**Fix:**

```bash
QT_NETWORK_H2_ALLOWED=0 systemsettings
```

Then open the widget or theme installer again from inside System Settings. If this is unreliable, download the widget from its GitHub repo and install manually instead.

---

## 5. Plasma shell restart

**Symptom:** Newly installed widgets don't appear, or `kstart plasmashell` gives:

```
Could not register app ID: App info not found for 'org.kde.kstart'
```

**Fix:**

```bash
kbuildsycoca6
plasmashell --replace > /tmp/plasmashell.log 2>&1 & disown
```

Run `kbuildsycoca6` first after installing new widgets, then replace the shell.

---

## 6. Kurve / CAVA audio visualizer not starting

**Symptom:** Kurve widget shows errors like:

```
CAVA is not running
module "QtWebSockets" is not installed
module "com.github.luisbocanegra.audiovisualizer.process" is not installed
```

**Cause:** CAVA and the required Qt/Python WebSocket dependencies are missing.

**Fix:**

```bash
sudo dnf install cava qt6-qtwebsockets qt6-qtwebsockets-devel python3-websockets
```

Test CAVA standalone first:

```bash
cava
```

If CAVA runs in terminal but the widget still fails, restart Plasma shell (see #5). If it still fails after that, log out and log back in.

---

## 7. Ginti broken on Plasma 6.6+

**Symptom:**

```
module "org.kde.plasma.private.pager" is not installed
```

**Cause:** Ginti depends on `org.kde.plasma.private.pager`, a private KDE module that was deprecated and removed in newer Plasma versions.

**Fix:** Remove Ginti and use Kara instead.

```bash
rm -rf ~/.local/share/plasma/plasmoids/org.kde.plasma.ginti
rm -rf ~/.local/share/plasma/plasmoids/org.kde.plasma.plasm6desktopindicator
rm -rf ~/.local/share/plasma/plasmoids/org.dhruv8sh.kara
```

Then install Kara from source (see #8).

---

## 8. Kara build failing — KDE Frameworks version mismatch

**Symptom:**

```
Could not find a configuration file for package "KF6CoreAddons"
compatible with requested version "6.26.0"
version found: 6.25.0
```

**Cause:** Some KF6 packages upgraded while others didn't, creating a version conflict during CMake configuration.

**Fix:** Sync all affected KF6 packages:

```bash
sudo dnf clean metadata
sudo dnf makecache --refresh
sudo dnf upgrade --refresh --best --allowerasing \
  'kf6-kcoreaddons*' \
  'kf6-kpackage*' \
  'kf6-kitemmodels*' \
  'kf6-kirigami*' \
  'libplasma*' \
  'plasma-activities*'
```

Then rebuild Kara:

```bash
cd /tmp/kara
rm -rf build
./install.sh
```

Restart Plasma after:

```bash
kbuildsycoca6
plasmashell --replace > /tmp/plasmashell.log 2>&1 & disown
```
