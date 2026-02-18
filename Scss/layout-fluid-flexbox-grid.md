[Zurück zum Inhaltsverzeichnis](../README.md)

# Fluides Layout mit Flexbox & Grid in SCSS

## Inhaltsverzeichnis

- [Einleitung](#einleitung)
- [Maximale Flexibilität ohne feste Größen](#maximale-flexibilität-ohne-feste-größen)
  - [Flexbox: Mit `flex-grow` flexibel verteilen](#flexbox-mit-flex-grow-flexibel-verteilen)
  - [Grid: Dynamische Spalten mit `auto-fit` und `minmax`](#grid-dynamische-spalten-mit-auto-fit-und-minmax)
- [Flexbox: Flexible Container](#flexbox-flexible-container)
- [Grid: Zweidimensionale Layouts](#grid-zweidimensionale-layouts)
- [Absolut positionierte Elemente](#absolut-positionierte-elemente)
- [Beispiel: Fluides Layout in der `about-me`- und `stack`-Section](#beispiel-fluides-layout-in-der-about-me-und-stack-section)
- [Zusammenfassung](#zusammenfassung)

---

## Einleitung

Ein fluides Layout passt sich flexibel an verschiedene Bildschirmgrößen an. Mit
SCSS, Flexbox und CSS Grid kannst du moderne, responsive Layouts einfach
umsetzen. Absolut positionierte Elemente benötigen dabei besondere
Aufmerksamkeit, um die Fluidität nicht zu beeinträchtigen.

---

## Maximale Flexibilität ohne feste Größen

### Flexbox: Mit `flex-grow` flexibel verteilen

Mit `flex-grow` kannst du Elemente so gestalten, dass sie den verfügbaren Platz
im Container optimal ausnutzen – ganz ohne feste Breiten.

**Beispiel: Zwei Spalten in der `about-me`-Section**

```scss
@use "./../../../../shared/styles/core" as *;

.wrapper {
  @include flexbox($flex-direction: row);
  gap: 50px;

  aside,
  .about-me-container {
    flex-grow: 1; // Beide wachsen gleichmäßig
    min-width: 0; // Verhindert Überlauf bei langen Inhalten
  }
}
```

- `flex-grow: 1;` sorgt dafür, dass beide Spalten den verfügbaren Platz
  gleichmäßig aufteilen.
- Mit `min-width: 0;` wird verhindert, dass Inhalte das Layout sprengen.

**Noch flexibler: Unterschiedliche Wachstumsfaktoren**

```scss
aside {
  flex-grow: 2; // wächst doppelt so stark wie .about-me-container
}
.about-me-container {
  flex-grow: 1;
}
```

**Dynamisches Verhalten sichtbar machen:**

Wenn du die Breite des Browserfensters änderst, wachsen und schrumpfen die
beiden Bereiche dynamisch mit – ohne dass eine feste Breite gesetzt ist.

---

### Grid: Dynamische Spalten mit `auto-fit` und `minmax`

Mit CSS Grid kannst du responsive, dynamische Spalten ohne feste Anzahl oder
Breite erzeugen.

**Beispiel: Stack-Icons dynamisch anordnen**

```scss
.stack-container {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(120px, 1fr));
  gap: 25px;

  .stack-item {
    width: 100%; // nimmt die volle Spaltenbreite ein
    min-width: 0;
    @include flexbox($flex-direction: column);
    align-items: center;

    img {
      width: 70%;
      max-width: 80px;
      aspect-ratio: 1/1;
      filter: grayscale(1);
      transition: filter 0.2s;
    }

    &:hover img {
      filter: grayscale(0);
    }

    p {
      margin-top: 0.5rem;
      font-size: 1rem;
      color: $color-font-light;
    }
  }
}
```

- `auto-fit` füllt die Zeile automatisch mit so vielen Spalten wie möglich.
- `minmax(120px, 1fr)` sorgt dafür, dass jede Spalte mindestens 120px breit ist,
  aber flexibel wächst.
- `.stack-item` wächst dynamisch mit der Spalte und bleibt immer gleichmäßig
  verteilt.

**Dynamisches Verhalten sichtbar machen:**

Wenn du die Breite des Containers oder Fensters änderst, passt sich die Anzahl
der Spalten automatisch an – die Icons werden immer optimal verteilt.

---

## Flexbox: Flexible Container

> **Hinweis:** Das Flexbox-Mixin ist in  
> `src/app/shared/styles/_core.scss`  
> definiert und wird z.B. so eingebunden:
>
> ```scss
> @use "./../../../../shared/styles/core" as *;
> ```

Flexbox eignet sich hervorragend, um Elemente in einer Zeile oder Spalte
flexibel anzuordnen.

**Tipp:**  
Standardmäßig werden Flexbox-Items mit `align-items: stretch` in der Höhe
gestreckt, sodass sie die volle Höhe des Containers einnehmen.  
Das ist besonders sinnvoll, wenn du möchtest, dass alle Spalten oder Bereiche
innerhalb einer `section` immer gleich hoch sind – z.B. bei
nebeneinanderliegenden Cards oder Spalten.

**Beispiel: Section mit gestreckten Flex-Items**

```scss
section {
  @include flexbox($align-items: stretch);
}
```

- Verwende dies, wenn du möchtest, dass alle direkten Kinder einer Section die
  gleiche Höhe bekommen.
- Besonders nützlich bei Layouts mit mehreren Spalten, die optisch gleichmäßig
  wirken sollen.

**Beispiel aus der `about-me`-Section:**

```scss
@use "./../../../../shared/styles/core" as *;

.wrapper {
  @include flexbox($flex-direction: row);
  gap: 50px;

  aside,
  .about-me-container {
    flex-grow: 1;
    min-width: 0;
  }
}
```

- `@include flexbox($flex-direction: row);` sorgt für eine horizontale
  Anordnung.
- Mit `gap` wird der Abstand zwischen den Elementen flexibel gehalten.
- `flex-grow: 1;` macht die Spalten dynamisch.

**Responsives Umschalten:**

```scss
@media (max-width: 820px) {
  .wrapper {
    @include flexbox($flex-direction: column-reverse);
    gap: 0px;

    aside,
    .about-me-container {
      width: 100%;
      flex-grow: unset;
    }
  }
}
```

- Bei kleineren Bildschirmen werden die Bereiche untereinander angeordnet und
  nehmen die volle Breite ein.

---

## Grid: Zweidimensionale Layouts

CSS Grid ist ideal, wenn du komplexere, zweidimensionale Layouts benötigst.

**Beispiel: Dynamisches Grid für Stack-Icons**

```scss
.stack-container {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(120px, 1fr));
  gap: 25px;
}
```

- `grid-template-columns` mit `auto-fit` und `minmax` sorgt für maximale
  Flexibilität.
- Die Anzahl der Spalten passt sich automatisch an die verfügbare Breite an.

**Responsives Grid:**

```scss
@media (max-width: 700px) {
  .stack-container {
    grid-template-columns: repeat(auto-fit, minmax(100px, 1fr));
    gap: 15px;
  }
}
```

- Das Grid bleibt auch auf kleinen Bildschirmen dynamisch und flexibel.

---

## Absolut positionierte Elemente

Absolut positionierte Elemente (`position: absolute`) werden aus dem normalen
Layoutfluss genommen. Damit sie trotzdem responsiv bleiben:

1. **Eltern-Element mit `position: relative`:**

   ```scss
   .relative-parent {
     position: relative;
   }
   .absolute-child {
     position: absolute;
     top: 1rem;
     right: 1rem;
     // Responsive Anpassungen per Media Query
   }
   ```

2. **Größen und Abstände in relativen Einheiten (`%`, `vw`, `rem`) angeben.**

3. **Media Queries nutzen, um Positionen auf kleinen Bildschirmen anzupassen.**

**Beispiel:**

```scss
.card {
  position: relative;

  .badge {
    position: absolute;
    top: 1rem;
    right: 1rem;
    font-size: 1.2rem;
    padding: 0.5rem 1rem;
    background: $color-accent-yellow;
    border-radius: 1rem;
  }
}

@media (max-width: 600px) {
  .card .badge {
    top: 0.5rem;
    right: 0.5rem;
    font-size: 1rem;
    padding: 0.25rem 0.5rem;
  }
}
```

---

## Beispiel: Fluides Layout in der `about-me`- und `stack`-Section

**about-me (Flexbox, dynamisch):**

```scss
@use "./../../../../shared/styles/core" as *;

.wrapper {
  @include flexbox($flex-direction: row);
  gap: 50px;

  aside,
  .about-me-container {
    flex-grow: 1;
    min-width: 0;
  }
}
```

**stack (Grid, dynamisch):**

```scss
.stack-container {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(120px, 1fr));
  gap: 25px;

  .stack-item {
    width: 100%;
    min-width: 0;
    @include flexbox($flex-direction: column);
    align-items: center;
  }
}
```

---

## Zusammenfassung

- **Flexbox** für einfache, ein-achsige Layouts – mit `flex-grow` werden
  Elemente maximal flexibel.
- **Tipp:** Mit `align-items: stretch` im Flexbox-Container (`section { ... }`)
  werden alle Kinder gleich hoch gestreckt – ideal für gleichmäßige Spalten.
- **Grid** für komplexe, zwei-achsige Layouts – mit `auto-fit` und `minmax`
  werden Spalten dynamisch.
- **Absolut positionierte Elemente** immer im Kontext eines relativ
  positionierten Elternteils und mit responsiven Einheiten gestalten.
- **Media Queries** sind essenziell, um Layouts auf allen Geräten nutzbar zu
  machen.

> **Tipp:** Nutze SCSS-Mixins für wiederverwendbare Flexbox- und Grid-Layouts!
