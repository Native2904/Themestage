# ThemeStage — Getting Started

## Installation

1. In Total Commander: **Configuration → Options → Plugins → File System Plugins → Configure → Add**
2. Select `ThemeStage.wfx64` (or `ThemeStage.wfx` for 32-bit)
3. Done — the plugin is now reachable via `\\themestage\`, e.g. by typing that path directly into TC's address bar

## What happens on the very first start

This runs automatically, no configuration needed:

1. **Opening `\\themestage\`** — on first launch, the plugin creates its own `ThemeStage.ini` and an empty `themes\` folder next to itself
2. **TC's color handling gets redirected once.** TC normally stores colors directly inside the central `wincmd.ini`. On first start, ThemeStage moves this section out into its own `color.ini` (using TC's own `RedirectSection=` mechanism) — this is necessary so ThemeStage can set colors precisely without touching the rest of the TC configuration. A backup of the original `wincmd.ini` is created automatically beforehand (`wincmd.ini.bak-themgr`)
3. **The panel folder starts out empty** (except for `[..]`) — that's expected, no theme has been placed in the `themes\` folder yet

## Where do I get themes?

ThemeStage reads color schemes in the **Windows Terminal format** (`.json`). A large, free collection is available at:

**https://isherlock.github.io/terminal-themes/**

Here's how:

1. Pick a theme you like on the website
2. Download its `.json` file
3. Place the file into the `themes\` folder right next to `ThemeStage.wfx64`
4. Re-open `\\themestage\` in TC (or simply navigate out and back in) — the new theme appears automatically in the panel, including a small preview icon

Any number of `.json` files can be placed in the folder at once — each one shows up as its own selectable entry.

## Selecting a theme

Just press **Enter** on a theme entry. The plugin then:

- computes the colors from the `.json` file
- writes them into TC's color file
- automatically adjusts file-type colors (code, images, videos, archives, etc. each get a matching color from the chosen theme)
- checks contrast while doing so: text that would be hard to read on the new background is automatically brightened or darkened

**Important:** On TC 10.50 and later, a theme change takes effect immediately — colors and window title update live, no TC restart involved. ThemeStage does this by triggering an official, documented TC command (`cm_SwitchColorsByFileType`) that tells TC to re-read its color settings. On older TC versions (before 10.50), the plugin automatically falls back to a full restart (a brief flash of the window, but just as reliable) — this can also be forced manually in `ThemeStage.ini`, see below.

## Working alongside other plugins (e.g. Autorun)

If you use another tool alongside ThemeStage that manages its own file-type color filters — for example the Autorun plugin, which often expects its own content filter to sit at a fixed position — ThemeStage automatically recognizes such foreign entries and leaves them alone. Only the entries ThemeStage itself generates under `AutoColorFilters=1` get replaced on a theme change; everything else stays exactly where it is, even across `! Reset`. No separate switch needed, fully automatic.

## Dark mode

ThemeStage automatically detects whether TC's own dark mode is set to "always enabled," and writes theme colors to the matching section (`[ColorsDark]` instead of `[Colors]`) accordingly. This means: if you want TC's overall interface (menus, dialogs) to stay dark, you can still pick any theme — light or dark — for the file list itself. No separate switch needed, fully automatic.

## Viewing details about a theme

**Alt+Enter** on a theme entry shows:

- every color code with an actual color swatch next to it
- whether the theme was classified as light or dark
- the currently linked brightness settings

## Fine-tuning brightness per theme

Every theme has its own small settings file:

```
themes\active\<ThemeName>.active.ini
```

```ini
[ThemMgr]
FontBrightness=0        ; -3 to +3, shifts the foreground colors
BackgroundBrightness=0  ; -3 to +3, shifts the background color
```

This file is created **automatically** the first time a theme is found — normally there's nothing to enter here by hand. But if you'd like one particular theme's colors slightly brighter or darker, you can adjust the values directly here (values outside -3 to +3 are automatically clamped to the valid range). This setting is **separate per theme** — each theme can have its own fine-tuning, independent of all others.

With `AutoColorFilters=0`, the same file additionally contains a `[ColorFilters]` section — this is the automatic record of that theme's most recently active file-type colors, managed by the plugin itself.

## Important settings (`ThemeStage.ini`)

```ini
[ThemMgr]
ActiveTheme=                  ; which theme is currently active (set automatically when selecting one)
AutoColorFilters=1            ; 1 = automatically generate file-type colors
                               ; 0 = your own, manually set file-type colors in TC are preserved
AutoRestartForTitle=0         ; 0 = default, theme changes take effect immediately, no restart (TC 10.50+)
                               ; 1 = a theme change fully restarts TC (for TC before 10.50, or as a
                               ;     fallback if the live path doesn't work cleanly for you)
                               ; Only affects normal theme changes - "! Reset" ALWAYS restarts TC,
                               ; regardless of this setting (see section below)
ColorFilterReloadMethod=1     ; only relevant when AutoRestartForTitle=0 - which live-update path is
                               ; used, normally no need to touch this
ColorIniPathOverride=         ; optional: a fixed path to the color file instead of automatic detection
DebugLogging=0                ; 1 = write a debug log (ThemeStage_debug.log) - leave off for normal use
```

## Undoing everything — `! Reset`

There's always a fixed entry at the very top of the panel: **`! Reset`**. This is the plugin's off switch — pressing Enter on it undoes **everything** ThemeStage has ever changed:

- Colors are restored from a backup taken automatically on the very first start (`wincmd.ini.bak-themgr`)
- The window title is removed
- `ActiveTheme=` is cleared — the plugin is back to its pure starting state (opt-in, touches nothing)

This **always** uses a real TC restart — regardless of the `AutoRestartForTitle=`/`ColorFilterReloadMethod=` settings above, which only affect normal theme changes. Reset removes the color redirect itself (TC should go back to reading directly from `wincmd.ini` instead of `color.ini`), and TC only picks that up reliably at its own startup. The effect is still immediate, just with the brief window flash a restart brings along.
