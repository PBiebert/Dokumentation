[Zurück zum Inhaltsverzeichnis](../README.md)

# Git – Schritt für Schritt: Von der Initialisierung zum Remote-Repository

Diese Anleitung zeigt die typischen ersten Schritte beim Start eines neuen
Projekts mit Git, inklusive Branch-Umbenennung und Verbindung zu einem
Remote-Repository.

---

## Inhaltsverzeichnis

1. [Neues Projektverzeichnis erstellen](#1-neues-projektverzeichnis-erstellen)
2. [Lokales Git-Repository initialisieren](#2-lokales-git-repository-initialisieren)
3. [Branch auf `main` umbenennen (optional)](#3-branch-auf-main-umbenennen-optional)
4. [Dateien hinzufügen und ersten Commit erstellen](#4-dateien-hinzufügen-und-ersten-commit-erstellen)
5. [Neues Remote-Repository erstellen (z.B. auf GitHub)](#5-neues-remote-repository-erstellen-zb-auf-github)
6. [Lokales Repository mit Remote verbinden](#6-lokales-repository-mit-remote-verbinden)
7. [Lokalen Branch zum Remote pushen](#7-lokalen-branch-zum-remote-pushen)
8. [Status und Workflow prüfen](#8-status-und-workflow-prüfen)
9. [Typischer Workflow (Zusammenfassung)](#9-typischer-workflow-zusammenfassung)

---

## 1. Neues Projektverzeichnis erstellen

Erstelle und betrete deinen neuen Projektordner:

```bash
mkdir mein-projekt
cd mein-projekt
```

---

## 2. Lokales Git-Repository initialisieren

```bash
git init
```

Dadurch wird ein versteckter `.git`-Ordner erstellt und dein Verzeichnis zu
einem Git-Repository.

---

## 3. Branch auf `main` umbenennen (optional)

Standardmäßig verwendet Git eventuell `master` als Hauptbranch. Um ihn auf
`main` umzubenennen (empfohlen):

```bash
git branch -m main
```

---

## 4. Dateien hinzufügen und ersten Commit erstellen

Füge alle Dateien (oder bestimmte Dateien) zum Staging-Bereich hinzu:

```bash
git add .
```

Committe deine Änderungen:

```bash
git commit -m "Initialer Commit"
```

---

## 5. Neues Remote-Repository erstellen (z.B. auf GitHub)

Gehe zu GitHub (oder einem anderen Git-Hosting-Service) und erstelle ein neues,
leeres Repository. Initialisiere es nicht mit README oder .gitignore, falls du
diese lokal schon hast.

---

## 6. Lokales Repository mit Remote verbinden

Ersetze `<URL>` durch die URL deines Repositories (z.B.
`https://github.com/username/mein-projekt.git`):

```bash
git remote add origin <URL>
```

---

## 7. Lokalen Branch zum Remote pushen

```bash
git push -u origin main
```

Das `-u`-Flag setzt `origin/main` als Standard-Upstream-Branch.

---

## 8. Status und Workflow prüfen

Prüfe den Status deines Arbeitsverzeichnisses und Staging-Bereichs:

```bash
git status
```

---

## 9. Typischer Workflow (Zusammenfassung)

1. Änderungen an deinen Dateien machen
2. Änderungen zum Staging hinzufügen: `git add .`
3. Commit: `git commit -m "deine Nachricht"`
4. Push: `git push`

---

Damit sind die wichtigsten ersten Schritte für jedes neue Git-Projekt abgedeckt
– von der Initialisierung bis zum Remote-Setup und ersten Push.
