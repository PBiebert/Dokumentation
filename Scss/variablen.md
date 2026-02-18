[Zurück zum Inhaltsverzeichnis](../README.md)

# SCSS Variablen

## Inhaltsverzeichnis

- [Was sind SCSS-Variablen?](#was-sind-scss-variablen)
- [Globale Variablen definieren](#globale-variablen-definieren)
- [Variablen in andere SCSS-Dateien importieren](#variablen-in-andere-scss-dateien-importieren)
- [Beispiel: Verwendung globaler Variablen](#beispiel-verwendung-globaler-variablen)
- [Hinweis zu @use (neuere Syntax)](#hinweis-zu-use-neuere-syntax)
- [Zusammenfassung](#zusammenfassung)

---

## Was sind SCSS-Variablen?

Mit Variablen kannst du Werte wie Farben, Abstände oder Schriftgrößen zentral
speichern und im gesamten Stylesheet wiederverwenden.

```scss
$primary-color: #3498db;
$padding-base: 16px;

.button {
  background: $primary-color;
  padding: $padding-base;
}
```

---

## Globale Variablen definieren

Um Variablen global zu nutzen, legst du sie in einer eigenen Datei ab, z.B.
`_variables.scss`.

```scss
// _variables.scss
$primary-color: #3498db;
$secondary-color: #e67e22;
$font-size-base: 16px;
```

> Konvention: Dateinamen mit Unterstrich (`_`) beginnen, damit sie nicht direkt
> zu CSS kompiliert werden.

---

## Variablen in andere SCSS-Dateien importieren

Um die globalen Variablen in anderen SCSS-Dateien zu verwenden, importiere die
Datei am Anfang deiner SCSS-Datei:

```scss
@import "variables";
// oder mit Pfad, z.B.:
@import "../styles/variables";
```

> Der Unterstrich und die Dateiendung `.scss` werden beim Import weggelassen.

---

## Beispiel: Verwendung globaler Variablen

```scss
// _variables.scss
$primary-color: #3498db;

// styles.scss
@import "variables";

.header {
  background: $primary-color;
}
```

---

## Hinweis zu @use (neuere Syntax)

Statt `@import` empfiehlt SCSS ab Version 4 die Verwendung von `@use`:

```scss
// _variables.scss
$primary-color: #3498db;

// styles.scss
@use "variables";

.button {
  color: variables.$primary-color;
}
```

> Mit `@use` sind Variablen standardmäßig nur über den Namensraum
> (`variables.$primary-color`) verfügbar.

---

## Zusammenfassung

- Variablen speichern zentrale Werte für dein SCSS.
- Lege globale Variablen in einer eigenen Datei ab.
- Importiere die Datei mit `@import` oder `@use` in andere SCSS-Dateien.
- Mit Variablen wird dein CSS wartbarer und konsistenter.
