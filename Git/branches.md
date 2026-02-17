[Zurück zum Inhaltsverzeichnis](../README.md)

# Git – Arbeiten mit Branches

Branches sind das zentrale Werkzeug für parallele Entwicklung und eine saubere
Commit-Historie in Git. Sie ermöglichen es dir, neue Features oder Bugfixes zu
testen, ohne die Hauptentwicklungslinie zu beeinflussen.

<!-- Inhaltsverzeichnis -->

## Inhaltsverzeichnis

- [Vorteile und Anwendungsfälle](#vorteile-und-anwendungsfälle)
- [Typische Branchnamen und Workflows](#typische-branchnamen-und-workflows)
- [Branches erstellen und wechseln](#branches-erstellen-und-wechseln)
- [Branch veröffentlichen](#branch-veröffentlichen)
- [Änderungen aus main auf den eigenen Branch holen](#änderungen-aus-main-auf-den-eigenen-branch-holen)
- [Zwischenzeitlich zu einem anderen Branch wechseln](#zwischenzeitlich-zu-einem-anderen-branch-wechseln)
- [Branches zusammenführen (Mergen)](#branches-zusammenführen-mergen)
- [Abgeschlossene Branches löschen](#abgeschlossene-branches-löschen)
- [Wichtige Punkte](#wichtige-punkte)
- [Siehe auch](#siehe-auch)

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

**Unterschied zwischen Merge und Rebase**

- **Merge (`git merge origin/main`):**
  - Erstellt einen neuen "Merge-Commit", der die Änderungen aus `origin/main` und deinem aktuellen Branch zusammenführt.
  - Die Historie bleibt verzweigt, d.h. man sieht, dass zwei Entwicklungsstränge zusammengeführt wurden.
  - Vorteil: Die Commit-Historie bleibt vollständig und zeigt, wann Branches zusammengeführt wurden.
  - Nachteil: Die Historie kann bei vielen Merges unübersichtlich werden.

- **Rebase (`git rebase origin/main`):**
  - "Spult" deine eigenen Commits so um, als ob sie direkt auf den neuesten Stand von `origin/main` aufgesetzt wurden.
  - Die Historie wirkt dadurch linearer und aufgeräumter.
  - Vorteil: Saubere, lineare Commit-Historie, als ob alles nacheinander entwickelt wurde.
  - Nachteil: Bei öffentlichen Branches kann das Umschreiben der Historie zu Problemen führen, wenn andere darauf arbeiten.

**Wann sollte man was verwenden?**

- **Merge** eignet sich, wenn du die Historie vollständig und nachvollziehbar halten möchtest, z.B. in Teamprojekten oder bei öffentlichen Branches.
- **Rebase** ist sinnvoll, wenn du eine saubere, lineare Historie bevorzugst und dein Branch nur von dir genutzt wird (z.B. vor dem Erstellen eines Pull Requests).
- **Wichtig:** Bei gemeinsam genutzten Branches solltest du Rebase nur anwenden, wenn alle Teammitglieder damit einverstanden sind und wissen, wie man mit eventuellen Konflikten umgeht.

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

## Branches zusammenführen (Mergen)

Wenn du mit der Arbeit an einem Branch (z.B. `feature/xyz`) fertig bist und die
Änderungen in einen anderen Branch (z.B. `main` oder `dev`) übernehmen möchtest,
kannst du die Branches zusammenführen (mergen).

**So funktioniert das Mergen:**

1. **Wechsle in den Ziel-Branch (z.B. `main`):**

   ```bash
   git switch main
   ```

   oder

   ```bash
   git checkout main
   ```

2. **Führe den Merge aus:**

   ```bash
   git merge feature/xyz
   ```

   Dadurch werden die Änderungen aus `feature/xyz` in den aktuellen Branch
   (`main`) übernommen.

3. **(Optional) Push auf das Remote-Repository:**
   ```bash
   git push origin main
   ```

**Hinweis:**  
Falls es Konflikte gibt, zeigt Git dir an, welche Dateien angepasst werden
müssen. Löse die Konflikte, committe die Änderungen und führe ggf. den Merge
fort.

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

## Wichtige Punkte

- Verwende für jede neue Aufgabe oder Bugfix einen eigenen Branch.
- Wechsle zwischen Branches mit `switch` oder `checkout`.
- Im Detached-HEAD-Zustand immer einen neuen Branch erstellen, wenn du deine
  Änderungen behalten möchtest.

---

**Siehe auch:**

- [Änderungen und Commits rückgängig machen](./aenderungen_und_commits_rueckgaengig.md)
- [Merge-Konflikte und Merges rückgängig machen](./merge_commits.md)
