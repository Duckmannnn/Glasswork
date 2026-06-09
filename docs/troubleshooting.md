# Troubleshooting

This file documents the actual problems I ran into while building this rice.

It is not a universal KDE guide. It is a record of what broke on my setup, what I tried, and what finally worked.

## 1. Top panel clashing with Zen Browser

This whole rice started because I switched from Chrome to Zen Browser.

My old Plasma setup had the main panel and settings controls at the top of the screen. That worked fine with Chrome, but it felt awkward with Zen because Zen already has a strong browser layout with its own sidebar and top UI.

Instead of only moving the panel, I decided to rebuild the desktop layout.

Final decision:

* move away from the top-panel setup
* build around three separated bottom panels
* keep the browser area visually clean
* make the desktop fit Zen instead of fighting against it

This was the starting point of the whole rice.

## 2. Trying to force transparency with custom CSS

My first real mistake was trying to make everything transparent manually.

I edited or experimented with:

* `userChrome.css`
* `userContent.css`
* Stylus
* global website CSS
* random site-specific CSS patches

The idea was simple: make Zen transparent, make websites transparent, and make the whole desktop look like glass.

It did not work well.

Some things became transparent, but many pages became harder to read or started looking broken.

Problems I ran into:

* Gmail icons became too dark or hard to see.
* Coursemology had black text on dark translucent backgrounds.
* GitHub avatars and contribution colors were affected.
* Figma icons and UI did not render nicely.
* Some pages became full of visible rectangular boxes.
* Some sites looked over-styled instead of clean.
* Zen Browser itself looked worse with a large custom `userChrome.css`.

The main lesson was that global CSS is too aggressive for a daily browser setup.

Final fix:

* clear `userChrome.css`
* clear `userContent.css`
* stop using heavy global Stylus rules
* avoid forcing every website to become transparent
* use Zen mods and KDE blur instead

Useful cleanup idea:

```bash
find ~/.config/zen -maxdepth 3 -type d -name chrome
```

Then clear the active profile's files:

```bash
: > userChrome.css
: > userContent.css
```

In my final setup, both files are empty or minimal.

## 3. Transparency without blur looked bad

After removing some CSS, I still wanted the glass look.

The problem was that transparency alone was not enough.

A transparent window without blur often looks messy because the wallpaper shows through too clearly. Text from the background mixes with text from the app, and the UI becomes harder to read.

At one point, Zen was technically transparent, but it was not comfortable to use. It looked more like a broken theme than a proper rice.

The actual glass effect needed compositor blur, not just transparent CSS.

Final fix:

* use Better Blur DX for real blur
* let KWin handle the blur effect
* use Transparent Zen and Zen Internet for Zen styling
* avoid trying to blur websites with CSS

This was one of the biggest turning points in the setup.

## 4. Better Blur DX not applying properly to Zen

After installing Better Blur DX, Zen did not immediately look right.

The window could be transparent, but the blur effect was not always applied the way I expected. The fix was to use forced blur matching.

Useful Better Blur DX classes for Zen:

```text
zen
Navigator
```

Current direction:

* blur decorations: enabled
* blur docks: enabled
* blur menus: enabled
* translucency: enabled
* force blur for Zen classes

This made the browser look much closer to the glass effect I wanted.

## 5. Zen looked better when kept mostly native

I tried making a large custom `userChrome.css`.

It technically changed the browser, but it did not make it better. Zen's own UI already looks good, and rewriting too much of it made the browser feel less polished.

Final decision:

* keep Zen Browser mostly native
* use Transparent Zen
* use Zen Internet
* use Better Blur DX from KDE/KWin
* avoid full browser UI rewrites

Final Zen approach:

```text
userChrome.css: empty or minimal
userContent.css: empty
```

This made the setup much more maintainable.

## 6. Figma and some websites did not like global styling

Some websites are not worth forcing into the rice.

Figma was the clearest example. When global styling or dark/transparency rules affected it, icons and UI elements became hard to read or just wrong.

Similar issues happened with:

* Gmail
* Coursemology
* GitHub
* Figma

Final fix:

* do not use global CSS for all websites
* disable styling on sensitive sites when needed
* let important web apps keep their original UI
* prioritize usability over making every website transparent

This is why the final rice is not a "make every website glass" setup.

## 7. KDE Store / Get New Stuff network errors

While installing KDE widgets and themes, KDE's Get New Stuff sometimes failed with HTTP2 / RST_STREAM style errors.

This made the built-in KDE Store feel unreliable.

Workaround:

```bash
QT_NETWORK_H2_ALLOWED=0 systemsettings
```

Then open the widget or theme installer again.

In some cases, manual installation from GitHub was more reliable than using KDE Store.

## 8. Restarting Plasma shell safely

After installing widgets or changing Plasma settings, I needed to restart Plasma shell.

Using `kstart plasmashell` gave a portal warning on my system:

```text
Could not register app ID: App info not found for 'org.kde.kstart'
```

The cleaner command was:

```bash
plasmashell --replace > /tmp/plasmashell.log 2>&1 & disown
```

Useful command after installing widgets:

```bash
kbuildsycoca6
```

Then restart Plasma shell.

## 9. Kurve / CAVA audio visualizer dependency issues

Kurve initially did not work correctly.

The widget reported problems like:

```text
CAVA is not running
module "QtWebSockets" is not installed
module "com.github.luisbocanegra.audiovisualizer.process" is not installed
```

The issue was not just the widget itself. It needed CAVA and some Qt/Python dependencies.

Useful Fedora packages:

```bash
sudo dnf install cava qt6-qtwebsockets qt6-qtwebsockets-devel python3-websockets
```

Test CAVA first:

```bash
cava
```

If CAVA works in terminal but the widget still fails:

```bash
plasmashell --replace > /tmp/plasmashell.log 2>&1 & disown
```

If that still does not work, log out and log back in.

Final result:

* CAVA works
* Kurve works
* audio visualizer can be used as part of the desktop rice

## 10. Ginti broke on Plasma 6.6

I tried using Ginti / desktop indicator style widgets, but it failed with:

```text
module "org.kde.plasma.private.pager" is not installed
```

This happened because the widget depended on an old/private KDE pager module.

Final fix:

* remove broken Ginti
* use Kara instead
* build Kara from source if the KDE Store version is broken

Remove old widgets if needed:

```bash
rm -rf ~/.local/share/plasma/plasmoids/org.kde.plasma.ginti
rm -rf ~/.local/share/plasma/plasmoids/org.kde.plasma.plasm6desktopindicator
rm -rf ~/.local/share/plasma/plasmoids/org.dhruv8sh.kara
```

Then install Kara from source.

## 11. Kara build failed because KDE Frameworks versions were mixed

While building Kara, CMake failed because some KDE Frameworks packages were on different versions.

The error looked like this:

```text
Could not find a configuration file for package "KF6CoreAddons"
compatible with requested version "6.26.0"

version found: 6.25.0
```

The problem was a version mismatch. Some KF6 packages had upgraded, but others were still older.

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

Then rebuild Kara:

```bash
cd /tmp/kara
rm -rf build
./install.sh
```

After that, restart Plasma:

```bash
kbuildsycoca6
plasmashell --replace > /tmp/plasmashell.log 2>&1 & disown
```

Final result:

* Kara installed successfully
* Ginti was no longer needed

## 12. GitHub token instead of password

When pushing this repo to GitHub over HTTPS, GitHub asked for a password.

The password field is not the normal GitHub account password. It needs a Personal Access Token.

What worked:

```text
Username: GitHub username
Password: GitHub personal access token
```

For a classic token, the useful scope was:

```text
repo
```

If Git remembered the wrong credential, this can clear it:

```bash
printf "protocol=https\nhost=github.com\n\n" | git credential reject
```

Then push again:

```bash
git push
```

## 13. Git push rejected because remote had newer changes

At one point, pushing failed with:

```text
! [rejected] main -> main (fetch first)
Updates were rejected because the remote contains work that you do not have locally.
```

Fix:

```bash
git pull --rebase origin main
git push
```

This keeps the local commit and replays it on top of the remote changes.

## 14. Git used the wrong local name/email

Git automatically used my Linux username and hostname for the commit author.

To avoid leaking local machine info, set the repo identity manually:

```bash
git config user.name "Duckmannnn"
git config user.email "Duckmannnn@users.noreply.github.com"
```

If the last commit already used the wrong identity:

```bash
git commit --amend --reset-author --no-edit
```

Then push again.

## Final notes

The biggest lesson from this rice:

```text
Transparency is not the same as glass.
```

The setup only started looking good after I stopped forcing everything with CSS and let the right tools do their jobs:

* KDE/KWin handles blur.
* Better Blur DX improves the glass effect.
* Zen stays mostly native.
* Transparent Zen and Zen Internet handle browser styling.
* Custom CSS is only a last resort.

The final result is simpler than the first attempt, but much cleaner and easier to maintain.
