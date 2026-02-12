[Zurück zum Inhaltsverzeichnis](../README.md)

# Git – Arbeiten mit Branches

Branches sind das zentrale Werkzeug für parallele Entwicklung und eine saubere
Commit-Historie in Git. Sie ermöglichen es dir, neue Features oder Bugfixes zu
testen, ohne die Hauptentwicklungslinie zu beeinflussen.

## Vorteile und Anwendungsfälle

- Jeder Branch ist eine eigene Entwicklungsumgebung
- Ermöglicht parallele Teamarbeit
- Unterstützt verschiedene Release-Stände (z.B. `main`, `dev`, `feature/*`,
  `bugfix/*`)
- Hält die Historie klar und verständlich

## Typische Branchnamen und Workflows

- **main**: Hauptentwicklungs-Branch, enthält stabilen Code
- **dev**: Entwicklungs-Branch für neue Features
- **feature/xyz**: Für neue Features, z.B. `feature/login-system`
- **bugfix/xyz**: Für Bugfixes, z.B. `bugfix/login-error`

## Branches erstellen und wechseln

Einen neuen Branch erstellen und direkt darauf wechseln:

```bash
git checkout -b <branchname>
```

Oder moderner:

```bash
git switch -c <branchname>
```

Zu einem bestehenden Branch wechseln:

```bash
git checkout <branchname>
```

Oder moderner:

```bash
git switch <branchname>
```

Alle Branches auflisten:

```bash
git branch
```

Einen Branch löschen:

```bash
git branch -d <branchname>
```

## Änderungen aus `main` auf den eigenen Branch holen

Mit `git pull` holst du standardmäßig nur die Änderungen des Remote-Branches,
auf dem du dich gerade befindest (z.B. `origin/feature/xyz` auf
`feature/xyz`).  
**Um explizit die neuesten Änderungen aus `main` auf deinen aktuellen Branch zu
holen, verwende:**

**Möglichkeit 1: Mergen**

```bash
git fetch origin
git merge origin/main
```

**Möglichkeit 2: Rebase**

```bash
git fetch origin
git rebase origin/main
```

> **Hinweis:**  
> In vielen Beispielen steht zuerst `git checkout <dein-branch>`.  
> Das ist nur nötig, wenn du dich noch nicht auf deinem Ziel-Branch befindest.  
> Bist du bereits auf dem Branch, kannst du direkt mit `git fetch` und
> `git merge` oder `git pull origin main` weitermachen.

> `git pull origin main` würde die Änderungen aus `main` direkt in deinen
> aktuellen Branch mergen (entspricht `fetch` + `merge`).  
> Das ist möglich, aber explizit:
>
> ```bash
> git pull origin main
> ```
>
> **Achtung:** Das macht ein Merge von `origin/main` in deinen aktuellen Branch.

**Zusammengefasst:**

- `git pull` allein aktualisiert nur deinen aktuellen Branch.
- Mit `git pull origin main` oder `git merge origin/main` holst du gezielt die
  Änderungen aus `main`.

## Abgeschlossene Branches löschen

Wenn du mit einem Branch fertig bist (z.B. nach dem Merge in `main`), kannst du
ihn löschen:

**Lokal löschen:**

```bash
git branch -d <branchname>
```

- Nutze `-d` (delete), wenn der Branch bereits gemerged wurde.
- Nutze `-D` (force delete), um den Branch auch ohne Merge zu löschen.

**Remote löschen:**

```bash
git push origin --delete <branchname>
```

- Damit entfernst du den Branch auch vom Remote-Repository (z.B. auf GitHub).

> **Hinweis:**  
> Das Löschen von Branches ist **nicht zwingend erforderlich**.  
> Du kannst Branches auch behalten.  
> Allerdings empfiehlt es sich, nicht mehr benötigte Branches zu löschen, um das
> Repository übersichtlich zu halten und Verwirrung zu vermeiden.

> **Tipp:**  
> Lösche Branches erst, wenn sie wirklich nicht mehr benötigt werden und alle
> Änderungen übernommen wurden.

## Branch veröffentlichen

Wenn du einen neuen Branch erstellt hast, ist dieser zunächst nur lokal
vorhanden.  
Um ihn für andere sichtbar zu machen und auf das Remote-Repository (z.B. GitHub)
hochzuladen, verwende:

```bash
git push origin <branchname>
```

- Dadurch wird der Branch auf das Remote-Repository übertragen.
- Nach dem Push können andere Teammitglieder darauf zugreifen.

> **Tipp:**  
> Nach dem ersten Push kannst du mit `git push` und `git pull` wie gewohnt
> arbeiten, da der Branch nun mit dem Remote-Branch verknüpft ist.

## Zwischenzeitlich zu einem anderen Branch wechseln

Manchmal möchtest du während deiner Arbeit an einem Branch (z.B. `feature/xyz`)
kurz zu einem anderen Branch (z.B. `main`) wechseln, um dort Änderungen
vorzunehmen. Danach kannst du wieder zurück in deinem ursprünglichen Branch
wechseln und dort weiterarbeiten.

**So gehst du vor:**

1. **Stelle sicher, dass deine aktuellen Änderungen gesichert sind:**  
   Wenn du noch nicht committet hast, kannst du deine Änderungen mit `git stash`
   zwischenspeichern:

   ```bash
   git stash
   ```

2. **Wechsle zum gewünschten Branch (z.B. `main`):**

   ```bash
   git switch main
   ```

   oder

   ```bash
   git checkout main
   ```

3. **Nimm deine Änderungen im anderen Branch vor**  
   Führe die gewünschten Änderungen durch und committe sie wie gewohnt.

4. **Wechsle zurück zu deinem ursprünglichen Branch:**

   ```bash
   git switch feature/xyz
   ```

   oder

   ```bash
   git checkout feature/xyz
   ```

5. **(Optional) Hole deine zwischengespeicherten Änderungen zurück:**  
   Falls du `git stash` verwendet hast, hole die Änderungen mit:
   ```bash
   git stash pop
   ```

Du kannst diesen Ablauf beliebig oft wiederholen, um flexibel zwischen Branches
zu wechseln und überall Änderungen vorzunehmen.

## Wichtige Punkte

- Verwende für jede neue Aufgabe oder Bugfix einen eigenen Branch.
- Wechsle zwischen Branches mit `switch` oder `checkout`.
- Im Detached-HEAD-Zustand immer einen neuen Branch erstellen, wenn du deine
  Änderungen behalten möchtest.

---

**Siehe auch:**

- [Änderungen und Commits rückgängig machen](./aenderungen_und_commits_rueckgaengig.md)
- [Merge-Konflikte und Merges rückgängig machen](./merge_commits.md)
