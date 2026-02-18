[Zurück zum Inhaltsverzeichnis](../README.md)

# TypeScript Installation und Kompilierung

## Inhaltsverzeichnis

- [package.json und package-lock.json](#packagejson-und-package-lockjson)
- [TypeScript Installation](#typescript-installation)
  - [Lokal installieren](#lokal-installieren)
  - [Global installieren](#global-installieren)
- [TypeScript Version prüfen und steuern](#typescript-version-prüfen-und-steuern)
  - [Version anzeigen](#version-anzeigen)
  - [Version ändern](#version-ändern)
- [TypeScript kompilieren](#typescript-kompilieren)
  - [Kompilieren eines TypeScript-Files zu JavaScript](#kompilieren-eines-typescript-files-zu-javascript)
  - [Kompilieren aller Dateien im Projekt](#kompilieren-aller-dateien-im-projekt)
  - [Kompilieren mit Ziel-JavaScript-Version](#kompilieren-mit-ziel-javascript-version)
- [Browser-Kompatibilität und Versionsempfehlung](#browser-kompatibilität-und-versionsempfehlung)
- [Zusammenfassung](#zusammenfassung)

---

## package.json und package-lock.json

**package.json**

- Enthält Metadaten zum Projekt (Name, Version, Beschreibung).
- Listet alle Abhängigkeiten und Skripte auf.
- Ermöglicht das einfache Installieren und Verwalten von Paketen.

**package-lock.json**

- Wird automatisch beim Installieren von Paketen erstellt.
- Sichert die exakten Versionen aller installierten Pakete.
- Garantiert, dass alle Entwickler dieselben Paketversionen nutzen.

---

## TypeScript Installation

### Lokal installieren

TypeScript wird nur im aktuellen Projekt installiert.

```bash
npm install typescript --save-dev
```

- Das `--save-dev` ist **nicht zwingend erforderlich**, aber empfohlen.  
  Ohne `--save-dev` wird TypeScript unter `dependencies` gespeichert, mit
  `--save-dev` unter `devDependencies`.
- **devDependencies** sind Pakete, die nur für die Entwicklung benötigt werden
  (z.B. Compiler, Test-Tools).
- **dependencies** sind Pakete, die auch im Produktivbetrieb gebraucht werden.

> **Hinweis:**  
> Die Datei `package.json` wird im lokalen Projektordner erstellt, wenn du z.B.
>
> ```bash
> npm init
> ```
>
> ausführst oder das erste npm-Paket installierst.  
> Ohne `package.json` werden keine Abhängigkeiten dokumentiert.

### Global installieren

TypeScript steht systemweit zur Verfügung.

```bash
npm install -g typescript
```

- Vorteil: `tsc` ist überall im Terminal verfügbar.
- Nachteil: Alle Projekte nutzen die gleiche globale Version.

---

## TypeScript Version prüfen und steuern

### Version anzeigen

```bash
tsc --version
```

### Version ändern

- Lokal:  
  Ändere die Version in der `package.json` unter `devDependencies` (diese Datei
  liegt im Projektordner) und führe dann

  ```bash
  npm install
  ```

  aus.

- Global:  
  Installiere eine bestimmte Version mit
  ```bash
  npm install -g typescript@<version>
  ```

---

## TypeScript kompilieren

### Kompilieren einer TypeScript-Datei zu JavaScript

```bash
tsc datei.ts
```

- Erstellt eine `datei.js` im gleichen Verzeichnis.

### Kompilieren aller Dateien im Projekt

Erstelle zuerst eine `tsconfig.json` mit

```bash
npx tsc --init
```

Dann einfach

```bash
tsc
```

ausführen.

### Kompilieren mit Ziel-JavaScript-Version

Du kannst beim Kompilieren direkt die gewünschte JavaScript-Version angeben:

```bash
tsc *.ts --target ES5
```

- Das kompiliert alle `.ts`-Dateien im Verzeichnis und erzeugt JavaScript im
  angegebenen Standard (z.B. ES5, ES6, ES2017, etc.).
- Die Option `--target <version>` überschreibt die Einstellung aus der
  `tsconfig.json` für diesen Kompiliervorgang.

---

## Browser-Kompatibilität und Versionsempfehlung

- Moderne Browser unterstützen aktuelle JavaScript-Versionen.
- Für maximale Kompatibilität empfiehlt es sich, beim Kompilieren auf eine
  ältere JS-Version wie ES5 oder ES6 zurückzugehen.
- In der `tsconfig.json` kann das Ziel eingestellt werden:
  ```json
  {
    "compilerOptions": {
      "target": "ES5"
    }
  }
  ```
- **Empfehlung:** Gehe 2–3 Versionen zurück (z.B. ES5 oder ES6), um eine sehr
  gute Browserabdeckung zu gewährleisten.

---

## Zusammenfassung

- Mit npm installierst du TypeScript lokal oder global.
- Die Version steuerst du über npm und die package.json.
- Kompiliere mit `tsc` deine TypeScript-Dateien zu JavaScript.
- Für beste Browser-Kompatibilität wähle als Ziel eine ältere JS-Version
  (ES5/ES6).
- Die package.json und package-lock.json helfen dir beim Verwalten und
  Nachvollziehen deiner Abhängigkeiten.
