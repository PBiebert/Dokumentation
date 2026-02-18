[Zurück zum Inhaltsverzeichnis](../README.md)

# Git – Änderungen und Commits rückgängig machen

## Inhaltsverzeichnis

1. [Überblick: HEAD, Staging Area, Arbeitsverzeichnis](#überblick-head-staging-area-arbeitsverzeichnis)
2. [git reset](#1-git-reset)
3. [git revert](#2-git-revert)
4. [git restore](#3-git-restore)
5. [git checkout (Dateien/Commits)](#4-git-checkout-dateiencommits)
6. [Letzten Commit ändern (amend)](#5-letzten-commit-ändern-amend)
7. [Wichtige Hinweise](#wichtige-hinweise)

## Überblick: HEAD, Staging Area, Arbeitsverzeichnis

| Befehl              | HEAD           | Staging Area  | Arbeitsverzeichnis      | Historie                     |
| ------------------- | -------------- | ------------- | ----------------------- | ---------------------------- |
| reset --soft        | bewegt         | bleibt        | bleibt                  | verändert                    |
| reset --mixed       | bewegt         | zurückgesetzt | bleibt                  | verändert                    |
| reset --hard        | bewegt         | zurückgesetzt | zurückgesetzt           | verändert                    |
| revert              | neuer Commit   | bleibt        | bleibt                  | bleibt erhalten              |
| restore             | bleibt         | optional      | optional                | bleibt erhalten              |
| checkout -- <Datei> | bleibt         | bleibt        | Datei wiederhergestellt | bleibt erhalten              |
| commit --amend      | letzter Commit | bleibt        | bleibt                  | letzter Commit überschrieben |

## 1. git reset

`git reset` bewegt den HEAD-Zeiger und kann optional die Staging Area und das
Arbeitsverzeichnis verändern.

- **Soft Reset:**
  ```bash
  git reset --soft HEAD~1
  ```

  - Entfernt den letzten Commit, Änderungen bleiben gestaged.
- **Mixed Reset (Standard):**
  ```bash
  git reset --mixed HEAD~1
  ```

  - Entfernt den letzten Commit, Änderungen gehen zurück ins Arbeitsverzeichnis.
- **Hard Reset:**
  ```bash
  git reset --hard HEAD~1
  ```

  - Entfernt den letzten Commit und alle Änderungen im Arbeitsverzeichnis (nicht
    rückgängig zu machen!).

> **Achtung:** `reset` verändert die Historie. Nur für lokale Commits verwenden!

## 2. git revert

`git revert` erstellt einen neuen Commit, der einen früheren Commit rückgängig
macht. Die Historie bleibt erhalten.

```bash
git revert <commitID>
```

- Sicher für öffentliche Branches, da die Historie erhalten bleibt.
- Besonders nützlich, wenn ein fehlerhafter Commit bereits gepusht wurde.

## 3. git restore

`git restore` wird verwendet, um Dateien auf einen früheren Stand
zurückzusetzen.

- Arbeitsverzeichnis wiederherstellen:
  ```bash
  git restore <Datei>
  ```
- Änderungen aus der Staging Area entfernen:
  ```bash
  git restore --staged <Datei>
  ```

## 4. git checkout (Dateien/Commits)

- Eine Datei wiederherstellen:
  ```bash
  git checkout -- <Datei>
  ```

  - Stellt eine Datei aus dem letzten Commit wieder her.
- Zu einem bestimmten Commit wechseln (detached HEAD):
  ```bash
  git checkout <commit-id>
  ```

  - HEAD zeigt nun direkt auf einen Commit, nicht auf einen Branch.

> **Tipp:** Für das Wechseln von Branches und das Wiederherstellen von Dateien
> werden inzwischen `git switch` und `git restore` empfohlen.

## 5. Letzten Commit ändern (amend)

- Letzte Commit-Nachricht ändern:
  ```bash
  git commit --amend
  ```
- Dateien zum letzten Commit hinzufügen:
  ```bash
  git add .
  git commit --amend
  ```
- **Achtung:** Amend nur für lokale Commits verwenden! Bereits gepushte Commits
  nur mit `git push --force` überschreiben (gefährlich für Teamarbeit).

### Commit-Nachricht im Vim-Editor bearbeiten

Wenn du `git commit --amend` ausführst und sich der Vim-Editor öffnet:

1. Drücke `i`, um den Einfügemodus zu starten und die Commit-Nachricht zu
   bearbeiten.
2. Wenn du fertig bist, drücke `Esc`, um in den normalen Modus zurückzukehren.
3. Gib `:wq` ein und drücke `Enter`, um zu speichern und Vim zu verlassen.

**Schnelle Vim-Befehle:**

- `i` → Einfügemodus (Text bearbeiten)
- `Esc` → Normalmodus
- `:wq` → Speichern und beenden
- `:q!` → Beenden ohne zu speichern

Nach dem Verlassen von Vim wird dein geänderter Commit gespeichert.

## Wichtige Hinweise

- Verwende `reset` für lokale Änderungen, `revert` für öffentliche Commits und
  `restore` für gezielte Dateiwiederherstellung.
- `amend` eignet sich für kleine Korrekturen am letzten Commit, solange dieser
  nicht gepusht wurde.
- Im Zweifel: Historie lieber erhalten (revert/restore) als löschen
  (reset/hard).
