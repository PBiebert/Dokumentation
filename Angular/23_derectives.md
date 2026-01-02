[Zurück zur Übersicht](../README.md)

# Directives – Verhalten und Darstellung von Komponenten steuern

---

## Was sind Directives?

Directives (Direktiven) sind spezielle Klassen in Angular, mit denen du das
Aussehen oder Verhalten von HTML-Elementen direkt beeinflussen kannst.  
Sie funktionieren wie eigene HTML-Attribute und machen deine Templates
dynamischer und flexibler.

---

## Vorteile von Directives

- **Wiederverwendbarkeit:** Schreibe Logik einmal und nutze sie überall.
- **Saubere Templates:** Weniger Code im HTML, mehr Struktur.
- **Flexibilität:** Passe Aussehen und Verhalten von Elementen dynamisch an.
- **Kapselung:** Die Logik ist sauber in einer Klasse gekapselt.

---

## Wo gehören Directives hin?

Directives sind Teil deines Angular-Projekts und liegen meist im Ordner
`src/app/directives/` oder direkt im `src/app`-Verzeichnis.  
Jede Directive bekommt eine eigene Datei, z.B. `highlight.directive.ts` oder
`uppercase.directive.ts`.  
Sie werden wie Komponenten oder Services in dein Projekt eingebunden und im
Template als Attribute verwendet.

**Hinweis:**  
In größeren Angular-Projekten ist es üblich, wiederverwendbare Directives im
`shared`-Modul (`src/app/shared/`) abzulegen.  
Globale oder anwendungsspezifische Directives können auch im `core`-Modul
(`src/app/core/`) liegen.

- **shared:** Für Directives, Pipes und Komponenten, die in mehreren
  Modulen/Features genutzt werden.
- **core:** Für zentrale, meist einmalig genutzte Funktionalität (z.B. globale
  Directives, Services).

---

## Typen von Directives

- **Attribute Directives:** Ändern das Aussehen oder Verhalten eines Elements
  (z.B. `[ngStyle]`, `[ngClass]`).
- **Structural Directives:** Fügen Elemente zum DOM hinzu oder entfernen sie
  (z.B. `*ngIf`, `*ngFor`).

---

## Beispiel A: Farbe oder Aussehen ändern

Wir erstellen eine Directive, die die Hintergrundfarbe eines Elements setzt.

> **Best Practice:**  
> Verwende für DOM-Manipulationen immer den `Renderer2`.  
> Der direkte Zugriff auf das DOM mit `ElementRef` sollte nur in Ausnahmefällen
> genutzt werden.

```typescript
// highlight.directive.ts (empfohlen mit Renderer2)
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

**Alternative mit ElementRef (nicht empfohlen):**

```typescript
import { Directive, ElementRef, Input, OnInit, inject } from "@angular/core";

@Directive({
  selector: "[appHighlight]",
  standalone: true,
})
export class HighlightDirective implements OnInit {
  @Input({ required: false }) appHighlight: string = "yellow";

  private el = inject(ElementRef);

  ngOnInit() {
    this.el.nativeElement.style.backgroundColor = this.appHighlight;
  }
}
```

**So verwendest du die Directive im Template:**

```html
<p [appHighlight]="'lightgreen'">Dieser Text hat einen grünen Hintergrund.</p>
<p appHighlight>Dieser Text hat einen gelben Hintergrund.</p>
```

---

## Beispiel B: Text automatisch großschreiben

Wir erstellen eine Directive, die den Text eines Elements automatisch in
Großbuchstaben umwandelt.

> **Best Practice:**  
> Auch für Textmanipulationen sollte `Renderer2` verwendet werden.  
> `ElementRef` ist nur als Fallback gedacht.

```typescript
// uppercase.directive.ts (empfohlen mit Renderer2)
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

**Alternative mit ElementRef (nicht empfohlen):**

```typescript
import { Directive, ElementRef, OnInit, inject } from "@angular/core";

@Directive({
  selector: "[appUppercase]",
  standalone: true,
})
export class UppercaseDirective implements OnInit {
  private el = inject(ElementRef);

  ngOnInit() {
    const native: HTMLElement = this.el.nativeElement;
    native.textContent = native.textContent?.toUpperCase() ?? "";
  }
}
```

**So verwendest du die Directive im Template:**

```html
<p appUppercase>dieser text wird automatisch GROSS geschrieben.</p>
```

---

## Schritt-für-Schritt: Eigene Directive erstellen

1. **Erstelle eine neue Datei** für deine Directive, z.B.
   `highlight.directive.ts`.
2. **Nutze den `@Directive`-Decorator** und gib einen eindeutigen Selector an.
3. **Best Practice:**  
   Verwende für DOM-Manipulationen den `Renderer2` (über Konstruktor-Injektion),
   um plattformübergreifend und sicher zu arbeiten.  
   `ElementRef` (mit `inject()` oder Konstruktor) sollte nur in Ausnahmefällen
   direkt genutzt werden.
4. **Passe das Element im Code an** (z.B. Farbe, Text, Stil).
5. **Nutze die Directive als Attribut** im Template.

---

## Weitere Beispiele

### Structural Directive: \*ngIf

```html
<div *ngIf="isVisible">
  Dieser Block wird nur angezeigt, wenn isVisible true ist.
</div>
```

### Attribute Directive: ngClass

```html
<button [ngClass]="{ active: isActive }">Klick mich</button>
```

---

## Hinweise zu Angular 16+ / 20

- **Standalone Directives:** Neue Directives sollten möglichst als
  `standalone: true` deklariert werden.
- **Dependency Injection:** Verwende `inject()` oder Konstruktor-Injektion für
  Abhängigkeiten.  
  Für DOM-Manipulationen immer `Renderer2` bevorzugen.
- **Input-Validierung:** Nutze `@Input({ required: true })` für verpflichtende
  Inputs.
- **Imports:** Importiere Angular-APIs direkt aus ihren Paketen, z.B.
  `import { Directive } from '@angular/core'`.

---

## Fazit

Directives sind ein mächtiges Werkzeug in Angular, um das Verhalten und die
Darstellung von Komponenten flexibel zu steuern.  
Mit eigenen Directives kannst du z.B. Farben, Schriftgrößen oder andere
Eigenschaften zentral und wiederverwendbar steuern – und sogar Text automatisch
umwandeln.
