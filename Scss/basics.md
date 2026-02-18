[Zurück zum Inhaltsverzeichnis](../README.md)

# SCSS Installation und Kompilierung

## Inhaltsverzeichnis

- [SCSS installieren](#scss-installieren)
- [SCSS kompilieren](#scss-kompilieren)
- [Zusammenfassung](#zusammenfassung)

---

## SCSS installieren

Installiere Sass (SCSS-Compiler) global mit npm:

```bash
npm install -g sass
```

Oder lokal im Projekt:

```bash
npm install sass --save-dev
```

---

## SCSS kompilieren

Kompiliere eine SCSS-Datei zu CSS:

```bash
sass style.scss style.css
```

Automatisches Kompilieren bei Änderungen:

```bash
sass --watch style.scss:style.css
```

Du kannst auch ganze Ordner beobachten:

```bash
sass --watch scss/:css/
```

---

## Zusammenfassung

- Installiere Sass global oder lokal mit npm.
- Kompiliere SCSS-Dateien mit dem Befehl `sass`.
- Nutze `--watch`, um Änderungen automatisch zu kompilieren.
