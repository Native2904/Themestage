# ThemeStage — Getting Started

[Screenshot_1320.bmp](https://github.com/user-attachments/files/34913499/Screenshot_1320.bmp)


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
4. Re-open `\\themestage\` in TC (or simply navigate out and back in) — the new theme appears automatically in the panel, including a preview icon

Any number of `.json` files can be placed in the folder at once — each one shows up as its own selectable entry.

**Tip:** In TC's thumbnail view, every entry shows a larger, sharp preview with the theme's main colors — handy for flipping through a bigger collection at a glance instead of opening each theme one by one.

## Selecting a theme

Just press **Enter** on a theme entry. The plugin then:

- computes the colors from the `.json` file
- writes them into TC's color file
- automatically adjusts file-type colors (code, images, videos, archives, etc. each get a matching color from the chosen theme)
- checks contrast while doing so: text that would be hard to read on the new background is automatically brightened or darkened

**Important:** On TC 10.50 and later, a theme change takes effect immediately — colors and window title update live, no TC restart involved. ThemeStage does this by triggering an official, documented TC command (`cm_SwitchColorsByFileType`) that tells TC to re-read its color settings. On older TC versions (before 10.50), the plugin automatically falls back to a full restart (a brief flash of the window, but just as reliable) — this can also be forced manually in `ThemeStage.ini`, see below.

## Telling which theme is currently active

There are four indicators. The three that can be switched off are controlled individually in `ThemeStage.ini`:

**Green check mark on the icon** (`ActiveBadge=1`) — the active theme carries a small green check mark at the bottom right of its icon, in every view: thumbnail, brief and full. So the mark moves along immediately after a change, ThemeStage has the panel re-read once. If you'd rather it didn't, set `ActiveBadge=0` — that turns off both.

**Short notice after a change** (`ShowToast=1`) — a small notice appears in the bottom right of the TC window for about two seconds, showing the theme name and its main colors, then fades out by itself. This is the plugin drawing its own notice, not a Windows notification — so it still shows up when Windows is set to "Do not disturb".

**Name in the title bar** (`ShowThemeInTitle=1`) — TC's window title ends with the name of the active theme. If you'd rather leave the title bar alone, set `ShowThemeInTitle=0`; the name is removed on the next change. See also the separate note about the window title.

**Alt+Enter** — shows a green **ACTIVE** badge right after the name of the active theme. This is the one indicator that can never be covered up, even with all three above switched off.

## Multiple TC windows open at once

If you regularly run TC with several windows open side by side (two or three separate TC instances at once), this behavior is worth knowing about — not a bug, but good to understand so nothing feels unexpected.

**All TC windows share the same color file.** Selecting a theme in one window writes to that one, shared file. Selecting a different theme in another window shortly after simply overwrites that same file again — with the new value.

**An example:**

1. Window A: selects theme "Dracula"
2. Window B (shortly after): selects theme "Nord"
3. Window C (shortly after that): selects theme "Gruvbox"

In the end, the file simply contains **Gruvbox** — the last value written, regardless of which window was opened first or closed last. Only the timing of the actual selection matters.

**What you'll see:** Right after those three selections, each window initially keeps showing **its own**, last-selected theme — window A still shows Dracula, window B still shows Nord, even though the file has long since moved on to Gruvbox. This isn't a broken or corrupted file — each window simply shows whatever it last triggered itself, and doesn't automatically learn about what other windows have done. This display catches up on its own the next time that window becomes active again (its own next theme change, or a normal TC restart).

The same goes for the green check mark: in each window it marks the theme that this particular window selected last.

**In short:** Multiple TC windows open at the same time coexist just fine technically — no corrupted file, no data loss. Only the on-screen display can briefly drift apart until it catches up. If you work with several TC windows regularly and want them to always show the same color, just change the theme in **one** window and then briefly refresh the others (e.g. leave `\\themestage\` and re-enter it).

## Working alongside other plugins (e.g. Autorun)

If you use another tool alongside ThemeStage that manages its own file-type color filters — for example the Autorun plugin, which often expects its own content filter to sit at a fixed position — ThemeStage automatically recognizes such foreign entries and leaves them alone. Only the entries ThemeStage itself generates under `AutoColorFilters=1` get replaced on a theme change; everything else stays exactly where it is, even across `! Reset`. No separate switch needed, fully automatic.

**Autorun scripts that write the title bar:** some Autorun scripts rebuild TC's window title again and again, typically through the `TC_Title` variable. The old theme name used to stay there, because the script overwrote the title shortly after ThemeStage had set it. ThemeStage now updates that variable as well, so the title stays correct even with such a script running. If you don't want a theme name in the title at all, use `ShowThemeInTitle=0` — the name is then removed from `TC_Title` too.

## Dark mode

ThemeStage automatically detects whether TC's own dark mode is set to "always enabled," and writes theme colors to the matching section (`[ColorsDark]` instead of `[Colors]`) accordingly. This means: if you want TC's overall interface (menus, dialogs) to stay dark, you can still pick any theme — light or dark — for the file list itself. No separate switch needed, fully automatic.

## Viewing details about a theme

**Alt+Enter** on a theme entry opens its own dark-styled window. The theme name sits at the top, followed by a green **ACTIVE** badge if this is the active theme, with its light/dark classification and background lightness just below.

**COLORS section** — all 20 colors as slim rows in two columns. Each row has a color chip, the name, the hex value, and on the right the contrast ratio against the theme background with its rating (AA / AA Large / Fail). The four UI fields (background, foreground, cursor, selection) keep their functional names; the 16 ANSI colors are labelled with **the name that matches the actual color**. If a theme's "yellow" slot really holds an orange, it says Orange. That used to work differently and regularly caused confusion.

**ACTIVE SETTINGS section** — the plugin's switches with their current value, read fresh from the ini files:

| Row | comes from |
|---|---|
| Font brightness | this theme's `FontBrightness=` |
| Background brightness | this theme's `BackgroundBrightness=` |
| Color filters | `AutoColorFilters=` |
| Title bar name | `ShowThemeInTitle=` |
| Toast on change | `ShowToast=` |
| Check mark on icon | `ActiveBadge=` |
| Colors written to | where TC writes its colors (`color.ini` or `wincmd.ini`) |
| Extension overrides | whether this theme carries its own extension lists |

At the very bottom, a note appears if any incomplete or invalid color values had to be replaced while loading.

The window is display only — values can't be changed there; that's what the ini files and ThemeLister are for.

## Editing colors directly — ThemeLister.wlx

ThemeStage ships with a companion Lister plugin, `ThemeLister.wlx64` (or `.wlx` for 32-bit). Once installed, that same view becomes a full color editor, right inside TC's own file viewer:

1. Install it once: **Configuration → Options → Plugins → Lister Plugins → Configure → Add** → select `ThemeLister.wlx64`
2. Select any theme entry in `\\themestage\` and press **F3** (Lister) or **Ctrl+Q** (QuickView)
3. Click any of the 20 color rows to open the standard Windows color picker and change it
4. Press **Ctrl+S** to save — this writes directly back into that theme's real `.json` file

Instead of the contrast ratio, each row shows the **file category** that this color paints on the right — so you can see straight away that touching "Bright Green" affects your source files, not something abstract.

A few things worth knowing:

- The very first time you save a given theme, ThemeStage automatically creates a one-time backup (`<theme>.json.bak-themelister`) right next to it, so an accidental edit can always be undone by hand
- This only works for themes already sitting in `themes\` — ThemeLister doesn't do anything with unrelated `.json` files elsewhere on your system, it specifically reacts to files ThemeStage itself hands it when you press F3/Ctrl+Q
- No `.active.ini`-related settings (brightness fine-tuning, `AutoColorFilters=`) are shown or touched here — that's a separate file (see "Fine-tuning brightness per theme" below). Ctrl+S here saves only to the theme's own `themes\<name>.json` — the raw color source, not the brightness/filter settings file

## Which files get which color — the extension editor

ThemeStage spreads the 16 ANSI colors across 16 file categories (source code, text documents, images, videos, archives, system files and so on). Which extensions belong to which category comes preset — and as of this version it can be **adjusted per theme**, without opening an ini by hand.

How to get there: in Lister (F3 on a theme), click **Edit file extensions ›** in the top right. A plain white text area appears, laid out like an ini file, one line per category:

```
[ExtensionCategories]

SourceCode=*.cpp;*.h;*.py;*.cs;*.js;*.ts;*.go;*.rs;
    *.java;*.rb;*.php

TextDocuments=*.txt;*.md;*.nfo;*.rtf
```

The keys:

| Key | What it does |
|---|---|
| **Tab** | jumps to the next extension and selects it — typing replaces it right away |
| **Shift+Tab** | the same backwards; at the end of the list it wraps around |
| **Ctrl+S** | save and go back to the colors |
| **Esc** | discard |
| **Ctrl+A** | select all |

A few rules, so nothing gets lost:

- Only the text **after the `=`** counts. The category names are fixed; a line with an unknown name is ignored
- **An indented line continues the line above.** Long categories are wrapped that way when the editor opens and joined back together when saving. A line without an `=` also counts as a continuation, in case you delete the indentation by accident
- **Deleting a whole line leaves its category unchanged** — so there's no way to accidentally empty a category by deleting. To really empty one, leave the text after the `=` blank
- An extension without a wildcard gets completed automatically: `cpp` becomes `*.cpp`. If you write your own pattern (`driv*.`), it is taken as is

Everything is saved into the `[ExtensionCategories]` section of `themes\active\<ThemeName>.active.ini` — that is, **separately per theme**. If nothing is in there, the built-in default list applies. Whether a theme carries its own lists is shown by Alt+Enter in the "Extension overrides" row.

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

The same file may hold two further sections, both managed by the plugin itself: `[ColorFilters]` when `AutoColorFilters=0` (the record of that theme's most recently active file-type colors) and `[ExtensionCategories]` once you have saved something in the extension editor.

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

ShowThemeInTitle=1            ; 1 = the active theme's name appears in TC's window title
                               ; 0 = the title bar is left alone; the name is removed on the next
                               ;     change, including from the Autorun variable TC_Title
ShowToast=1                   ; 1 = brief notice in the bottom right corner after each theme change
                               ; 0 = off
ActiveBadge=1                 ; 1 = green check mark on the active theme's icon; the panel is re-read
                               ;     after each change so the mark moves along
                               ; 0 = no check mark, no re-read

DebugLogging=1                ; 1 = write a debug log (ThemeStage_debug.log) - default since a real
                               ;     data-loss case was hard to diagnose with logging off by default;
                               ;     safe to set back to 0 once you're confident everything is stable
IconLogBudget=150             ; how many icon/thumbnail requests get written to the log; only of
                               ; interest when chasing a problem, 0 = none
```

Two further keys are created and maintained by the plugin itself: `WindowTitleBase=` (see the separate note about the window title) and `ActiveThemeSince=` — the moment the active theme was selected. TC needs the latter to notice that the entry has changed, so the check mark actually moves after a switch. There is nothing to set by hand there.

## Undoing everything — `! Reset`

There's always a fixed entry at the very top of the panel: **`! Reset`**. This is the plugin's off switch — pressing Enter on it undoes **everything** ThemeStage has ever changed:

- Colors are restored from a backup taken automatically on the very first start (`wincmd.ini.bak-themgr`)
- The window title is removed, along with the stored `WindowTitleBase=`
- `ActiveTheme=` is cleared — the plugin is back to its pure starting state (opt-in, touches nothing); the green check mark disappears with it

This **always** uses a real TC restart — regardless of the `AutoRestartForTitle=`/`ColorFilterReloadMethod=` settings above, which only affect normal theme changes. Reset removes the color redirect itself (TC should go back to reading directly from `wincmd.ini` instead of `color.ini`), and TC only picks that up reliably at its own startup. The effect is still immediate, just with the brief window flash a restart brings along.

What Reset does *not* touch are the individual themes' `.active.ini` files — brightness values and custom extension lists survive and apply again as soon as you pick that theme once more.
