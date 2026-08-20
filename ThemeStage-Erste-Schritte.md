# ThemeStage — Erste Schritte

## Installation

1. In Total Commander: **Konfigurieren → Einstellungen → Plugins → Dateisystem-Plugins → Konfigurieren → Hinzufügen**
2. `ThemeStage.wfx64` (bzw. `ThemeStage.wfx` für 32-Bit) auswählen
3. Fertig — das Plugin ist ab jetzt über `\\themestage\` erreichbar, z. B. indem man den Pfad direkt in die TC-Adressleiste tippt

## Was beim allerersten Start passiert

Der Ablauf läuft automatisch, ohne dass man etwas einstellen muss:

1. **`\\themestage\` öffnen** — das Plugin legt beim ersten Aufruf seine eigene `ThemeStage.ini` und einen leeren `themes\`-Ordner neben sich an
2. **TCs Farbverwaltung wird einmalig umgeleitet.** TC speichert Farben normalerweise direkt in der zentralen `wincmd.ini`. ThemeStage lagert diesen Bereich beim ersten Start in eine eigene `color.ini` aus (TCs eigener `RedirectSection=`-Mechanismus) — das ist notwendig, damit ThemeStage gezielt Farben setzen kann, ohne den Rest der TC-Konfiguration zu berühren. Von der ursprünglichen `wincmd.ini` wird vorher automatisch eine Sicherungskopie angelegt (`wincmd.ini.bak-themgr`)
3. **Der Panel-Ordner ist zunächst leer** (bis auf `[..]`) — das ist normal, es liegt noch kein Theme im `themes\`-Ordner

## Woher bekommt man Themes?

ThemeStage liest Farbschemata im **Windows-Terminal-Format** (`.json`). Eine große, kostenlose Sammlung gibt es unter:

**https://isherlock.github.io/terminal-themes/**

So geht's:

1. Auf der Webseite ein Theme aussuchen, das gefällt
2. Die zugehörige `.json`-Datei herunterladen
3. Die Datei in den `themes\`-Ordner legen, der direkt neben `ThemeStage.wfx64` liegt
4. `\\themestage\` in TC neu öffnen (oder einfach kurz raus- und wieder reinnavigieren) — das neue Theme erscheint automatisch im Panel, inklusive eines kleinen Vorschau-Icons

Es lassen sich beliebig viele `.json`-Dateien gleichzeitig in den Ordner legen — jede erscheint als eigener, wählbarer Eintrag.

## Ein Theme auswählen

Einfach **Enter** auf einem Theme-Eintrag drücken. Das Plugin:

- berechnet die Farben aus der `.json`-Datei
- schreibt sie in TCs Farbdatei
- passt automatisch die Dateityp-Farben an (Code, Bilder, Videos, Archive usw. bekommen jeweils eine passende Farbe aus dem gewählten Theme)
- prüft dabei den Kontrast: Texte, die auf dem neuen Hintergrund kaum lesbar wären, werden automatisch aufgehellt oder abgedunkelt

**Wichtig:** TC übernimmt eine neu gesetzte Farbe nicht immer sofort sichtbar. In den meisten Fällen geschieht das automatisch im Hintergrund (kurzes Umschalten des Dunkelmodus, siehe `AutoBounceDarkmode=` weiter unten). Falls die Farbe trotzdem nicht sofort erscheint: einmal durch **Konfigurieren → Einstellungen → Farben** gehen und mit OK bestätigen — das zwingt TC, die Datei neu einzulesen.

## Details zu einem Theme ansehen

**Alt+Enter** auf einem Theme-Eintrag zeigt:

- alle Farbcodes mit echtem Farbmuster daneben
- ob das Theme als hell oder dunkel eingestuft wurde
- die aktuell verknüpften Helligkeits-Einstellungen

## Helligkeit pro Theme feinjustieren

Jedes Theme hat seine eigene, kleine Einstellungsdatei:

```
themes\active\<Themename>.active.ini
```

```ini
[ThemMgr]
FontBrightness=0        ; -3 bis +3, verschiebt die Vordergrundfarben
BackgroundBrightness=0  ; -3 bis +3, verschiebt die Hintergrundfarbe
```

Diese Datei wird **automatisch** angelegt, sobald ein Theme zum ersten Mal gefunden wird — von Hand muss hier normalerweise nichts eingetragen werden. Wer aber eine Farbe eines bestimmten Themes minimal heller oder dunkler haben möchte, kann die Werte direkt hier anpassen (Werte außerhalb von -3 bis +3 werden automatisch auf den gültigen Bereich begrenzt). Diese Einstellung ist **pro Theme getrennt** — jedes Theme kann seine eigene Feinjustierung haben, unabhängig von allen anderen.

Bei `AutoColorFilters=0` steht in derselben Datei zusätzlich ein `[ColorFilters]`-Abschnitt — das ist die automatische Aufzeichnung der zuletzt für dieses Theme aktiven Dateityp-Farben, wird vom Plugin selbst verwaltet.

## Wichtige Einstellungen (`ThemeStage.ini`)

```ini
[ThemMgr]
ActiveTheme=                  ; welches Theme aktuell aktiv ist (wird beim Auswaehlen automatisch gesetzt)
AutoColorFilters=1            ; 1 = Dateityp-Farben automatisch generieren
                               ; 0 = eigene, manuell in TC gesetzte Dateityp-Farben bleiben erhalten
ColorIniPathOverride=         ; optional: eigener, fester Pfad zur Farbdatei statt automatischer Ermittlung
```

Eine neu gewählte Farbe wird normalerweise **automatisch** sofort sichtbar, ganz ohne manuellen Umweg über den Farben-Dialog. Sollte das auf einem System einmal nicht zuverlässig funktionieren, lässt sich das über `AutoBounceDarkmode=0` in der `ThemeStage.ini` abschalten (dann wieder wie oben beschrieben: einmal manuell durch **Konfigurieren → Einstellungen → Farben** gehen).

## Eigene, bereits vorhandene TC-Farben übernehmen

Wer schon eine eigene Farbkonfiguration in TC hat, bevor ThemeStage zum ersten Mal läuft, kann diese als eigenes Theme importieren:

```ini
[ThemMgr]
ImportLegacyColors=1
```

Einmal setzen, TC neu starten — ThemeStage erzeugt daraus automatisch ein Theme namens „Return_to_Default" im `themes\`-Ordner (nur die vier direkt übertragbaren Grundfarben, keine erfundenen Werte).
