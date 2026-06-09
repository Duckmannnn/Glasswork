# Journey

This rice did not start as a plan to build a full KDE setup.

It started because I switched from Chrome to Zen Browser.

Before using Zen, my Plasma panel and settings controls were placed at the top of the screen. That layout was fine with Chrome, but it started to clash with Zen's interface. Zen already has its own strong layout and sidebar direction, so my old desktop setup suddenly felt awkward.

At that point, instead of only moving the panel, I thought: why not rice the whole desktop properly?

## The inspiration

I started looking through r/unixporn and found two KDE setups that looked really good:

* `[KDE] Yet Another Rice`
* `[KDE Plasma] My First Rice - Monochrome`

At first, I wanted to copy them directly.

But after trying to understand the style, I realized that a direct copy was not really mine. The references were beautiful, but I did not want my desktop to become someone else's screenshot. I liked specific ideas from them: the three bottom panels, the glass/blur direction, the Zen Browser transparency, the music visualizer, and the live wallpaper feel.

So the goal changed from "copy this rice" to "take the parts I like and make a setup that fits how I actually use my computer."

## The first mistake: forcing transparency with CSS

My first approach was honestly messy.

Instead of reading the GitHub pages and setup notes carefully, I tried to make everything transparent myself with custom CSS.

I edited:

* `userChrome.css`
* `userContent.css`
* global Stylus rules
* random site-specific CSS patches

The idea sounded simple: make Zen transparent, make websites transparent, make everything glassy.

In reality, it broke a lot of things.

Some pages became transparent, but not readable. Some text stayed black on dark backgrounds. Some icons disappeared. Some websites looked like they had been forced into a theme they were never designed for.

The worst parts were:

* Gmail icons became too dark or unreadable.
* Coursemology had black text on dark translucent backgrounds.
* GitHub avatars and contribution colors were affected.
* Figma's UI and icons broke badly.
* Many pages became full of ugly boxes instead of clean glass.
* Zen Browser itself started looking worse than the original themed UI.

I kept trying to fix one website at a time, but every fix created another problem somewhere else.

That was the point where I realized I was not really ricing anymore. I was just fighting CSS.

## Transparency is not blur

Another mistake was thinking that transparency alone would create the glass effect.

It did not.

A transparent window without blur often just looks messy. Text from the wallpaper mixes with text from the app. Bright parts of the background make icons harder to see. Dark parts make black text impossible to read.

At one point, Zen was technically transparent, but it was not pretty or usable. It looked more like a broken theme than a proper rice.

That was when I understood that the real glass effect needs compositor blur, not just transparent CSS.

## The turning point

I eventually stopped trying to force everything manually.

I cleared my custom browser CSS:

* `userChrome.css`: empty
* `userContent.css`: empty

Then I rebuilt the setup around tools that were actually meant for this:

* Transparent Zen
* Zen Internet
* Better Blur DX
* KDE/KWin blur and translucency
* Zen's own native UI

This worked much better.

Zen looked cleaner when I stopped rewriting its whole interface. Websites became usable again. The desktop still had the transparent glass feeling, but without breaking every page I opened.

For Better Blur DX, I also had to make sure Zen was actually being blurred by KWin. The useful forced blur classes were:

```text
zen
Navigator
```

That was one of the most important fixes. Before that, the browser could be transparent but still not have the real blurred background effect.

## Widget problems

The widgets also had their own problems.

Kurve / Audio Visualizer needed CAVA and QtWebSockets dependencies before it worked properly. At first it reported that CAVA was not running or that QML modules were missing.

Ginti also broke on my Plasma version because it depended on an old private KDE pager module:

```text
org.kde.plasma.private.pager
```

The fix was to stop using the broken Ginti widget and switch to Kara instead. Building Kara also exposed some KDE Frameworks version mismatch issues, so I had to update related KF6 packages before it compiled correctly.

These problems were annoying, but they were much easier to fix than the CSS mess, because at least the errors were concrete.

## The final direction

The final rice is much simpler than my first attempt.

It uses:

* three bottom panels
* Better Blur DX
* Zen Browser
* Transparent Zen
* Zen Internet
* Kurve / CAVA
* Kara
* Plasmusic Toolbar
* Modern Clock
* Panel Colorizer
* weather and system widgets
* pixel/retro live wallpaper

But it avoids:

* heavy global CSS
* forcing every website to be transparent
* rewriting the whole Zen UI
* copying another rice exactly
* uploading raw private config folders

The most important lesson from this setup was that more customization does not always mean a better rice.

The desktop became better when I removed the unnecessary parts and let KDE, Zen, and Better Blur DX do what they were good at.

This repo exists to document that process: not just the final screenshot, but the mistakes, fixes, and decisions behind it.
