# Glasswork

My first real KDE Plasma rice — a build log, not a raw dotfiles dump.

Glass blur, Zen Browser, live wallpaper, three bottom panels, desktop widgets, and a lot of things I broke before it finally worked.

## Preview

> Screenshots will be added later.

| Desktop                   | Zen Browser                   | Widgets                   |
| ------------------------- | ----------------------------- | ------------------------- |
| `screenshots/desktop.png` | `screenshots/zen-browser.png` | `screenshots/widgets.png` |

## What this repo is

This repo documents my Fedora KDE Plasma rice.

It is not a full dotfiles dump. I do not want to upload my entire `~/.config` or browser profile because those folders may contain private data, machine-specific files, browser sessions, cookies, cache, and local paths.

Instead, this repo is a rice guide and build log. It explains:

* the final look
* the features I used
* the sources that inspired me
* the mistakes I made
* the bugs I had to fix
* the setup philosophy I ended up with

## System

* OS: Fedora KDE
* Desktop: KDE Plasma
* Session: Wayland
* Browser: Zen Browser
* Style: glass, blur, dark theme, pixel/retro wallpaper
* Main direction: a usable daily desktop, not only screenshot candy

## Final Result

The final setup is built around:

* three separated bottom panels
* Breeze Dark base theme
* Better Blur DX
* Wobbly Windows
* Translucency
* Zen Browser with a transparent/glass look
* Transparent Zen
* Zen Internet
* Bonjourr start page
* uBlock Origin
* Kurve / CAVA audio visualizer
* Kara workspace indicator
* Modern Clock
* Plasmusic Toolbar
* Panel Colorizer
* Just Weather
* Thermal/system monitor widgets
* Web Pets
* pixel/retro live wallpaper

## Main Features

### Three bottom panels

The desktop uses three separated panels at the bottom.

The general idea:

* left panel: utility widgets / launcher / music
* center panel: task manager / pinned apps
* right panel: system tray / clock / controls

This layout was inspired by KDE rice posts on r/unixporn.

### Blur and transparency

Blur is mainly handled by KDE/KWin through Better Blur DX.

I originally tried to force transparency everywhere with CSS, but that made many apps and websites worse. The final version relies more on KDE's compositor and less on aggressive CSS hacks.

### Zen Browser

Zen Browser is kept mostly native.

Final approach:

* use Zen's own UI
* use Transparent Zen
* use Zen Internet
* use Better Blur DX for real blur
* keep `userChrome.css` empty or minimal
* keep `userContent.css` empty

This looked much cleaner than rewriting the entire browser UI manually.

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

The wallpaper direction is based on pixel/retro live wallpapers.

Wallpaper tools used or tested:

* Smart Video Wallpaper Reborn
* Shader Wallpaper

Wallpaper source:

* D3Ext/aesthetic-wallpapers Live Wallpapers collection

## Inspiration

Main inspirations:

1. r/unixporn — `[KDE] Yet Another Rice`
2. r/unixporn — `[KDE Plasma] My First Rice - Monochrome`
3. D3Ext/aesthetic-wallpapers Live Wallpapers

More details are in [`docs/references.md`](docs/references.md).

## What I changed from the references

This rice is not a direct copy.

My version focuses more on:

* keeping Zen Browser usable
* keeping the desktop clean
* avoiding heavy global CSS
* using KDE's compositor for blur
* documenting fixes and failed attempts
* making the rice stable enough for daily browsing, coding, and studying

## Things I had to fix

Some important problems I ran into:

* global CSS made websites unreadable
* Gmail icons became dark or invisible
* Coursemology had black text on dark translucent backgrounds
* GitHub avatars and contribution colors were affected
* Figma UI broke
* Zen Browser looked worse with a large custom `userChrome.css`
* Better Blur DX needed forced blur classes
* KDE Store had HTTP2 / RST_STREAM problems
* Kurve needed CAVA and QtWebSockets dependencies
* Ginti broke on Plasma 6.6, so I switched to Kara

More details are in [`docs/troubleshooting.md`](docs/troubleshooting.md).

## Setup Philosophy

The final rule:

> Use KDE and Zen's own theming systems first. Use custom CSS only as a last resort.

The rice became better after I removed unnecessary custom CSS.

## Safety note

Do not upload full browser profiles or raw desktop config dumps.

Avoid uploading:

* `~/.config/zen`
* cookies
* browser storage
* session files
* machine IDs
* private reports
* screenshots with personal tabs, emails, school/work pages, or messages

## Credits

Credits to the projects and communities that made this setup possible:

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
