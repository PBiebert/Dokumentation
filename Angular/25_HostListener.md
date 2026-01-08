[Zurück zur Übersicht](../README.md)

# @HostListener – Events direkt am Host-Element abfangen in Angular

---

## Was ist @HostListener?

`@HostListener` ist ein Decorator in Angular, mit dem du Events direkt am
Host-Element deiner Komponente oder Directive abfangen kannst.  
Damit kannst du z.B. auf Klicks, Tastendrücke, Mausbewegungen oder andere
DOM-Events reagieren, ohne im Template Event-Bindings zu schreiben.

---

## Vorteile von @HostListener

- **Direkte Eventbindung** am Host-Element
- **Saubere Trennung** von Logik und Template
- **Einfache Syntax** für viele Event-Typen
- **Plattformunabhängig** und testbar

---

## Typische Anwendungsfälle

- Klicks, Mouseover, Mouseleave, Tastatur-Events am Host-Element abfangen
- Eigene Interaktionen für Directives und Komponenten definieren
- Globale Events wie `window:resize` oder `document:keydown` behandeln

---

## Beispiel: Klick auf das Host-Element

```typescript
// click.directive.ts
import { Directive, HostListener } from "@angular/core";

@Directive({
  selector: "[appClick]",
  standalone: true,
})
export class ClickDirective {
  @HostListener("click")
  onClick() {
    alert("Host-Element wurde geklickt!");
  }
}
```

> **Hinweis:**  
> Der Name der Methode (`onClick`) ist beliebig wählbar. Wichtig ist nur, dass
> sie mit `@HostListener` dekoriert ist und zum gewünschten Event passt.
>
> **Wichtig:**  
> Ein `@HostListener` bezieht sich immer nur auf die Methode, die direkt
> darunter steht.  
> Wenn du mehrere Events abfangen möchtest, kannst du einfach mehrere Methoden
> mit jeweils eigenem `@HostListener` in derselben Klasse anlegen. Jeder
> Listener ist unabhängig und reagiert nur auf das angegebene Event.

---

## Beispiel: Tastendruck am Host-Element

```typescript
// key.directive.ts
import { Directive, HostListener } from "@angular/core";

@Directive({
  selector: "[appKey]",
  standalone: true,
})
export class KeyDirective {
  @HostListener("keydown", ["$event"])
  handleKey(event: KeyboardEvent) {
    console.log("Taste gedrückt:", event.key);
  }
}
```

---

## Beispiel: Globales Event (Window Resize)

```typescript
// resize.directive.ts
import { Directive, HostListener } from "@angular/core";

@Directive({
  selector: "[appResize]",
  standalone: true,
})
export class ResizeDirective {
  @HostListener("window:resize", ["$event"])
  onResize(event: UIEvent) {
    console.log(
      "Fenstergröße geändert:",
      window.innerWidth,
      window.innerHeight
    );
  }
}
```

---

## Schritt-für-Schritt: @HostListener verwenden

1. **Importiere HostListener** aus `@angular/core`.
2. **Dekoriere eine Methode** mit `@HostListener("eventName")`.
3. **Optional:** Übergib Event-Objekte als Parameter mit `["$event"]`.
4. **Für globale Events:** Schreibe z.B. `"window:resize"` oder
   `"document:click"`.

---

## Methodenübersicht

| Syntax                                     | Beschreibung                     |
| ------------------------------------------ | -------------------------------- |
| @HostListener("click")                     | Klick-Event am Host-Element      |
| @HostListener("mouseenter")                | Mouseenter-Event am Host-Element |
| @HostListener("keydown", ["$event"])       | Tastendruck am Host-Element      |
| @HostListener("window:resize", ["$event"]) | Globales Resize-Event am Fenster |
| @HostListener("document:scroll")           | Scroll-Event am Dokument         |

---

## Hinweise

- **Mehrere Events:** Du kannst beliebig viele Methoden mit `@HostListener` in
  einer Klasse verwenden. Jeder `@HostListener` gilt immer nur für die Methode
  direkt darunter.
- **Event-Objekte:** Mit `["$event"]` erhältst du das native Event-Objekt.
- **Globale Events:** Für Events außerhalb des Host-Elements nutze
  `"window:event"` oder `"document:event"`.

---

## Fazit

Mit `@HostListener` kannst du Events am Host-Element oder global einfach und
sauber in Angular abfangen.  
Das sorgt für übersichtlichen Code und eine klare Trennung zwischen Template und
Logik.

---

## Beispiel: Mehrere Events in einer Directive

```typescript
// multi-event.directive.ts
import { Directive, HostListener } from "@angular/core";

@Directive({
  selector: "[appMultiEvent]",
  standalone: true,
})
export class MultiEventDirective {
  @HostListener("click")
  onClick() {
    console.log("Host-Element wurde geklickt!");
  }

  @HostListener("mouseenter")
  onMouseEnter() {
    console.log("Maus ist über dem Host-Element!");
  }

  @HostListener("mouseleave")
  onMouseLeave() {
    console.log("Maus hat das Host-Element verlassen!");
  }
}
```

> In diesem Beispiel hat jede Methode ihren eigenen `@HostListener`.  
> Jeder Listener reagiert nur auf das jeweilige Event und ist unabhängig von den
> anderen.
