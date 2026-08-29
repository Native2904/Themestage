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

## Mehrere TC-Fenster gleichzeitig offen

Läuft TC bei dir öfter mit mehreren Fenstern gleichzeitig (z. B. zwei oder drei separate TC-Instanzen nebeneinander), lohnt sich ein kurzer Blick auf dieses Verhalten — kein Fehler, aber gut zu wissen, damit nichts überraschend wirkt.

**Alle TC-Fenster teilen sich dieselbe Farbdatei.** Wählst du in einem Fenster ein Theme, wird das in genau diese eine, gemeinsame Datei geschrieben. Wählst du kurz danach in einem anderen Fenster ein anderes Theme, wird dieselbe Datei einfach erneut überschrieben — mit dem neuen Wert.

**Ein Beispiel:**

1. Fenster A: Theme "Dracula" ausgewählt
2. Fenster B (kurz danach): Theme "Nord" ausgewählt
3. Fenster C (wieder kurz danach): Theme "Gruvbox" ausgewählt

Am Ende steht in der Datei schlicht **Gruvbox** — der zuletzt geschriebene Wert, unabhängig davon, welches Fenster zuerst geöffnet oder zuletzt geschlossen wurde. Es zählt einzig der Zeitpunkt der tatsächlichen Auswahl.

**Was du dabei siehst:** Direkt nach den drei Auswahlen zeigt jedes der drei Fenster erstmal weiterhin **sein eigenes**, zuletzt selbst gewähltes Theme — Fenster A zeigt also noch Dracula, Fenster B noch Nord, obwohl in der Datei längst Gruvbox steht. Das ist keine fehlerhafte oder beschädigte Datei, sondern schlicht: jedes Fenster zeigt nur das, was es selbst zuletzt ausgelöst hat, und bekommt von den Aktionen der anderen Fenster nichts automatisch mit. Diese Anzeige gleicht sich von selbst wieder an, sobald ein Fenster das nächste Mal selbst aktiv wird (nächster Themewechsel in genau diesem Fenster, oder ein normaler TC-Neustart).

**Kurz gesagt:** Mehrere gleichzeitig offene TC-Fenster vertragen sich technisch problemlos miteinander — es entsteht keine beschädigte Datei, kein Datenverlust. Nur die Anzeige kann für einen Moment auseinanderlaufen, bis sich alles wieder angleicht. Wer regelmäßig mit mehreren TC-Fenstern arbeitet und immer dieselbe Farbe überall sehen möchte, sollte ein Theme einfach nur in **einem** Fenster wechseln und das andere anschließend kurz neu laden (z. B. `\\themestage\` einmal verlassen und wieder betreten).

## Zusammenspiel mit anderen Plugins (z. B. Autorun)

Nutzt du neben ThemeStage ein weiteres Tool, das eigene Dateityp-Farbfilter in TC verwaltet — etwa das Autorun-Plugin, das oft einen eigenen Inhalts-Filter fest auf einer bestimmten Position erwartet — erkennt ThemeStage solche fremden Einträge automatisch und lässt sie in Ruhe. Nur die Einträge, die ThemeStage selbst bei `AutoColorFilters=1` generiert, werden bei einem Themewechsel ersetzt; alles andere bleibt unverändert an seinem Platz stehen, auch über `! Reset` hinweg. Kein eigener Schalter nötig, läuft automatisch.

## Dunkelmodus

ThemeStage erkennt automatisch, ob TCs eigener Dunkelmodus auf "Immer aktiviert" steht, und schreibt die Theme-Farben dann in die passende Sektion (`[ColorsDark]` statt `[Colors]`). Das bedeutet: Wer TCs Oberfläche grundsätzlich dunkel haben möchte (Menüs, Dialoge), kann trotzdem jedes beliebige — auch helle — Theme für die Dateiliste selbst wählen. Kein eigener Schalter nötig, läuft komplett automatisch.

## Details zu einem Theme ansehen

**Alt+Enter** auf einem Theme-Eintrag öffnet ein eigenes, dunkel gestaltetes Dashboard-Fenster mit:

- allen UI-Farben (Hintergrund, Vordergrund, Cursor, Auswahl) als Farbkarten mit Hex-Code
- der kompletten ANSI-Palette (16 Farben), jeweils mit Kontrastverhältnis und Bewertung (AA / AA Large / Fail) gegen den Theme-Hintergrund
- ob das Theme als hell oder dunkel eingestuft wurde
- den aktuell verknüpften Helligkeits-Einstellungen sowie dem `AutoColorFilters=`-Stand
- einem Hinweis, falls beim Laden unvollständige oder ungültige Farbangaben ersetzt werden mussten

## Farben direkt bearbeiten — ThemeLister.wlx

ThemeStage bringt ein eigenes Lister-Plugin mit, `ThemeLister.wlx64` (bzw. `.wlx` für 32-Bit). Einmal installiert, macht es aus demselben dunklen Dashboard von Alt+Enter einen vollwertigen Farbeditor, direkt in TCs eigenem Datei-Betrachter:

1. Einmalig installieren: **Konfigurieren → Einstellungen → Plugins → Lister-Plugins → Konfigurieren → Hinzufügen** → `ThemeLister.wlx64` auswählen
2. Einen beliebigen Theme-Eintrag in `\\themestage\` auswählen und **F3** (Lister) oder **Strg+Q** (QuickView) drücken
3. Auf eine der 20 Farbkarten klicken öffnet den normalen Windows-Farbwähler zum Ändern
4. **Strg+S** speichert — schreibt direkt in die echte `.json`-Datei dieses Themes zurück

Ein paar Dinge dazu:

- Beim allerersten Speichern eines Themes legt ThemeStage automatisch eine einmalige Sicherung an (`<Theme>.json.bak-themelister`) direkt daneben — ein versehentlicher Klick lässt sich also jederzeit von Hand rückgängig machen
- Das funktioniert nur für Themes, die schon in `themes\` liegen — ThemeLister rührt keine anderen, unabhängigen `.json`-Dateien auf deinem System an, es reagiert gezielt nur auf Dateien, die ThemeStage selbst beim Drücken von F3/Strg+Q übergibt
- Keine `.active.ini`-Einstellungen (Helligkeits-Feinjustierung, `AutoColorFilters=`) werden hier angezeigt oder verändert — das ist eine separate Datei (siehe "Helligkeit pro Theme feinjustieren" weiter unten). Strg+S speichert hier ausschließlich in `themes\<Name>.json` — die rohe Farbquelle, nicht die Helligkeits-/Filter-Einstellungsdatei

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
DebugLogging=1                ; 1 = Protokolldatei (ThemeStage_debug.log) schreiben - neuer Standard,
                               ;     weil ein echter Datenverlust-Fall ohne Log schwer zu diagnostizieren
                               ;     war; kann auf 0 zurueckgestellt werden, sobald alles stabil laeuft
```

## Alles rückgängig machen — `! Reset`

Ganz oben im Panel steht immer ein fester Eintrag: **`! Reset`**. Das ist der Ausschalter des Plugins — Enter darauf macht **alles** rückgängig, was ThemeStage jemals verändert hat:

- Farben werden aus einer beim allerersten Start automatisch angelegten Sicherung (`wincmd.ini.bak-themgr`) wiederhergestellt
- Der Fenstertitel wird entfernt
- `ActiveTheme=` wird geleert — das Plugin ist danach wieder im reinen Ausgangszustand (Opt-in, greift in nichts ein)

Das läuft **immer** über einen echten TC-Neustart — unabhängig von `AutoRestartForTitle=`/`ColorFilterReloadMethod=` oben, die nur normale Themewechsel betreffen. Reset entfernt nämlich die Farbumleitung selbst (TC soll danach wieder direkt aus `wincmd.ini` statt aus `color.ini` lesen), und das erkennt TC zuverlässig nur bei seinem eigenen Programmstart. Die Wirkung ist trotzdem sofort sichtbar, nur eben mit dem kurzen Aufblitzen des Fensters, das ein Neustart mit sich bringt.
