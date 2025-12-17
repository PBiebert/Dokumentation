[Back to Table of Contents](../README.md)

# Was sind Components in Angular?

**Components** (Komponenten) sind die zentralen Bausteine jeder
Angular-Anwendung. Sie steuern, wie ein bestimmter Teil der Benutzeroberfläche
aussieht und sich verhält.

## Aufbau einer Komponente

Eine Komponente besteht immer aus drei Hauptbestandteilen:

- **TypeScript-Datei (`.ts`)**: Enthält die Logik und Daten der Komponente.
- **HTML-Template (`.html`)**: Definiert das Aussehen (Struktur) der Komponente.
- **Stylesheet (`.scss` oder `.css`)**: Enthält die spezifischen Styles für
  diese Komponente.

Beispiel:

```
my-component/
├── my-component.ts
├── my-component.html
└── my-component.scss
```

## Wie funktionieren Komponenten?

- Jede Komponente kapselt einen bestimmten Bereich der Oberfläche und deren
  Verhalten.
- Komponenten können eigene Daten (Properties) und Methoden besitzen.
- Sie können miteinander kommunizieren, z.B. über **Input** (Daten von außen)
  und **Output** (Events nach außen).
- Komponenten werden in der Regel hierarchisch verschachtelt: Eine
  Hauptkomponente (Root Component) enthält weitere Komponenten.

## Deklaration einer Komponente

Eine Komponente wird mit dem `@Component`-Decorator in TypeScript definiert,
z.B.:

```typescript
import { Component } from "@angular/core";

@Component({
  selector: "app-beispiel",
  templateUrl: "./beispiel.component.html",
  styleUrls: ["./beispiel.component.scss"],
})
export class BeispielComponent {
  // Logik und Daten der Komponente
}
```

- **selector**: Der HTML-Tag, mit dem die Komponente im Template verwendet wird.
- **templateUrl**: Verweis auf das HTML-Template.
- **styleUrls**: Verweis auf die Styles.

## Vorteile von Komponenten

- **Wiederverwendbarkeit**: Einmal erstellte Komponenten können mehrfach
  verwendet werden.
- **Kapselung**: Jede Komponente ist für ihren eigenen Bereich zuständig.
- **Strukturierung**: Große Anwendungen werden übersichtlich und wartbar.

## Zusammenfassung

Komponenten sind das Herzstück von Angular-Anwendungen. Sie ermöglichen eine
modulare, wiederverwendbare und klar strukturierte Entwicklung von
Benutzeroberflächen.
