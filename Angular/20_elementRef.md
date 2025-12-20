[Zurück zur Übersicht](../README.md)

# ElementRef – Direktes Arbeiten mit DOM-Elementen

---

## Was ist ElementRef?

`ElementRef` ist eine Angular-Klasse, mit der du direkten Zugriff auf ein
DOM-Element erhältst, das von einer Komponente oder Directive referenziert
wird.  
Damit kannst du Eigenschaften und Styles des Elements direkt verändern.

---

## Vorteile und Hinweise

- **Direkter DOM-Zugriff:** Ermöglicht Manipulation von HTML-Elementen aus
  TypeScript.
- **Flexibilität:** Du kannst Styles, Attribute oder Inhalte gezielt anpassen.
- **Achtung:** Direkter DOM-Zugriff sollte sparsam und bewusst eingesetzt
  werden, um die Sicherheit (z.B. XSS) und Plattformunabhängigkeit zu
  gewährleisten.

---

## Begriffe

- **nativeElement:** Das eigentliche DOM-Element, auf das du zugreifst
  (`this.el.nativeElement`).
- **Dependency Injection:** `ElementRef` wird per DI in Komponenten oder
  Directives eingebunden.

---

## Schritt-für-Schritt: ElementRef verwenden

### 1. In einer Directive nutzen

```typescript
import { Directive, ElementRef, OnInit, inject } from "@angular/core";

@Directive({
  selector: "[appHighlight]",
  standalone: true,
})
export class HighlightDirective implements OnInit {
  private el = inject(ElementRef);

  ngOnInit() {
    this.el.nativeElement.style.backgroundColor = "yellow";
  }
}
```

**Erklärung:**

- `inject(ElementRef)` gibt dir Zugriff auf das Element, an das die Directive
  gebunden ist.
- Mit `this.el.nativeElement.style.backgroundColor` änderst du direkt das
  Aussehen.

---

### 2. In einer Komponente nutzen

```typescript
import { Component, ElementRef, ViewChild } from "@angular/core";

@Component({
  selector: "app-example",
  template: `<p #myPara>Hallo Welt!</p>`,
  standalone: true,
})
export class ExampleComponent {
  @ViewChild("myPara", { static: true }) para!: ElementRef;

  ngOnInit() {
    this.para.nativeElement.style.fontWeight = "bold";
  }
}
```

**Erklärung:**

- Mit `@ViewChild` holst du dir ein Element aus dem Template.
- Über `para.nativeElement` kannst du das Element direkt manipulieren.

---

## Best Practices

- **Vermeide unnötigen DOM-Zugriff:** Nutze Angular-Bindings und Renderer, wenn
  möglich.
- **Nutze Renderer2 für komplexe Manipulationen:** Damit bleibt dein Code
  plattformunabhängig und sicher.
- **Direkter Zugriff ist für einfache Anpassungen (z.B. Farbe, Text) ok.**

---

## Fazit

`ElementRef` ist ein nützliches Werkzeug, um gezielt auf DOM-Elemente
zuzugreifen und sie zu verändern.  
Setze es bewusst und sparsam ein, um die Vorteile von Angular voll auszuschöpfen
und Sicherheitsrisiken zu vermeiden.
