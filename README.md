# Glasswork

A Fedora KDE Plasma rice with a glass/blur desktop, Zen Browser, three bottom panels, live wallpaper, and lightweight Plasma widgets.

This repository is a rice documentation repo, not a full dotfiles dump.

## Preview

> Screenshots will be added later.

| Desktop                   | Zen Browser                   | Widgets                   |
| ------------------------- | ----------------------------- | ------------------------- |
| `screenshots/desktop.png` | `screenshots/zen-browser.png` | `screenshots/widgets.png` |

## Overview

Glasswork started as a small layout adjustment after I switched from Chrome to Zen Browser. My old top-panel setup did not fit well with Zen's interface, so I rebuilt the desktop around a cleaner bottom-panel layout.

The final setup uses KDE's compositor effects and Zen's own theming system instead of heavy custom CSS.

## System

| Component | Details                                        |
| --------- | ---------------------------------------------- |
| OS        | Fedora KDE                                     |
| Desktop   | KDE Plasma                                     |
| Session   | Wayland                                        |
| Browser   | Zen Browser                                    |
| Style     | Glass, blur, dark theme, pixel/retro wallpaper |

## Features

* Three separated bottom panels
* Better Blur DX
* Wobbly Windows
* Translucency effect
* Zen Browser transparent/glass setup
* Transparent Zen
* Zen Internet
* Bonjourr start page
* Kurve / CAVA audio visualizer
* Kara workspace indicator
* Plasmusic Toolbar
* Modern Clock
* Panel Colorizer
* Weather and system monitor widgets
* Pixel/retro live wallpaper

## Components

### KDE Plasma

The Plasma setup is based on a three-panel bottom layout.

General layout:

* left panel: launcher / pinned apps
* center panel: task manager / important and active apps
* right panel: tray / controls / clock

### Zen Browser

Zen Browser is kept mostly native.

Used:

* Transparent Zen
* Zen Internet
* Bonjourr
* uBlock Origin
* Better Blur DX forced blur

Avoided:

* heavy `userChrome.css`
* heavy `userContent.css`
* global website CSS overrides

### Widgets

Main widgets used or tested:

* Modern Clock
* Plasmusic Toolbar
* Kurve / Audio Visualizer
* Kara
* Just Weather
* Thermal Monitor
* KVitals
* Web Pets
* Panel Colorizer
* Minimize All
* KDE Connect
* Media Controller

### Wallpaper

Wallpaper tools used or tested:

* Smart Video Wallpaper Reborn
* Shader Wallpaper

Wallpaper source:

* [D3Ext/aesthetic-wallpapers Live Wallpapers](https://github.com/D3Ext/aesthetic-wallpapers/blob/main/pages/Live.md)

## Notes

This rice originally involved custom `userChrome.css`, `userContent.css`, and global CSS experiments. Those were removed because they caused readability and UI issues on several websites.

Final approach:

* keep Zen mostly native
* keep `userChrome.css` empty or minimal
* keep `userContent.css` empty
* use KDE/KWin blur instead of forcing transparency through CSS
* use Transparent Zen and Zen Internet for browser styling

Useful Better Blur DX matching classes for Zen:

```text
zen
Navigator
```

## Troubleshooting

Common issues and fixes are documented in:

* [`docs/troubleshooting.md`](docs/troubleshooting.md)

Covered issues include:

* KDE Store HTTP2 / RST_STREAM errors
* Kurve / CAVA dependency issues
* Ginti breaking on Plasma 6.6
* Kara build problems
* Zen Browser CSS readability issues
* Better Blur DX force blur setup

## Inspiration

This rice was inspired by several KDE setups and wallpaper collections:

* [[KDE] Yet Another Rice](https://www.reddit.com/r/unixporn/comments/1pgcy8d/kde_yet_another_rice/)
* [[KDE Plasma] My First Rice - Monochrome](https://www.reddit.com/r/unixporn/comments/1tg82cr/kde_plasma_my_first_rice_monochrome/)
* [My first KDE rice, keep it simple](https://www.reddit.com/r/LinuxPorn/comments/1s82mu9/my_first_kde_rice_keep_it_simple/)
* [D3Ext/aesthetic-wallpapers Live Wallpapers](https://github.com/D3Ext/aesthetic-wallpapers/blob/main/pages/Live.md)

## Credits

* Fedora KDE Plasma
* KDE Plasma
* Zen Browser
* Transparent Zen
* Zen Internet
* Better Blur DX
* CAVA
* Kurve / Audio Visualizer
* Kara
* Plasmusic Toolbar
* Smart Video Wallpaper Reborn
* Shader Wallpaper
* Panel Colorizer
* Bonjourr
* uBlock Origin
* r/unixporn
* r/LinuxPorn
