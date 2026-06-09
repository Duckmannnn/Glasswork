# Journey

This started with a browser switch.

I moved from Chrome to Zen Browser and immediately my desktop felt wrong. My old Plasma setup had the main panel at the top. That worked fine with Chrome, which doesn't have strong opinions about the rest of your screen. Zen does. It has its own sidebar, its own visual direction, its own sense of layout. My top panel suddenly looked like it belonged to a different setup entirely.

I could have just moved the panel down and called it done.

I didn't.

---

## Finding a direction

I went through r/unixporn the way everyone does when they decide to rice — tabs open for hours, screenshots saved to folders, names of tools noted down that I didn't understand yet.

Two KDE posts kept pulling me back:

- `[KDE] Yet Another Rice`
- `[KDE Plasma] My First Rice - Monochrome`

Both had the glass-and-blur direction I wanted. Three bottom panels, depth that made the desktop look like it was breathing. The Zen Browser transparency, the CAVA visualizer running in a panel, a live wallpaper that didn't feel like a screensaver.

My first instinct was to copy them directly. That lasted about an hour before I realized I was tracing someone else's choices without understanding any of them. The second I closed those screenshots, I didn't know what step came next.

So the goal changed: take what I liked from these references and build something that fit how I actually used the machine. The direction stayed — glass, bottom panels, Zen, live wallpaper. The execution had to be mine.

---

## The CSS spiral

My first approach was to build the glass effect from scratch with custom CSS.

The logic seemed straightforward: make Zen transparent, make websites transparent, everything becomes glassy.

It wasn't that straightforward.

I spent time editing `userChrome.css`, `userContent.css`, and Stylus rules, then adding site-specific patches as things broke. Gmail icons went dark. Coursemology got black text sitting on top of a dark translucent background. Figma's UI started glitching in ways that made it hard to use. GitHub's contribution graph colors shifted. Some pages ended up covered in opaque boxes where the transparency had failed to apply cleanly.

Every fix broke something else. After a while I'd lost track of what I was actually building. I was doing CSS damage control on a browser I'd broken myself.

That's when I knew I was doing it wrong.

---

## Transparency is not blur

Somewhere in that mess was a false belief that took too long to shake: that transparency and the glass effect were the same thing.

They're not.

A transparent window without compositor blur just looks broken. Whatever is behind the window bleeds through — wallpaper text mixes with UI text, bright backgrounds wash out icons, dark wallpapers swallow dark text. At one point Zen was technically transparent and it looked worse than the unmodified default theme.

The actual glass effect comes from blur at the compositor level. KWin applying a blur behind a translucent surface is what creates the frosted-glass quality. CSS transparency has nothing to do with that. I was punching holes in the browser window and expecting them to look like frosted glass.

Once I understood this, the whole approach changed.

---

## Starting over

I deleted everything I'd added to the browser CSS files and left them empty. Killed the Stylus rules. Then rebuilt around tools that were actually designed for this job: Transparent Zen, Zen Internet, Better Blur DX, and KWin's native blur and translucency effects.

The difference was immediate. Zen looked cleaner with its own native UI than it ever had under my custom CSS. Websites were readable again. The blur was real this time — handled at the compositor layer, not faked with CSS holes.

The specific technical steps for getting this working are in [`docs/troubleshooting.md`](troubleshooting.md).

---

## Widget detours

The panel layout went together cleanly. The widgets took longer.

Kurve, the CAVA audio visualizer widget, needed specific dependencies installed before it would do anything. Ginti — the workspace indicator I originally wanted to use — turned out to be broken on my Plasma version because it depended on a private KDE module that had been deprecated. I switched to Kara instead, which required building from source and resolving a KDE Frameworks version mismatch.

Annoying, but at least concrete. "CAVA is not running" is a real error message you can look up and act on. The CSS problems never gave me anything that clear.

All the widget error messages and fixes are in [`docs/troubleshooting.md`](troubleshooting.md).

---

## What the setup became

Three bottom panels. KWin blur and Better Blur DX handling the glass effect. Zen Browser kept mostly native with Transparent Zen and Zen Internet on top. Kurve and CAVA for the audio visualizer. Kara for workspace switching. A pixel/retro live wallpaper.

The full component list is in the [README](../README.md).

---

## What I'd do differently

Read the docs before touching anything.

The CSS approach felt hands-on. It wasn't. It was spending time undoing self-inflicted damage. The tools that actually make this rice work — Transparent Zen, Better Blur DX, KWin — all have setup notes and specific required steps. Twenty minutes reading those would have saved several hours of CSS archaeology.

The glass effect is a compositor feature. You configure it. You don't build it from scratch in a browser CSS file.

That's the thing this whole process taught me.
