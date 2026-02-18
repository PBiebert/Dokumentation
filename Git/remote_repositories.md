[Zurück zum Inhaltsverzeichnis](../README.md)

# Git – Verbinden und Synchronisieren von Remote-Repositories

Mit `git remote add` verknüpfst du ein Remote-Repository (z.B. auf GitHub) mit
deinem lokalen Repository. Du kannst dann deine Änderungen pushen oder Updates
vom Remote ziehen.

## 1. Remote hinzufügen

```bash
git remote add origin <URL>
```

Beispiel:

```bash
git remote add origin https://github.com/PBiebert/Github-remote-repo-test.git
```

- `origin` ist der Name des Remote-Repositories (Standard, kann aber beliebig
  gewählt werden).

## 2. Branch umbenennen (optional)

```bash
git branch -M main
```

- Macht den aktuellen Branch zum `main`.
- Hinweis: In manchen Projekten ist der Standard-Branch noch `master`. Passe
  ggf. an:

```bash
git branch -M master
```

## 3. Änderungen zum Remote pushen (Ersteinrichtung)

```bash
git push -u origin main
```

- Mit `-u` wird das Tracking eingerichtet: Dein lokaler Branch wird mit dem
  Remote-Branch verknüpft.
- Danach reichen `git push` und `git pull`, du musst Remote und Branch nicht
  jedes Mal angeben.

**Wichtig:**

- Prüfe den Namen deines Standard-Branches (`main` oder `master`).
- `git remote add` ist nur einmal pro Remote nötig.

## 4. Änderungen zum Remote pushen (nach Ersteinrichtung)

Wenn du bereits `git push -u origin main` ausgeführt hast, genügt für weitere
Uploads:

```bash
git push
```

- Pusht alle neuen Commits von deinem lokalen Branch zum Remote.
- Praktisch für laufende Arbeit, da Remote und lokaler Branch bereits verknüpft
  sind.

## 5. Änderungen vom Remote ziehen

Wenn es neue Commits im Remote-Repository gibt (z.B. von Teammitgliedern oder
direkten Änderungen auf GitHub), kannst du sie lokal holen:

```bash
git pull
```

- Holt neue Commits vom verknüpften Remote-Branch und führt sie automatisch mit
  deinem lokalen Stand zusammen.
