[Zurück zum Inhaltsverzeichnis](../README.md)

# Git – Typischer Workflow

Ein strukturierter Workflow hilft, Fehler und Konflikte zu vermeiden und sorgt
für eine saubere Versionshistorie.

## 1. Immer zuerst die neuesten Änderungen ziehen

Bevor du etwas committest, solltest du die neuesten Änderungen aus dem
Remote-Repository (z.B. GitHub) holen:

```bash
git pull
```

- Holt die neuesten Änderungen vom Remote (meist `origin main`) und integriert
  sie in dein lokales Repository.

## 2. Dateien zum Commit vormerken (stagen)

Wenn du eine oder mehrere Dateien bearbeitet hast:

```bash
git add file.txt
```

- Merkt eine Datei für den nächsten Commit vor.

Oder alle Änderungen stagen:

```bash
git add .
```

- Merkt alle geänderten Dateien vor.

## 3. Commit erstellen

Erstelle einen Commit mit einer aussagekräftigen Nachricht:

```bash
git commit -m "Kurze, klare Beschreibung der Änderung"
```

- Speichert deine Änderungen dauerhaft im Repository.

## 4. Änderungen pushen

Damit andere (oder du auf einem anderen Computer) deine Änderungen sehen können:

```bash
git push
```

- Pusht deine Commits ins Remote-Repository (z.B. GitHub).

## Kurzversion (immer gleiche Reihenfolge)

```bash
git pull               # 1. Neueste Änderungen holen
git add .              # 2. Änderungen stagen
git commit -m "..."    # 3. Commit mit Nachricht
git push               # 4. Änderungen pushen
```

## Nützliche Extras

- **Status prüfen:**

  ```bash
  git status
  ```

  Zeigt, welche Dateien geändert wurden oder bereits zum Commit vorgemerkt sind.

- **Letzte Commits anzeigen:**

  ```bash
  git log --oneline --graph --decorate
  ```

  Zeigt die Historie kompakt und übersichtlich an.

- **Konflikte nach git pull lösen:**
  1. Bearbeite die betroffenen Abschnitte in den Dateien manuell.
  2. Stag die Änderungen erneut mit `git add` und committe mit `git commit`.
  3. Danach wie gewohnt mit `git push` hochladen.

## Wichtig

Halte dich an die Reihenfolge: `pull → add → commit → push`. So bleibt dein
Repository synchron und Konflikte werden minimiert.
