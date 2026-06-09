# Troubleshooting

This file records the main problems I ran into while building the rice and how I fixed them.

## Restart Plasma shell

After installing or changing widgets, restart Plasma shell with:

```bash
plasmashell --replace > /tmp/plasmashell.log 2>&1 & disown
```

Another useful command:

```bash
kbuildsycoca6
```

## KDE Store HTTP2 / RST_STREAM issue

Sometimes KDE's Get New Stuff / KDE Store failed with HTTP2 or RST_STREAM errors.

Workaround:

```bash
QT_NETWORK_H2_ALLOWED=0 systemsettings
```

Then open the relevant theme/widget installer again.

Manual installation from GitHub is often more reliable for some widgets.

## Kurve / CAVA dependency issue

Kurve initially failed because CAVA or QtWebSockets dependencies were missing.

Useful Fedora packages:

```bash
sudo dnf install cava qt6-qtwebsockets qt6-qtwebsockets-devel python3-websockets
```

Test CAVA:

```bash
cava
```

If CAVA works in terminal but the widget still fails, restart Plasma or log out and back in.

## Ginti broken on Plasma 6.6

Ginti may fail with:

```text
module "org.kde.plasma.private.pager" is not installed
```

This happens because it relies on an old/private KDE pager module.

Final fix:

* remove old Ginti
* use Kara instead
* build Kara from source if the KDE Store version is broken

## Kara build version mismatch

While building Kara, CMake may fail if KDE Frameworks packages are on different versions.

Example problem:

```text
Could not find a configuration file for package "KF6CoreAddons"
compatible with requested version "6.26.0"
version found: 6.25.0
```

Useful fix:

```bash
sudo dnf clean metadata
sudo dnf makecache --refresh
sudo dnf install -y kf6-kcoreaddons kf6-kcoreaddons-devel
sudo dnf upgrade --refresh --best --allowerasing \
  'kf6-kcoreaddons*' \
  'kf6-kpackage*' \
  'kf6-kitemmodels*' \
  'kf6-kirigami*' \
  'libplasma*' \
  'plasma-activities*'
```

Then rebuild Kara.

## Better Blur DX force blur

Useful classes for Zen Browser:

```text
zen
Navigator
```

These help Better Blur DX apply blur to Zen correctly.

## Zen Browser CSS problems

At first, I tried heavy global CSS, Stylus rules, `userChrome.css`, and `userContent.css`.

This caused problems:

* Gmail icons became dark or unreadable
* Coursemology had black text on dark translucent backgrounds
* GitHub avatars and contribution colors were affected
* Figma UI broke
* websites became over-styled
* Zen Browser looked uglier than the native themed UI

Final fix:

* clear `userChrome.css`
* clear `userContent.css`
* avoid heavy global CSS
* use Transparent Zen
* use Zen Internet
* use Better Blur DX for real blur
* disable styling on sensitive sites if needed

## Final lesson

The setup became better when I stopped trying to customize everything manually.

The best result came from:

* KDE handling blur
* Better Blur DX handling glass effects
* Zen Browser staying mostly native
* Transparent Zen and Zen Internet handling browser transparency
* custom CSS being used only as a last resort
