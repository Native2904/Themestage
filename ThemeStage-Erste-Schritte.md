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

**Wichtig:** Ab TC 10.50 wirkt ein Themewechsel sofort — Farben und Fenstertitel aktualisieren sich live, ohne dass TC neu startet. Dafür nutzt ThemeStage einen eigenen, offiziell dokumentierten TC-Befehl (`cm_SwitchColorsByFileType`), der TC anweist, seine Farbeinstellungen neu einzulesen. Bei älteren TC-Versionen (vor 10.50) fällt das Plugin automatisch auf einen kompletten Neustart zurück (kurzes Aufblitzen des Fensters, technisch aber genauso zuverlässig) — das lässt sich in `ThemeStage.ini` auch manuell erzwingen, siehe unten.

## Dunkelmodus

ThemeStage erkennt automatisch, ob TCs eigener Dunkelmodus auf "Immer aktiviert" steht, und schreibt die Theme-Farben dann in die passende Sektion (`[ColorsDark]` statt `[Colors]`). Das bedeutet: Wer TCs Oberfläche grundsätzlich dunkel haben möchte (Menüs, Dialoge), kann trotzdem jedes beliebige — auch helle — Theme für die Dateiliste selbst wählen. Kein eigener Schalter nötig, läuft komplett automatisch.

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
AutoRestartForTitle=0         ; 0 = Standard, Themewechsel wirken sofort ohne Neustart (TC 10.50+)
                               ; 1 = Themewechsel startet TC komplett neu (fuer TC vor 10.50, oder
                               ;     falls der Live-Weg bei dir aus irgendeinem Grund nicht sauber
                               ;     laeuft - dann als Ruecksprung-Option gedacht)
                               ; Betrifft NUR normale Themewechsel - "! Reset" startet TC IMMER neu,
                               ; unabhaengig von dieser Einstellung (siehe Abschnitt weiter unten)
ColorFilterReloadMethod=1     ; nur wirksam bei AutoRestartForTitle=0 - welcher Live-Weg genutzt wird,
                               ; normalerweise nicht noetig anzufassen
ColorIniPathOverride=         ; optional: eigener, fester Pfad zur Farbdatei statt automatischer Ermittlung
DebugLogging=0                ; 1 = Protokolldatei (ThemeStage_debug.log) schreiben - nur zur Fehlersuche
```

## Alles rückgängig machen — `! Reset`

Ganz oben im Panel steht immer ein fester Eintrag: **`! Reset`**. Das ist der Ausschalter des Plugins — Enter darauf macht **alles** rückgängig, was ThemeStage jemals verändert hat:

- Farben werden aus einer beim allerersten Start automatisch angelegten Sicherung (`wincmd.ini.bak-themgr`) wiederhergestellt
- Der Fenstertitel wird entfernt
- `ActiveTheme=` wird geleert — das Plugin ist danach wieder im reinen Ausgangszustand (Opt-in, greift in nichts ein)

Das läuft **immer** über einen echten TC-Neustart — unabhängig von `AutoRestartForTitle=`/`ColorFilterReloadMethod=` oben, die nur normale Themewechsel betreffen. Reset entfernt nämlich die Farbumleitung selbst (TC soll danach wieder direkt aus `wincmd.ini` statt aus `color.ini` lesen), und das erkennt TC zuverlässig nur bei seinem eigenen Programmstart. Die Wirkung ist trotzdem sofort sichtbar, nur eben mit dem kurzen Aufblitzen des Fensters, das ein Neustart mit sich bringt.
