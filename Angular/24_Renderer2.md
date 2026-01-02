[Zurück zur Übersicht](../README.md)

# Renderer2 – Sichere und plattformübergreifende DOM-Manipulation in Angular

---

## Was ist Renderer2?

`Renderer2` ist ein Service von Angular, der dir ermöglicht, DOM-Elemente sicher
und plattformübergreifend zu manipulieren.  
Statt direkt mit dem DOM zu arbeiten (z.B. über `ElementRef.nativeElement`),
solltest du für alle Änderungen am DOM den `Renderer2` verwenden.  
Das sorgt für mehr Sicherheit (z.B. XSS-Schutz) und macht deine Anwendung
kompatibel mit verschiedenen Plattformen wie Web, Mobile oder Server-Side
Rendering.

---

## Vorteile von Renderer2

- **Sicherheit:** Verhindert direkte Manipulationen am DOM und schützt vor
  Sicherheitslücken.
- **Plattformunabhängigkeit:** Funktioniert auch bei Server-Side Rendering und
  Web-Workern.
- **Klarer API:** Einheitliche Methoden für alle DOM-Operationen.
- **Testbarkeit:** Erleichtert das Testen von Komponenten und Directives.

---

## Typische Anwendungsfälle

- Dynamisches Hinzufügen/Entfernen von CSS-Klassen oder Styles
- Setzen von Attributen oder Properties
- Erstellen, Einfügen oder Entfernen von DOM-Elementen
- Reagieren auf Events

---

## Beispiel: Hintergrundfarbe setzen

Wir erstellen eine Directive, die die Hintergrundfarbe eines Elements mit
`Renderer2` setzt.

```typescript
// highlight.directive.ts
import { Directive, ElementRef, Input, OnInit, Renderer2 } from "@angular/core";

@Directive({
  selector: "[appHighlight]",
  standalone: true,
})
export class HighlightDirective implements OnInit {
  @Input({ required: false }) appHighlight: string = "yellow";

  constructor(private el: ElementRef, private renderer: Renderer2) {}

  ngOnInit() {
    this.renderer.setStyle(
      this.el.nativeElement,
      "backgroundColor",
      this.appHighlight
    );
  }
}
```

---

## Beispiel: Textinhalt ändern

```typescript
// uppercase.directive.ts
import { Directive, ElementRef, OnInit, Renderer2 } from "@angular/core";

@Directive({
  selector: "[appUppercase]",
  standalone: true,
})
export class UppercaseDirective implements OnInit {
  constructor(private el: ElementRef, private renderer: Renderer2) {}

  ngOnInit() {
    const native: HTMLElement = this.el.nativeElement;
    this.renderer.setProperty(
      native,
      "textContent",
      native.textContent?.toUpperCase() ?? ""
    );
  }
}
```

---

## Schritt-für-Schritt: Renderer2 verwenden

1. **Importiere Renderer2** aus `@angular/core`.
2. **Füge Renderer2 im Konstruktor** deiner Komponente oder Directive als
   Abhängigkeit hinzu.
3. **Nutze die Methoden von Renderer2** für DOM-Operationen, z.B.:
   - `setStyle(element, styleName, value)`
   - `removeStyle(element, styleName)`
   - `addClass(element, className)`
   - `removeClass(element, className)`
   - `setAttribute(element, attrName, value)`
   - `removeAttribute(element, attrName)`
   - `setProperty(element, propName, value)`
   - `listen(element, eventName, callback)`

---

## Methodenübersicht

| Methode         | Beschreibung                    |
| --------------- | ------------------------------- |
| setStyle        | Setzt einen CSS-Style           |
| removeStyle     | Entfernt einen CSS-Style        |
| addClass        | Fügt eine CSS-Klasse hinzu      |
| removeClass     | Entfernt eine CSS-Klasse        |
| setAttribute    | Setzt ein Attribut              |
| removeAttribute | Entfernt ein Attribut           |
| setProperty     | Setzt eine Property             |
| appendChild     | Fügt ein Kind-Element hinzu     |
| removeChild     | Entfernt ein Kind-Element       |
| createElement   | Erstellt ein neues Element      |
| createText      | Erstellt einen neuen Textknoten |
| listen          | Fügt einen Event-Listener hinzu |

---

## Hinweise

- **Immer Renderer2 verwenden**, wenn du das DOM manipulierst.
- Direkter Zugriff auf das DOM (`ElementRef.nativeElement`) ist nur in
  Ausnahmefällen sinnvoll.
- Renderer2 ist besonders wichtig für Universal/SSR und Web-Worker.

---

## Fazit

Mit `Renderer2` kannst du DOM-Elemente in Angular sicher, plattformübergreifend
und testbar manipulieren.  
Halte dich an diese Best Practice, um zukunftssichere und robuste Anwendungen zu
entwickeln.
