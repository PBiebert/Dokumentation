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

**Tipp:**  
Möchtest du den Entwicklungsserver für andere Geräte im selben Netzwerk (z.B.
Smartphone, Tablet) zugänglich machen, starte ihn mit:

```bash
ng serve --host 0.0.0.0
```

Anschließend kannst du das Projekt über die IP-Adresse deines Rechners im
Netzwerk aufrufen, z.B. `http://192.168.1.100:4200`.  
Die genaue IP-Adresse findest du mit `ipconfig` (Windows) oder `ifconfig`
(Mac/Linux) im Terminal.

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

---

# Sinnvolle Struktur im `app`-Ordner (nach Angular Styleguide)

Eine durchdachte Struktur im `src/app`-Ordner ist besonders bei größeren
Projekten mit mehreren Seiten und Modulen wichtig.  
Hier ein Vorschlag, wie du Services, Feature-Module, Shared-Elemente und
Seitenspezifisches sinnvoll organisieren kannst:

```
src/
└── app/
    ├── core/     # Zentrale, einmalige Services, globale Guards, Interceptors, z.B. Auth, ErrorHandler
    │   ├── services/
    │   │   ├── auth.service.ts
    │   │   └── ...
    │   ├── guards/
    │   ├── interceptors/
    │   └── core.module.ts
    ├── shared/     # Wiederverwendbare Komponenten, Pipes, Direktiven
    │   ├── components/
    │   │   ├── footer/
    │   │   │   ├── footer.component.ts
    │   │   │   ├── footer.component.html
    │   │   │   └── footer.component.scss
    │   │   ├── header/
    │   │   │   ├── header.component.ts
    │   │   │   ├── header.component.html
    │   │   │   └── header.component.scss
    │   │   └── ...
    │   ├── pipes/
    │   ├── directives/
    │   └── shared.module.ts
    ├── features/     # Feature-Module für größere Bereiche/Funktionen
    │   ├── fruitlist/
    │   │   ├── fruitlist.component.ts
    │   │   ├── fruitlist.component.html
    │   │   ├── fruitlist.module.ts
    │   │   └── ...
    │   ├── user/
    │   │   ├── user.component.ts
    │   │   ├── user.module.ts
    │   │   └── ...
    │   └── ...
    ├── pages/     # Seiten-Module, jeweils für eine Route/Seite (z.B. Home, About, Dashboard)
    │   ├── home/
    │   │   ├── home.component.ts
    │   │   ├── home.module.ts
    │   │   └── ...
    │   ├── about/
    │   │   ├── about.component.ts
    │   │   ├── about.module.ts
    │   │   └── ...
    │   └── ...
    ├── services/     # Projektweite Services, die nicht nur für core relevant sind
    │   ├── fruitlistdata.service.ts
    │   └── ...
    ├── app.component.ts     # Root-Komponente
    ├── app.component.html
    ├── app.component.scss
    ├── app.module.ts        # Root-Modul, importiert alle anderen Module
    └── app.routes.ts        # Routing-Konfiguration
```

**Erklärung und Tipps:**

- **core/**: Alles, was global und nur einmal im Projekt gebraucht wird
  (Singletons, zentrale Services, Guards, Interceptors).
- **shared/**: Wiederverwendbare Komponenten (z.B. Footer, Header), Pipes und
  Direktiven, die in mehreren Features/Seiten genutzt werden.
- **features/**: Funktionale Bereiche, die auch als eigenständige Module geladen
  werden können (z.B. Lazy Loading für große Features).
- **pages/**: Jede Seite (Route) als eigenes Modul, das wiederum Feature-Module
  oder Shared-Komponenten nutzt. Ideal für Projekte mit mehreren Seiten.
- **services/**: Projektweite Services, die nicht nur für core relevant sind
  (z.B. Datenservices, API-Services).
- **app.component.\*** und **app.module.ts**: Einstiegspunkt und zentrales Modul
  der Anwendung.

**Weitere Hinweise:**

- **Lazy Loading:** Feature- und Page-Module können bei Bedarf per Lazy Loading
  geladen werden, um die Startzeit der App zu optimieren.
- **Trennung von Features und Seiten:** Features sind wiederverwendbare
  Funktionsbereiche, Pages sind konkrete Seiten/Routen.
- **Services:** Services, die nur für ein Feature gebraucht werden, kommen ins
  jeweilige Feature-Modul. Services, die global gebraucht werden, nach
  `core/services` oder `app/services`.

**Vorteile dieser Struktur:**

- Klare Trennung zwischen globalen, wiederverwendbaren, feature-spezifischen und
  seitenbezogenen Elementen.
- Sehr gute Skalierbarkeit und Übersichtlichkeit, auch bei großen Projekten.
- Einhaltung der Angular Styleguides und Best Practices.

**Tipp:**  
Weitere Infos und Beispiele findest du im offiziellen
[Angular Styleguide](https://angular.io/guide/styleguide).
