[Back to Table of Contents](../README.md)

# SCSS Verschachtelung (Nesting)

## Was sind At-Rules?

At-Rules sind spezielle CSS-Anweisungen, die mit einem `@` beginnen. Sie steuern
das Verhalten des Stylesheets oder fügen zusätzliche Informationen hinzu.
Beispiele sind:

- `@media` – Für Media Queries (z.B. unterschiedliche Styles für verschiedene
  Bildschirmgrößen)
- `@import` – Um andere Stylesheets zu laden
- `@font-face` – Um eigene Schriftarten zu definieren
- `@keyframes` – Für CSS-Animationen

In SCSS können At-Rules verschachtelt werden, um sie direkt an einen Selektor zu
binden.

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
