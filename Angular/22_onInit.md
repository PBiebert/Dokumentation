[Zurück zur Übersicht](../README.md)

# OnInit – Initialisierung von Komponenten und Directives

---

## Was ist OnInit?

`OnInit` ist ein Lifecycle-Interface in Angular.  
Es ermöglicht dir, Code auszuführen, sobald eine Komponente oder Directive
fertig initialisiert ist – also nachdem alle Eingabewerte (`@Input`) gesetzt
wurden.

---

## Vorteile von OnInit

- **Klarer Startpunkt:** Initialisierungslogik ist sauber getrennt vom
  Konstruktor.
- **Sicherer Zugriff:** Alle Inputs und Abhängigkeiten sind gesetzt.
- **Strukturierter Code:** Bessere Übersicht und Wartbarkeit.

---

## Begriffe

- **Lifecycle-Hook:** Methoden, die Angular zu bestimmten Zeitpunkten im
  Lebenszyklus einer Komponente/Directive aufruft.
- **ngOnInit():** Die Methode, die du implementierst, um auf das OnInit-Ereignis
  zu reagieren.

---

## Schritt-für-Schritt: OnInit verwenden

### 1. Interface implementieren

```typescript
import { Component, OnInit } from "@angular/core";

@Component({
  selector: "app-example",
  template: `<p>Hallo Welt!</p>`,
  standalone: true,
})
export class ExampleComponent implements OnInit {
  ngOnInit() {
    // Initialisierungscode hier
    console.log("Komponente wurde initialisiert!");
  }
}
```

**Erklärung:**

- Mit `implements OnInit` verpflichtest du dich, die Methode `ngOnInit()` zu
  schreiben.
- Angular ruft `ngOnInit()` automatisch nach der Konstruktion und dem Setzen
  aller Inputs auf.

---

### 2. In einer Directive verwenden

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

---

## Best Practices

- **Konstruktor nur für DI:** Im Konstruktor nur Abhängigkeiten injizieren,
  keine Logik ausführen.
- **Initialisierung in ngOnInit:** Lade Daten, setze Werte oder führe Logik in
  `ngOnInit()` aus.
- **Weitere Lifecycle-Hooks:** Es gibt noch mehr Hooks wie `OnDestroy`,
  `OnChanges` usw.

---

## Fazit

`OnInit` sorgt dafür, dass deine Initialisierungslogik immer zum richtigen
Zeitpunkt ausgeführt wird.  
Das macht deinen Code robuster, übersichtlicher und besser wartbar.
