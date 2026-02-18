[Zurück zum Inhaltsverzeichnis](../README.md)

# SCSS @mixin

## Inhaltsverzeichnis

- [Was ist ein @mixin?](#was-ist-ein-mixin)
- [Syntax](#syntax)
- [Beispiel: Flexbox-Mixin](#beispiel-flexbox-mixin)
- [Vorteile von Mixins](#vorteile-von-mixins)
- [Zusammenfassung](#zusammenfassung)

---

## Was ist ein @mixin?

Mit `@mixin` kannst du wiederverwendbare Codeblöcke in SCSS definieren. So
kannst du häufig genutzte CSS-Regeln zentral speichern und überall einbinden.

---

## Syntax

```scss
@mixin mixin-name($param1, $param2: default) {
  // CSS-Regeln
}
```

Einbinden mit `@include`:

```scss
@include mixin-name(value1, value2);
```

---

## Beispiel: Flexbox-Mixin

Ein häufig genutztes Beispiel ist ein Flexbox-Mixin, das die wichtigsten
Eigenschaften flexibel setzt:

```scss
@mixin flexbox(
  $direction: row,
  $justify: flex-start,
  $align: stretch,
  $gap: 0
) {
  display: flex;
  flex-direction: $direction;
  justify-content: $justify;
  align-items: $align;
  gap: $gap;
}
```

> In diesem Beispiel haben alle Parameter einen Standardwert. Du kannst
> Standardwerte angeben, musst es aber nicht – Parameter können auch ohne
> Vorgabewert definiert werden.

Verwendung:

```scss
.container {
  @include flexbox(row, center, center, 16px);
}

// Beispiel mit benanntem Parameter:
.container {
  @include flexbox($justify: center);
}
```

Das erzeugt im CSS:

```css
.container {
  display: flex;
  flex-direction: row;
  justify-content: center;
  align-items: center;
  gap: 16px;
}

/* und */

.container {
  display: flex;
  flex-direction: row;
  justify-content: center;
  align-items: stretch;
  gap: 0;
}
```

---

## Vorteile von Mixins

- Vermeidung von doppeltem Code
- Zentrale Wartung von wiederkehrenden Mustern
- Mixins können Parameter mit Standardwerten haben

---

## Zusammenfassung

- Mit `@mixin` definierst du wiederverwendbare Codeblöcke.
- Mit `@include` bindest du sie ein.
- Mixins machen deinen SCSS-Code modular und übersichtlich.
