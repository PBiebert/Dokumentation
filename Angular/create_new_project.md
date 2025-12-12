[Back to Table of Contents](../README.md)

# Neues Angular-Projekt erstellen

## 1. Projekt erstellen

Öffne das Terminal und führe folgenden Befehl aus:

```bash
ng new projektname
```

Dabei wirst du nach einigen Optionen gefragt (z.B. ob Routing benötigt wird,
welches Stylesheet-Format verwendet werden soll). Wähle die gewünschten
Einstellungen aus.

## 2. Entwicklungsserver starten

Um das Projekt lokal zu starten, verwende:

```bash
ng serve
```

Optional kannst du den Server direkt im Browser öffnen lassen:

```bash
ng serve --open
```

Dadurch wird das Projekt kompiliert und unter `http://localhost:4200` angezeigt.

---

# Ordnerstruktur und wichtige Dateien

Nach dem Erstellen sieht die Struktur etwa so aus (ab Angular v20):

```
projektname/
│
├── .angular/           # Angular-spezifische Daten (nicht bearbeiten)
├── .vscode/            # VSCode-spezifische Einstellungen (nicht bearbeiten)
├── node_modules/       # Installierte Pakete (automatisch verwaltet)
├── public/             # Öffentliche Ressourcen (z.B. statische Dateien)
│   ├── assets/         # Statische Ressourcen wie Bilder, Fonts etc.
│   │   ├── img/
│   │   └── fonts/
│   └── favicon.ico     # Favicon liegt direkt in public
├── src/                # Quellcode des Projekts
│   ├── app/            # Hauptanwendungsordner
│   │   ├── app.config.ts     # Konfigurationen für die App
│   │   ├── app.html         # Haupt-Template der App
│   │   ├── app.routes.ts    # Routing-Konfiguration
│   │   ├── app.scss         # Styles der App
│   │   ├── app.spec.ts      # Tests für die App
│   │   └── app.ts           # Hauptlogik/Modul der App
│   ├── index.html      # Einstiegspunkt der Anwendung (nur <head> anpassen)
│   ├── main.ts         # Einstiegspunkt für TypeScript
│   └── styles.scss     # Globale Styles (nur allgemeine Anpassungen)
├── .editorconfig       # Editor-Einstellungen
├── .gitignore          # Git-Konfiguration für auszuschließende Dateien
├── angular.json        # Zentrale Angular-Konfiguration
├── package-lock.json   # Sperrdatei für Paketversionen
├── package.json        # Projektabhängigkeiten und Skripte
├── README.md           # Projektbeschreibung
├── tsconfig.app.json   # TypeScript-Konfiguration für die App
├── tsconfig.json       # Allgemeine TypeScript-Konfiguration
└── tsconfig.spec.json  # TypeScript-Konfiguration für Tests

```

---

## Erklärung der wichtigsten Ordner und Dateien

### Projektordner (Root)

- **.angular/**: Interne Angular-Daten, nicht bearbeiten.
- **.vscode/**: VSCode-spezifische Einstellungen, nicht bearbeiten.
- **node_modules/**: Installierte Pakete, wird automatisch verwaltet.
- **public/**: Öffentliche Ressourcen, z.B. für statische Dateien. Enthält jetzt
  auch den `assets`-Ordner für Bilder, Fonts usw.
  - **assets/**: Statische Ressourcen wie Bilder, Fonts etc.
  - **favicon.ico**: Das Favicon liegt direkt im `public`-Ordner.
- **.editorconfig**: Einstellungen für den Editor.
- **.gitignore**: Legt fest, welche Dateien nicht zu Git hinzugefügt werden.
- **angular.json**: Zentrale Konfigurationsdatei für Angular.
- **package.json / package-lock.json**: Enthält Abhängigkeiten und Skripte.
- **README.md**: Projektbeschreibung.
- **tsconfig\*.json**: TypeScript-Konfigurationen für verschiedene Bereiche
  (App, Tests, allgemein).

### `src/`

- **app/**: Enthält alle zentralen Dateien deiner Anwendung:
  - **app.config.ts**: Konfigurationen für die App.
  - **app.html**: Haupt-Template der App.
  - **app.routes.ts**: Routing-Konfiguration.
  - **app.scss**: Styles der App.
  - **app.spec.ts**: Tests für die App.
  - **app.ts**: Hauptlogik oder Hauptmodul der App.
- **index.html**: Einstiegspunkt der Anwendung. In der Regel wird nur der
  `<head>` angepasst (z.B. Titel, Meta-Tags). Der `<body>` wird von Angular
  automatisch generiert.
- **main.ts**: Einstiegspunkt für die Anwendung, hier wird das Hauptmodul
  geladen.
- **styles.scss**: Globale Styles für die gesamte Anwendung. Hier sollten nur
  allgemeine, globale Anpassungen vorgenommen werden. Komponenten-spezifische
  Styles werden in den jeweiligen `.scss` Dateien gepflegt.

---

## Hinweise

- **Globale Styles**: Nur allgemeine Anpassungen in `styles.scss` vornehmen.
  Komponenten-spezifische Styles gehören in die jeweiligen Komponenten-Dateien.
- **index.html**: Nur den `<head>` bearbeiten, z.B. Titel oder Meta-Tags. Der
  `<body>` wird von Angular verwaltet.
- **Konfiguration**: Die wichtigste Datei für Einstellungen ist `angular.json`.
