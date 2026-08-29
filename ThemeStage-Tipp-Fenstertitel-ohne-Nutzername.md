# Kleiner Hack: Nutzernamen aus dem TC-Fenstertitel entfernen

**Kein offizielles Feature von ThemeStage** — sondern ein Nebeneffekt der Art, wie ThemeStage den Fenstertitel live aktualisiert. Bewusst dokumentiert, weil er nützlich ist, aber bewusst *nicht* als vollwertige, beworbene Einstellung ausgebaut.

## Was das Ganze ist

ThemeStage muss bei jedem Themewechsel den Fenstertitel neu zusammensetzen (`Total Commander ... - <Theme-Name>`), ohne TC dafür neu zu starten. Damit das zuverlässig funktioniert, merkt sich das Plugin einmalig eine "saubere Basis" — alles, was vor dem eigenen, angehängten Theme-Namen steht — und speichert sie in `ThemeStage.ini`:

```ini
WindowTitleBase=Total Commander (x64) 11.58 - John Doe
```

Bei jedem weiteren Themewechsel wird der Titel **immer** aus genau diesem gespeicherten Wert plus dem aktuellen Theme-Namen neu gebaut — nie aus dem, was gerade zufällig im Fenster steht.

## Der Hack

Der gespeicherte Wert wird **nur einmal automatisch ermittelt** und danach nie wieder von ThemeStage selbst verändert. Trägt man von Hand einen **eigenen** Wert ein — zum Beispiel ohne den Nutzernamen —

```ini
WindowTitleBase=Total Commander (x64) 11.58
```

— dann übernimmt ThemeStage diesen Wert unverändert und baut ab sofort jeden Titel darauf auf: `Total Commander (x64) 11.58 <Theme-Name>`. Kein Nutzername mehr, dauerhaft, ganz ohne dass ThemeStage dafür extra Code bräuchte.

## Bewusste Grenzen — das ist wichtig

- **Nur für den Fall gedacht, in dem ohnehin schon "Nutzername raus" gewünscht ist** — nicht als allgemeine Fenstertitel-Anpassung. Wer etwas völlig anderes ins Fenster schreiben will, ist damit nicht bedient und sollte ThemeStage nicht dafür zweckentfremden.
- **Von Hand eingetragen, von Hand gepflegt.** Ändert sich TCs eigene Versionsnummer (`11.58` → `11.59` bei einem Update), passt der von Hand eingetragene Wert nicht mehr automatisch mit — das ist kein Bug, sondern die erwartete Grenze dieses Hacks.
- **`! Reset` löscht den Wert wieder** — danach wird beim nächsten Themewechsel erneut automatisch (mit Nutzername) eingefangen. Wer den Hack dauerhaft will, muss ihn nach einem Reset erneut von Hand eintragen.
- **Bleibt trotzdem der ganz normale Betrieb, nichts Kaputtes dabei.** Wer nie von Hand in `WindowTitleBase=` eingreift, merkt von alldem nichts — die automatische Erstermittlung läuft im Hintergrund genau wie gehabt.

## Kurz für die Praxis

1. `ThemeStage.ini` öffnen
2. `WindowTitleBase=` suchen (existiert erst nach dem ersten Themewechsel)
3. Wert von Hand auf das kürzen, was stehen bleiben soll — z. B. nur `Total Commander (x64) 11.58`
4. Nächster Themewechsel übernimmt das automatisch, dauerhaft, bis zum nächsten `! Reset`
