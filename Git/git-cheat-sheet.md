[Zurück zur Übersicht](../README.md)

# Git Cheat Sheet – Die wichtigsten Git-Befehle auf einen Blick

---

## Commit Messages richtig schreiben

Jedes Mal, wenn du Änderungen an einem gemeinsamen Projekt vornimmst, solltest
du eine Commit-Message schreiben, um dein Team zu informieren.

### Commit-Typen

| Commit-Typ | Bedeutung             | Wann verwenden?                   | Beispiel                                    |
| ---------- | --------------------- | --------------------------------- | ------------------------------------------- |
| feat       | Neue Funktionalität   | Für neue Features                 | feat: add login page                        |
| fix        | Fehlerbehebung        | Für Bugfixes                      | fix: resolve image loading flicker          |
| docs       | Dokumentation         | Änderungen an Doku                | docs: update README with setup instructions |
| style      | Code-Formatierung     | Nur Format, kein Code-Change      | style: fix indentation in main.js           |
| refactor   | Code-Umstrukturierung | Keine neue Funktion / kein Bugfix | refactor: simplify login flow logic         |
| perf       | Performance           | Leistungsverbesserungen           | perf: reduce code to under 400 lines        |
| test       | Tests                 | Tests hinzufügen oder ändern      | test: add unit tests for login component    |

---

## Tipps für gute Commit Messages

- Deskriptiver Titel
- Detaillierte Beschreibung
  - Was war erwartet?
  - Was ist passiert?
  - Schritte zum Reproduzieren (bei Fehlern)
  - Screenshots oder Logs (falls hilfreich)
  - Angabe der Umgebung (Browser, OS, etc.)
- Verwendung von Labels (z. B. Bug, Verbesserung)

---

## Repository initialisieren & klonen

### Neues Repository anlegen

```bash
git init <directory>
```

### Remote Repository herunterladen

```bash
git clone <url> <path>
```

- `<path>` ist optional und kann auch `.` sein

---

## Lokaler Workflow

### Änderungen zum Commit vorbereiten

```bash
git add <file>
```

- `<file>` kann auch `.` sein (fügt alle Änderungen im Working Directory hinzu)

### Änderungen speichern mit Nachricht

```bash
git commit -m "Commit Message"
```

---

## Änderungen & Historie überprüfen

### Aktuellen Stand und Änderungen anzeigen

```bash
git status
```

### Commit-Historie anzeigen

```bash
git log
```

### Unterschiede anzeigen (noch nicht staged)

```bash
git diff
```

### Commit einsehen, aber nicht speichern

```bash
git checkout <commit_ID>
```

---

## Änderungen rückgängig machen

### Commit entfernen und Änderungen zurückstellen

```bash
git reset <flag> HEAD~1
```

**Mögliche Flags:**

- `--soft`
- `--mixed`
- `--hard`

### Änderung eines Commits rückgängig machen (neuer Commit)

```bash
git revert <commit_ID>
```

---

## Remote Workflow (Zusammenarbeit)

### Änderungen zu GitHub hochladen

```bash
git push
```

### Neueste Änderungen von GitHub holen und mergen

```bash
git pull
```

---

## Git Branches

### Lokale Branches anzeigen

```bash
git branch
```

### Alle Branches (lokal & remote) anzeigen

```bash
git branch -av
```

### Neuen Branch anlegen

```bash
git branch <new-branch>
```

### Zu einem anderen Branch wechseln

```bash
git checkout <branchname>
```

### Einen Branch in den aktuellen mergen

```bash
git checkout <branch_name1>
git merge <branch_name2>
```

### Branch löschen

```bash
git branch -d <branch_name>
```

---
