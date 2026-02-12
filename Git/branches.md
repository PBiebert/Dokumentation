[Back to Table of Contents](../README.md)

# Git – Working with Branches

Branches are the central tool for parallel development and a clean commit
history in Git. They allow you to test new features or bugfixes without
affecting the main development line.

## Benefits and Use Cases

- Each branch is its own development environment
- Enables parallel teamwork
- Supports different release states (e.g. `main`, `dev`, `feature/*`,
  `bugfix/*`)
- Keeps history clear and understandable

## Typical Branch Names and Workflows

- **main**: Main development branch, contains stable code
- **dev**: Development branch for new features
- **feature/xyz**: For new features, e.g. `feature/login-system`
- **bugfix/xyz**: For bugfixes, e.g. `bugfix/login-error`

## Creating and Switching Branches

Create and switch to a new branch:

```bash
git checkout -b <branchname>
```

Or, more modern:

```bash
git switch -c <branchname>
```

Switch to an existing branch:

```bash
git checkout <branchname>
```

Or, more modern:

```bash
git switch <branchname>
```

List all branches:

```bash
git branch
```

Delete a branch:

```bash
git branch -d <branchname>
```

## Detached HEAD and Switching to Commits

Switch to a specific commit (detached HEAD):

```bash
git checkout <commit-id>
```

- HEAD now points directly to a commit, not a branch.
- Changes you make here are "dangling" and will be lost unless you create a new
  branch:

```bash
git checkout -b <new-branch>
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

## Key Points

- Use branches for every new task or bugfix.
- Switch between branches with `switch` or `checkout`.
- In detached HEAD state, always create a new branch if you want to keep your
  changes.

---

**See also:**

- [Undoing changes and commits](./aenderungen_und_commits_rueckgaengig.md)
- [Merge conflicts and undoing merges](./merge_commits.md)
