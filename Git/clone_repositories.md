[Zurück zum Inhaltsverzeichnis](../README.md)

# Git – Repositories klonen

Mit `git clone` kopierst du ein entferntes Repository (z.B. von GitHub) auf
deinen lokalen Rechner.

## Inhaltsverzeichnis

1. [Befehl](#befehl)
2. [Option: Direkt ins aktuelle Verzeichnis klonen](#option-direkt-ins-aktuelle-verzeichnis-klonen)
3. [Zusammenfassung](#zusammenfassung)

## Befehl

```bash
git clone <GitHub-URL>
```

- `<GitHub-URL>` ist die Adresse des Repositories (HTTPS oder SSH).
- Die URL findest du auf der GitHub-Seite des Repositories über den
  "Code"-Button.
- Alternativ kannst du das Repository als ZIP herunterladen, verlierst aber die
  Git-Historie und Versionskontrolle.

## Option: Direkt ins aktuelle Verzeichnis klonen

```bash
git clone <GitHub-URL> .
```

- Der Punkt (`.`) am Ende bedeutet: direkt ins aktuelle Verzeichnis kopieren.
- Vorteil: Es wird kein neuer Unterordner erstellt, alles wird direkt in dein
  aktuelles Verzeichnis kopiert.
- Praktisch, wenn du bereits einen Projektordner vorbereitet hast.

## Zusammenfassung

| Variante            | Ergebnis                                                                      |
| ------------------- | ----------------------------------------------------------------------------- |
| `git clone <URL>`   | Erstellt einen neuen Ordner mit dem Repository-Namen und kopiert alles hinein |
| `git clone <URL> .` | Kopiert alle Dateien direkt ins aktuelle Verzeichnis, kein neuer Ordner       |
