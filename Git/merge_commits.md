[Zurück zum Inhaltsverzeichnis](../README.md)

# Git – Merge-Commits und Merges rückgängig machen

## Inhaltsverzeichnis

1. [Grundlagen eines Merge-Commits](#1-grundlagen-eines-merge-commits)
2. [Merge abbrechen (während des Vorgangs)](#2-merge-abbrechen-während-des-vorgangs)
3. [Merge rückgängig machen (nach Commit)](#3-merge-rückgängig-machen-nach-commit)
   - [Option 1: git reset (nur lokal)](#option-1-using-git-reset-local-only)
   - [Option 2: git revert (empfohlen für Remote)](#option-2-using-git-revert-recommended-for-remote)
4. [Merge erneut durchführen](#4-merge-erneut-durchführen)
5. [Umgang mit Merge-Konflikten](#5-umgang-mit-merge-konflikten)
6. [Wichtiger Hinweis](#key-point)

## 1. Grundlagen eines Merge-Commits

- Ein Merge-Commit verbindet zwei oder mehr Branches.
- Er hat mindestens zwei Eltern-Commits:
  - `HEAD^1`: erster Eltern-Commit (meist der Branch, auf dem du warst)
  - `HEAD^2`: zweiter Eltern-Commit (der Branch, der gemerged wurde)

**Beispiel:**

```bash
git show HEAD         # Zeigt den Merge-Commit
git show HEAD^1       # Zeigt den ersten Eltern-Commit
git show HEAD^2       # Zeigt den zweiten Eltern-Commit
```

## 2. Merge abbrechen (während des Vorgangs)

Wenn du einen Merge gestartet hast, aber Konflikte auftreten oder du abbrechen
möchtest:

```bash
git merge --abort
```

- Bricht den laufenden Merge ab und stellt den Zustand vor dem Merge wieder her.

## 3. Merge rückgängig machen (nach Commit)

### Option 1: git reset (nur lokal!)

Wenn der Merge-Commit noch nicht gepusht wurde:

```bash
git reset --hard HEAD~1
```

- Löscht den Merge-Commit und geht einen Schritt zurück.
- **Achtung:** Nur verwenden, wenn der Commit noch nicht zum Remote gepusht
  wurde!

### Option 2: git revert (empfohlen für Remote!)

Ein Merge-Commit ist speziell, daher brauchst du `-m` ("mainline"), um
anzugeben, von welchem Eltern-Commit du weiterarbeiten willst.

```bash
git revert -m 1 <merge-commit-id>
```

- `-m 1` → erster Eltern-Commit (`HEAD^1`) bleibt erhalten
- `-m 2` → zweiter Eltern-Commit (`HEAD^2`) bleibt erhalten

**Beispiel:**

```bash
git log --oneline
# e3a1b2c Merge branch 'feature'
# a7f8d3e Add new feature
# 9c5b7f1 Initial commit
```

- Erstellt einen neuen Commit, der den Merge rückgängig macht und vom ersten
  Eltern-Commit weiterarbeitet.
- Vorteil: Die Historie bleibt sauber, auch wenn der Merge schon gepusht wurde.

## 4. Merge erneut durchführen

Wenn du den Merge nach einem Reset oder Revert erneut durchführen möchtest:

```bash
git merge feature-branch
```

- Startet den Merge erneut.
- Bei Konflikten: Dateien bearbeiten, Konflikte lösen, dann:

```bash
git add <geänderte-Datei>
git commit
```

## 5. Umgang mit Merge-Konflikten

- Bei Konflikten markiert Git die betroffenen Stellen in den Dateien.
- Bearbeite die Dateien, löse die Konflikte und committe wie oben beschrieben.

## Wichtiger Hinweis

Merges können sicher rückgängig gemacht werden – lokal mit `reset`, remote mit
`revert -m`. Bei Konflikten: Ruhe bewahren, sorgfältig lösen und sauber
committen.
