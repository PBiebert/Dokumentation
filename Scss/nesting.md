[Zurück zum Inhaltsverzeichnis](../README.md)

# SCSS Verschachtelung (Nesting)

## Inhaltsverzeichnis

- [Grundlagen](#grundlagen)
- [Verschachtelungstiefe](#verschachtelungstiefe)
- [Das &-Zeichen](#das-zeichen)
  - [Modifier](#modifier)
  - [Pseudoklassen und Pseudoelemente](#pseudoklassen-und-pseudoelemente)
- [Verschachtelung mit @-Rules](#verschachtelung-mit--rules)
- [Beispiel: Komplexe Verschachtelung](#beispiel-komplexe-verschachtelung)
- [Best Practices](#best-practices)
- [Zusammenfassung](#zusammenfassung)

---

## Grundlagen

Mit SCSS kannst du CSS-Selektoren verschachteln, um die Struktur deines HTMLs
besser abzubilden und deinen Code übersichtlicher zu gestalten.

```scss
.parent {
  color: blue;

  .child {
    color: red;
  }
}
```

Das erzeugt im CSS:

```css
.parent {
  color: blue;
}
.parent .child {
  color: red;
}
```

---

## Verschachtelungstiefe

Du kannst beliebig tief verschachteln, aber zu viele Ebenen machen den Code
schwer lesbar.

```scss
.navbar {
  ul {
    li {
      a {
        color: green;
      }
    }
  }
}
```

---

## Das &-Zeichen

Das `&` steht für den Eltern-Selektor und ist besonders nützlich für Modifier,
Pseudoklassen und Pseudoelemente.

### Modifier

```scss
.button {
  &--primary {
    background: blue;
  }
}
```

Erzeugt:

```css
.button--primary {
  background: blue;
}
```

### Pseudoklassen und Pseudoelemente

```scss
.link {
  &:hover {
    text-decoration: underline;
  }
  &::after {
    content: ">";
  }
}
```

---

## Verschachtelung mit @-Rules

Du kannst auch Media Queries und andere @-Rules verschachteln.

```scss
.card {
  color: black;

  @media (max-width: 600px) {
    color: gray;
  }
}
```

Erzeugt:

```css
.card {
  color: black;
}
@media (max-width: 600px) {
  .card {
    color: gray;
  }
}
```

---

## Beispiel: Komplexe Verschachtelung

```scss
.menu {
  list-style: none;

  li {
    display: inline-block;

    a {
      color: black;

      &:hover {
        color: orange;
      }
    }
  }
}
```

---

## Best Practices

- Nicht zu tief verschachteln (max. 3 Ebenen).
- Das `&` gezielt für Modifier und Pseudoklassen nutzen.
- Verschachtelung für logische Gruppierung verwenden, nicht für alles.

---

## Zusammenfassung

- SCSS-Verschachtelung bildet die HTML-Struktur im CSS ab.
- Das `&`-Zeichen steht für den Eltern-Selektor.
- @-Rules wie Media Queries können verschachtelt werden.
- Zu tiefe Verschachtelung vermeiden, um die Lesbarkeit zu erhalten.
