[Zurück zur Übersicht](../README.md)

# Lifecycle-Hooks in Angular

---

## Was sind Lifecycle-Hooks?

Lifecycle-Hooks sind spezielle Methoden, die Angular zu bestimmten Zeitpunkten
im Lebenszyklus einer Komponente oder Directive automatisch aufruft.  
Sie ermöglichen dir, auf Ereignisse wie Initialisierung, Änderungen, Rendern
oder Zerstörung zu reagieren und gezielt Code auszuführen.

---

## Übersicht Lifecycle-Hooks

| Hook              | Wann wird er aufgerufen?                                      | Typische Anwendungsfälle          |
| ----------------- | ------------------------------------------------------------- | --------------------------------- |
| `ngOnInit`        | Nach der Konstruktion und dem Setzen aller Inputs             | Initialisierung, Daten laden      |
| `ngAfterViewInit` | Nachdem das View-Template und alle Child-Views aufgebaut sind | DOM-Zugriff, Animationen, Plugins |

---

# OnInit – Initialisierung von Komponenten und Directives

---

## Was ist OnInit?

`OnInit` ist ein Lifecycle-Interface in Angular.  
Es ermöglicht dir, Code auszuführen, sobald eine Komponente oder Directive
fertig initialisiert ist – also nachdem alle Eingabewerte (`@Input`) gesetzt
wurden.

---

## Wann verwendet man OnInit?

Du verwendest `OnInit`, wenn du Initialisierungscode ausführen möchtest, der auf
bereits gesetzte Eingabewerte (`@Input`) oder injizierte Abhängigkeiten
zugreifen muss. Typische Anwendungsfälle sind:

- Wenn du Daten laden oder asynchrone Operationen beim Start der Komponente
  ausführen willst.
- Wenn du Werte berechnen oder initialisieren möchtest, die von Inputs abhängen.
- Wenn du DOM-Manipulationen oder Initialisierungen in einer Directive
  durchführen willst.
- Immer dann, wenn der Konstruktor nicht ausreicht, weil dort noch keine Inputs
  gesetzt sind.

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

---

# AfterViewInit – Zugriff auf das View-Template nach der Initialisierung

---

## Was ist AfterViewInit?

`AfterViewInit` ist ein Lifecycle-Interface in Angular.  
Es ermöglicht dir, Code auszuführen, sobald das View-Template einer Komponente  
und alle Child-Komponenten/Directives vollständig initialisiert und im DOM
verfügbar sind.

---

## Wann verwendet man AfterViewInit?

Du verwendest `AfterViewInit`, wenn du auf Elemente im Template zugreifen oder  
DOM-Manipulationen durchführen möchtest, die erst nach dem Rendern des Views
möglich sind.  
Typische Anwendungsfälle sind:

- Zugriff auf Template-Referenzen (`@ViewChild`, `@ViewChildren`)
- Initialisierung von Drittanbieter-Bibliotheken, die DOM benötigen
- Animationen oder Messungen nach dem Rendern
- DOM-Manipulationen, die erst nach dem Aufbau des Views möglich sind

---

## Vorteile von AfterViewInit

- **Sicherer DOM-Zugriff:** Alle View-Elemente sind garantiert verfügbar.
- **Integration externer Bibliotheken:** Initialisierung von Plugins, die DOM
  benötigen.
- **Strukturierter Code:** Trennung von Initialisierungslogik und DOM-bezogener
  Logik.

---

## Begriffe

- **Lifecycle-Hook:** Methoden, die Angular zu bestimmten Zeitpunkten im
  Lebenszyklus einer Komponente/Directive aufruft.
- **ngAfterViewInit():** Die Methode, die du implementierst, um auf das
  AfterViewInit-Ereignis zu reagieren.

---

## Schritt-für-Schritt: AfterViewInit verwenden

### 1. Interface implementieren

```typescript
import { Component, AfterViewInit, ViewChild, ElementRef } from "@angular/core";

@Component({
  selector: "app-example",
  template: `<div #box>Box</div>`,
  standalone: true,
})
export class ExampleComponent implements AfterViewInit {
  @ViewChild("box") box!: ElementRef;

  ngAfterViewInit() {
    // Zugriff auf das DOM-Element nach dem Rendern
    this.box.nativeElement.style.backgroundColor = "lightblue";
    console.log("View wurde initialisiert!");
  }
}
```

**Erklärung:**

- Mit `implements AfterViewInit` verpflichtest du dich, die Methode
  `ngAfterViewInit()` zu schreiben.
- Angular ruft `ngAfterViewInit()` automatisch auf, nachdem das View-Template
  und alle Child-Views aufgebaut wurden.
- Erst hier sind z.B. `@ViewChild`-Elemente verfügbar.

---

### 2. In einer Directive verwenden

```typescript
import { Directive, ElementRef, AfterViewInit, inject } from "@angular/core";

@Directive({
  selector: "[appFocus]",
  standalone: true,
})
export class FocusDirective implements AfterViewInit {
  private el = inject(ElementRef);

  ngAfterViewInit() {
    this.el.nativeElement.focus();
  }
}
```

---

## Best Practices

- **Kein DOM-Zugriff im Konstruktor oder ngOnInit:** Erst in `ngAfterViewInit`
  ist das View-Template verfügbar.
- **Initialisierung von Plugins/Animationen:** Starte externe Bibliotheken oder
  Animationen in `ngAfterViewInit`.
- **Weitere Lifecycle-Hooks:** Es gibt noch mehr Hooks wie `OnInit`,
  `AfterContentInit`, `OnDestroy` usw.

---

## Fazit

`AfterViewInit` sorgt dafür, dass du sicher auf das gerenderte View-Template und
dessen Elemente zugreifen kannst.  
Das ist besonders wichtig für DOM-Manipulationen, Animationen oder die
Integration von Drittanbieter-Bibliotheken.
