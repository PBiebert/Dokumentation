[Back to Table of Contents](../README.md)

# Das npm-Modul `serve` für Single Page Applications (SPA)

`serve` ist ein einfaches, plattformübergreifendes npm-Modul, das dazu dient,
statische Dateien und Webseiten schnell bereitzustellen. Es eignet sich
besonders für das lokale Testen und Ausliefern von gebauten Webanwendungen, wie
z.B. Single Page Applications (SPA), kann aber auch für jede andere statische
Website verwendet werden.

Mit dem Paket [`serve`](https://www.npmjs.com/package/serve) kannst du statische
Dateien und insbesondere Single Page Applications (SPA) lokal oder im Netzwerk
bereitstellen.

## Installation

Installiere `serve` global:

```bash
npm install -g serve
```

## Nutzung

Wechsle in das Verzeichnis deiner gebauten SPA (z.B. `dist/` bei Angular) und
starte den Server:

```bash
serve -s .
```

- Das `-s` (oder `--single`) Flag sorgt dafür, dass alle Routen auf die
  `index.html` umgeleitet werden – wichtig für SPAs mit clientseitigem Routing.

## Beispiele

**Angular-Projekt bereitstellen:**

```bash
cd dist/mein-angular-projekt
serve -s .
```

**Port anpassen (z.B. Port 5000):**

```bash
serve -s . -l 5000
```

## Nützliche Optionen

- `-s, --single` – Single Page Application Modus (empfohlen für SPAs)
- `-l, --listen` – Port oder Adresse angeben (z.B. `-l 8080`)

## Wichtige CMD-Befehle für `serve`

| Befehl                      | Beschreibung                                             |
| --------------------------- | -------------------------------------------------------- |
| `serve .`                   | Startet einen statischen Server im aktuellen Verzeichnis |
| `serve -s .`                | Startet im SPA-Modus (alle Routen auf `index.html`)      |
| `serve -l 4000`             | Startet den Server auf Port 4000                         |
| `serve -s build -l 8080`    | Startet SPA aus dem `build`-Verzeichnis auf Port 8080    |
| `serve -s . --no-clipboard` | Startet ohne Kopieren der URL in die Zwischenablage      |
| `serve -s . -c 60`          | Setzt Cache-Control Header auf 60 Sekunden               |

## Zusammenfassung

- `serve` ist ideal, um gebaute SPAs lokal zu testen oder im Netzwerk
  bereitzustellen.
- Mit `serve -s .` wird das aktuelle Verzeichnis als SPA-Server gestartet.
