[Back to Table of Contents](../README.md)

# Routing in Angular

---

## Was ist Routing?

Routing ermöglicht es, verschiedene Komponenten (Seiten) in einer
Angular-Anwendung über URLs anzuzeigen. Jede Route ist mit einem Pfad und einer
Komponente verknüpft.

---

## Inhaltsverzeichnis

1. [Was ist Routing?](#was-ist-routing)
2. [!! WICHTIG !!](#-wichtig-)
3. [Routen definieren](#routen-definieren)
4. [RouterOutlet verwenden](#routeroutlet-verwenden)
5. [Komponenten für Routen](#komponenten-für-routen)
6. [Navigation zwischen Seiten](#navigation-zwischen-seiten)
7. [Beispiel: Footer mit Navigation](#beispiel-footer-mit-navigation)
8. [Zusammenfassung](#zusammenfassung)

---

## !! WICHTIG !!

**Eine Route sollte nie `main` heißen!**  
Das führt zu Problemen, da z.B. eine Datei `main.js` im Projekt vorhanden ist.  
Verwende stattdessen andere Namen für deine Routen.

---

## Routen definieren

Routen werden in einer Datei wie `app.routes.ts` als Array von Objekten
definiert.

```ts
import { Routes } from "@angular/router";
import { MainContent } from "./main-content/main-content";
import { Imprint } from "./imprint/imprint";

export const routes: Routes = [
  { path: "", component: MainContent }, // Startseite
  { path: "imprint", component: Imprint }, // Impressum-Seite
];
```

- **path:** Der URL-Teil, der die Route auslöst (z.B. `imprint`).
- **component:** Die Komponente, die angezeigt wird.

---

## RouterOutlet verwenden

Das `<router-outlet>`-Tag im Template (`app.html`) ist der Platzhalter, an dem
die jeweils aktive Komponente angezeigt wird.

```html
<router-outlet></router-outlet> <app-footer></app-footer>
```

---

## Komponenten für Routen

Jede Route zeigt eine eigene Komponente an. Beispiel:

- **MainContent:** Wird bei leerem Pfad (`''`) angezeigt.
- **Imprint:** Wird bei `/imprint` angezeigt.

---

## Navigation zwischen Seiten

Um zwischen Seiten zu navigieren, verwendet man Links mit dem Pfad der Route.

```html
<a routerLink="imprint">Impressum</a>
```

**Auch Buttons können für Navigation genutzt werden:**

```html
<button [routerLink]="['/imprint']">Impressum</button>
```

---

## Beispiel: Footer mit Navigation

Im Footer können Links zu verschiedenen Routen eingebaut werden:

```html
<footer>
  <!-- ...existing code... -->
  <a routerLink="imprint">Impressum</a>
  <button [routerLink]="['/imprint']">Impressum</button>
</footer>
```

---

## Zusammenfassung

- Routen werden als Array von Objekten definiert.
- `<router-outlet>` zeigt die jeweils aktive Komponente an.
- Mit Links kann zwischen den Seiten navigiert werden.
- Jede Route ist mit einer Komponente verknüpft.
