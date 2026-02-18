[Zurück zum Inhaltsverzeichnis](../README.md)

# Git – Repository-Struktur: README, .gitignore und LICENSE

## .gitignore

- **Zweck:** Schließt Dateien oder Ordner von der Git-Verfolgung aus, z.B.
  sensible Daten wie `.env`.
- **Ort:** Im Hauptordner deines Projekts.
- **Funktionsweise:**
  - Liste die zu ignorierenden Dateien oder Ordner in der `.gitignore`-Datei
    auf.

**Beispiel `.gitignore`:**

```
.env
node_modules/
.DS_Store
```

- **Ergebnis:** Diese Dateien werden nicht mehr committet, selbst wenn du
  `git add .` ausführst.

## README.md

- **Zweck:** Dokumentation für das Projekt, erklärt die Nutzung des Codes.
- **Mögliche Inhalte:**
  - Hinweise und Links
  - Dokumentation
  - Changelog
  - Installationsanleitung
  - Beispiele
- **Regeln / Tipps:**
  1. Struktur ähnlich wie HTML (Überschriften, Listen, Absätze)
  2. Klar, prägnant und lesbar schreiben

## LICENSE

- **Zweck:** Definiert Rechte und Pflichten bezüglich der Nutzung deines Codes.
- **Beispiele:** MIT-Lizenz, GPL, Apache-Lizenz
- **MIT-Lizenz:**
  - Sehr verbreitet und einfach
  - Enthält meist Haftungsausschluss und Nutzungsrechte
  - Wichtig, damit andere wissen, was sie mit deinem Code machen dürfen
