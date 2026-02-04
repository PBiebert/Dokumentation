[Zurück zur Übersicht](../README.md)

# State-Management mit BehaviorSubject im Service (am Beispiel Modal-Steuerung)

---

## 1. Was ist ein BehaviorSubject und warum im Service?

Ein `BehaviorSubject` ist ein spezielles Observable aus RxJS, das immer den
aktuellen Wert speichert und an neue Subscriber sofort ausliefert.  
Im Angular-Service eingesetzt, ermöglicht es ein zentrales, reaktives
State-Management für beliebige Komponenten.  
Das ist besonders nützlich, wenn mehrere Komponenten auf denselben Zustand
reagieren oder diesen verändern sollen – ohne komplexe Input-/Output-Ketten oder
EventEmitter.

**Kernidee:**

- Der Service hält den Zustand (State) als BehaviorSubject.
- Komponenten abonnieren diesen State und reagieren automatisch auf Änderungen.
- Methoden im Service (z.B. `showModal()`, `close()`) ändern den State.

---

## 2. Vorteile und typische Anwendungsfälle

### Vorteile

- **Zentraler State:** Eine Quelle der Wahrheit für den Zustand (z.B. ob ein
  Modal offen ist).
- **Reaktiv:** Komponenten werden automatisch aktualisiert, wenn sich der State
  ändert.
- **Entkopplung:** Komponenten müssen sich nicht direkt kennen, sondern
  kommunizieren über den Service.
- **Testbarkeit:** State-Logik ist im Service gebündelt und leicht testbar.

### Typische Anwendungsfälle

- Modale Fenster (Dialoge)
- Ladeindikatoren (Spinner)
- Globale Benachrichtigungen (Toasts)
- Authentifizierungsstatus
- Feature-Toggles

---

## 3. BehaviorSubject vs. Subject vs. andere Patterns

- **Subject:** Sendet Werte nur an aktuelle Subscriber, kein Startwert.
- **BehaviorSubject:** Hält immer den letzten Wert, neue Subscriber erhalten
  sofort den aktuellen Wert.
- **ReplaySubject:** Gibt eine definierte Anzahl vergangener Werte an neue
  Subscriber weiter.
- **EventEmitter:** Nur für Output in Angular-Komponenten gedacht, nicht für
  globales State-Management.

**BehaviorSubject ist ideal, wenn Komponenten immer den aktuellen Zustand kennen
müssen.**

---

## 4. Schritt-für-Schritt: Zentrales State-Management mit BehaviorSubject

### 1. Service mit BehaviorSubject anlegen

```typescript
// modal.service.ts
import { Injectable } from "@angular/core";
import { BehaviorSubject, Observable } from "rxjs";

@Injectable({ providedIn: "root" })
export class ModalService {
  // Der State: true = Modal offen, false = geschlossen
  private modalSubject = new BehaviorSubject<boolean>(false);

  // Observable für Komponenten (nur lesend)
  modal$: Observable<boolean> = this.modalSubject.asObservable();

  // State ändern
  showModal() {
    this.modalSubject.next(true);
  }
  close() {
    this.modalSubject.next(false);
  }

  // Optional: aktuellen Wert synchron abfragen
  get isOpen(): boolean {
    return this.modalSubject.value;
  }
}
```

### 2. Komponente abonniert den State

```typescript
// modal.component.ts
import { Component, OnDestroy } from "@angular/core";
import { ModalService } from "./modal.service";
import { Subscription } from "rxjs";

@Component({
  /* ... */
})
export class ModalComponent implements OnDestroy {
  show = false;
  private sub: Subscription;

  constructor(private modalService: ModalService) {
    // State abonnieren: show wird automatisch aktualisiert
    this.sub = this.modalService.modal$.subscribe((val) => (this.show = val));
  }

  ngOnDestroy() {
    this.sub.unsubscribe();
  }

  closeModal() {
    this.modalService.close();
  }
}
```

### 3. Template: Anzeige abhängig vom State

```html
<!-- modal.component.html -->
<div *ngIf="show" class="modal">
  <p>Modal-Inhalt</p>
  <button (click)="closeModal()">Schließen</button>
</div>
```

### 4. Modal von überall öffnen

```typescript
// irgendeine andere Komponente
constructor(private modalService: ModalService) {}

openModal() {
  this.modalService.showModal();
}
```

---

## 5. Tiefergehende Erklärungen

### Warum Service statt Direkt-Kommunikation?

- **Lose Kopplung:** Komponenten müssen sich nicht kennen.
- **Wiederverwendbarkeit:** Der Service kann von beliebig vielen Komponenten
  genutzt werden.
- **Single Source of Truth:** Der Zustand ist immer konsistent.

### Observer-Pattern

Das Observer-Pattern ist ein Entwurfsmuster, bei dem ein Objekt (Subject)
mehrere Beobachter (Observers) über Änderungen informiert.  
In Angular/RxJS ist das Subject (hier: BehaviorSubject) der Publisher, die
Komponenten sind die Subscriber.

### Fehlerquellen & Best Practices

- **Unsubscribe nicht vergessen:** Bei jedem Subscription im Component-Code im
  `ngOnDestroy` unsubscriben, sonst Memory-Leaks!
- **Keine Logik im Template:** State-Änderungen immer über Methoden im Service,
  nicht direkt im Template.
- **Initialwert beachten:** BehaviorSubject braucht immer einen Startwert.

---

## 6. Anwendungsbeispiel: Modal-Dialog

Das oben gezeigte Beispiel ist ein typischer Anwendungsfall:  
Ein zentrales Modal kann von überall geöffnet/geschlossen werden, ohne dass
Komponenten direkt miteinander kommunizieren.

---

## 7. Erweiterungen & Alternativen

- **Mehrere Modals:** Für mehrere Dialoge mehrere BehaviorSubjects oder ein
  Objekt als State verwenden.
- **Komplexerer State:** Statt `boolean` kann das Subject auch komplexe Objekte
  halten (z.B. `{ open: true, data: {...} }`).
- **State-Management Libraries:** Für große Apps bieten sich Libraries wie NgRx
  oder Akita an, die auf ähnlichen Prinzipien basieren.

---

## 8. Methodenübersicht

| Methode/Property | Beschreibung                             |
| ---------------- | ---------------------------------------- |
| `modal$`         | Observable für den aktuellen Modal-State |
| `showModal()`    | Modal öffnen                             |
| `close()`        | Modal schließen                          |
| `isOpen`         | Aktueller Wert synchron abfragbar        |

---

## 9. Fazit

Mit einem `BehaviorSubject` im Service implementierst du ein robustes, reaktives
State-Management in Angular.  
Das Pattern ist universell einsetzbar – nicht nur für Modals, sondern überall,
wo mehrere Komponenten auf denselben Zustand reagieren sollen.  
Es sorgt für sauberen, wartbaren und testbaren Code.

> **Hinweis:**  
> Ein `BehaviorSubject` ist nicht nur für Modals geeignet!  
> Es ist ein universelles Werkzeug für jegliches zentrales, reaktives State-Management in Angular.  
> Typische Einsatzgebiete sind z.B. Ladeindikatoren, Authentifizierungsstatus, globale Benachrichtigungen, Feature-Toggles oder das Teilen von Daten zwischen Komponenten.  
> Das Modal ist hier nur ein einfaches Anwendungsbeispiel.
