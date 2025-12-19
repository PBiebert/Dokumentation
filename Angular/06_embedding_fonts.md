[Zurück zur Übersicht](../README.md)

# Fonts in Angular einbinden

---

## 1. Font-Dateien ins Projekt kopieren

Lege die Schriftarten im Ordner `src/assets/fonts/` ab, z.B.:

```
src/
└── assets/
    └── fonts/
        └── Raleway/
            ├── Raleway-VariableFont_wght.ttf
            └── Raleway-Italic-VariableFont_wght.ttf
```

---

## 2. Fonts in styles.scss registrieren

Öffne `src/styles.scss` und füge die Fonts mit `@font-face` hinzu:

```scss
@font-face {
  font-family: "Bricolage_Grotesque";
  src: url(../public/assets/fonts/Bricolage_Grotesque/BricolageGrotesque-VariableFont.ttf);
  font-style: normal;
}

@font-face {
  font-family: "Kalam";
  src: url(../public/assets/fonts/Kalam/Kalam-Regular.ttf);
  font-style: normal;
}

.fontBricolageGrotesquey {
  font-family: "Bricolage_Grotesque";
}
```

---

## 3. Schriftart in Komponenten verwenden

Im Template:

```html
<h1 class=".fontBricolageGrotesquey">SAKURA RAMEN</h1>
```

Oder direkt im SCSS der Komponente:

```scss
h1 {
  font-family: "Bricolage_Grotesque";
}
```

---

## Hinweise

- Die Font-Dateien müssen im `assets`-Ordner liegen, damit sie von Angular
  gefunden werden.
- Die Einbindung erfolgt am besten in `styles.scss`, damit die Schriftart global
  verfügbar ist.
- Für komponentenspezifische Anpassungen kannst du die Font auch direkt im
  jeweiligen SCSS der Komponente verwenden.

---

## Zusammenfassung

- Font-Dateien in `src/assets/fonts/` ablegen.
- Fonts in `styles.scss` mit `@font-face` registrieren.
- Schriftart im Template oder SCSS der Komponente verwenden.
