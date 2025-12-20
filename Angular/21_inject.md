[Zurück zur Übersicht](../README.md)

# inject() – Moderne Dependency Injection in Angular

---

## Was ist inject()?

`inject()` ist eine Funktion in Angular (ab Version 14), mit der du Abhängigkeiten (z.B. Services, ElementRef) direkt im Code beziehen kannst – ohne sie im Konstruktor zu deklarieren.  
Sie wird vor allem in Standalone-Komponenten, -Directives und -Services verwendet.

---

## Vorteile von inject()

- **Kürzerer Code:** Kein Konstruktor nötig, weniger Boilerplate.
- **Klarheit:** Abhängigkeiten sind direkt im Code sichtbar.
- **Flexibel:** Besonders praktisch in Funktionen, Hooks und Standalone-APIs.

---

## Begriffe

- **Dependency Injection (DI):** Prinzip, bei dem Abhängigkeiten automatisch bereitgestellt werden.
- **inject():** Holt die gewünschte Abhängigkeit aus dem Angular-Injektor.

---

## Schritt-für-Schritt: inject() verwenden

### 1. In einer Directive nutzen

```typescript
import { Directive, ElementRef, OnInit, inject } from '@angular/core';

@Directive({
  selector: '[appHighlight]',
  standalone: true,
})
export class HighlightDirective implements OnInit {
  private el = inject(ElementRef);

  ngOnInit() {
    this.el.nativeElement.style.backgroundColor = 'yellow';
  }
}
```

**Erklärung:**

- `inject(ElementRef)` gibt dir Zugriff auf das DOM-Element.
- Kein Konstruktor nötig – die Abhängigkeit wird direkt als Property gesetzt.

---

### 2. In einer Komponente einen Service injizieren

```typescript
import { Component, inject } from '@angular/core';
import { FruitlistdataService } from './fruitlistdata.service';

@Component({
  selector: 'app-fruitlist',
  template: `...`,
  standalone: true,
})
export class FruitlistComponent {
  fruitService = inject(FruitlistdataService);

  // Jetzt kannst du fruitService im Code verwenden
}
```

---

## Best Practices

- **Verwende inject() für neue Standalone-Komponenten, -Directives und -Services.**
- **Für klassische Komponenten mit Konstruktor bleibt auch die alte Syntax gültig.**
- **inject() kann nicht im Konstruktor selbst verwendet werden, sondern nur auf Klassenebene oder in Methoden.**

---

## Fazit

Mit `inject()` kannst du Abhängigkeiten in Angular noch einfacher und übersichtlicher nutzen.  
Gerade in modernen Projekten mit Standalone-APIs ist `inject()` der empfohlene Weg für Dependency Injection.
