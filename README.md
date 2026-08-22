# ThemeStage

[Deutsch](#deutsch) | [English](#english)

<img width="1920" height="1041" alt="2026-08-21_214308" src="https://github.com/user-attachments/assets/70bd84cf-af18-4268-874c-ac513ba0cd07" />


## Deutsch

Ein Total-Commander-Dateisystem-Plugin, das Windows-Terminal-Farbschemata (`.json`) lädt und automatisch auf TCs Panel-Farben anwendet — inklusive passender Dateityp-Einfärbung und Kontrast-Prüfung.

### Was es kann

- Liest Farbschemata im Windows-Terminal-JSON-Format (19 Standardfarben)
- Wendet sie auf TCs eigene `[Colors]`-Konfiguration an
- Generiert automatisch passende Dateityp-Farben (8 Kategorien: Code, Bilder, Videos, Archive, Audio, Systemdateien, Dokumente, Ordner)
- Prüft dabei automatisch den Kontrast gegen den Theme-Hintergrund und korrigiert zu kontrastarme Werte
- Zeigt jedes Theme als eigenständigen Panel-Eintrag mit einem live gerenderten Vorschau-Icon (mehrere Auflösungsstufen, scharf bei jeder Anzeigegröße)
- Alt+Enter zeigt alle Farbcodes eines Themes mit echtem Farbmuster
- Merkt sich pro Theme eigene, manuell in TC angepasste Dateityp-Farben (optional, siehe `AutoColorFilters=`)
- Zeigt das aktive Theme im TC-Fenstertitel an
- Themewechsel wirken sofort, ohne TC neu zu starten (ab TC 10.50; bei älterem TC automatisch über einen Neustart, siehe `AutoRestartForTitle=`)
- Erkennt und schont Dateityp-Farbfilter anderer Tools (z. B. das Autorun-Plugin) — nur ThemeStages eigene, automatisch generierte Einträge werden ersetzt, alles andere bleibt unangetastet, auch über `! Reset` hinweg

### Installation

Siehe [ThemeStage-Erste-Schritte.md](ThemeStage-Erste-Schritte.md) für eine ausführliche Schritt-für-Schritt-Anleitung, inklusive wo man neue Themes herunterlädt.

Kurzfassung: Konfigurieren → Einstellungen → Plugins → Dateisystem-Plugins → Konfigurieren → Hinzufügen → `ThemeStage.wfx64` (bzw. `ThemeStage.wfx` für 32-Bit) auswählen.

### Konfiguration

Globale Einstellungen stehen in `ThemeStage.ini` (`[ThemMgr]`-Sektion), z. B. `ActiveTheme=`, `AutoColorFilters=`, `AutoRestartForTitle=`, `ColorFilterReloadMethod=`. Jedes einzelne Theme hat zusätzlich seine eigene `themes\active\<Name>.active.ini` mit Helligkeits-Feinjustierung (`FontBrightness=`/`BackgroundBrightness=`) — Details dazu ebenfalls in der Erste-Schritte-Anleitung.

### Build

MinGW-w64 (64-Bit und 32-Bit Toolchain), C++17. `build_release_themestage.bat` erzeugt ein fertiges, upload-bereites Paket inklusive `pluginst.inf`.

### Lizenz

MIT License. © Björn Dubberke / Native2904

---

## English

A Total Commander file-system plugin that loads Windows Terminal color schemes (`.json`) and automatically applies them to TC's panel colors — including matching file-type coloring and contrast checking.

### What it does

- Reads color schemes in the Windows Terminal JSON format (19 standard colors)
- Applies them to TC's own `[Colors]` configuration
- Automatically generates matching file-type colors (8 categories: code, images, video, archives, audio, system files, documents, folders)
- Automatically checks contrast against the theme's background and corrects low-contrast values
- Shows each theme as its own panel entry with a live-rendered preview icon (multiple resolution levels, sharp at any display size)
- Alt+Enter shows all color codes of a theme with an actual color swatch next to each one
- Remembers per-theme, manually adjusted file-type colors in TC (optional, see `AutoColorFilters=`)
- Displays the active theme in TC's window title
- Theme changes take effect immediately, without restarting TC (requires TC 10.50+; falls back to a restart automatically on older TC, see `AutoRestartForTitle=`)
- Recognizes and preserves file-type color filters belonging to other tools (e.g. the Autorun plugin) — only ThemeStage's own, automatically generated entries get replaced, everything else is left untouched, even across `! Reset`

### Installation

See [ThemeStage-Erste-Schritte.md](ThemeStage-Erste-Schritte.md) (German) for a detailed step-by-step guide, including where to download new themes.

Short version: Configuration → Options → Plugins → File System Plugins → Configure → Add → select `ThemeStage.wfx64` (or `ThemeStage.wfx` for 32-bit).

### Configuration

Global settings live in `ThemeStage.ini` (`[ThemMgr]` section), e.g. `ActiveTheme=`, `AutoColorFilters=`, `AutoRestartForTitle=`, `ColorFilterReloadMethod=`. Each individual theme additionally has its own `themes\active\<Name>.active.ini` with brightness fine-tuning (`FontBrightness=`/`BackgroundBrightness=`) — details also in the getting-started guide.

### Build

MinGW-w64 (64-bit and 32-bit toolchain), C++17. `build_release_themestage.bat` produces a ready-to-upload package including `pluginst.inf`.

### License

MIT License. © Björn Dubberke / Native2904
