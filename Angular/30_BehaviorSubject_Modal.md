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
import { Injectable } from "@angular/core"; // Macht die Klasse als Service nutzbar
import { BehaviorSubject, Observable } from "rxjs"; // Importiert RxJS für reaktive Programmierung

@Injectable({ providedIn: "root" }) // Service wird global bereitgestellt
export class ModalService {
  // Zentrale Quelle für den Modal-Status
  // Erstellt ein BehaviorSubject mit Startwert 'false' (Modal ist geschlossen)
  private modalSubject = new BehaviorSubject<boolean>(false);

  // Gibt das Subject als Observable nach außen, damit Komponenten abonnieren können
  // Das $ am Ende von 'modal$' ist eine Konvention, um zu kennzeichnen, dass es sich um ein Observable handelt.
  // So erkennt man direkt, dass man dieses Property abonnieren kann und nicht direkt verändert.
  // Die Methode asObservable() wandelt das BehaviorSubject in ein Observable um.
  // Dadurch können andere Komponenten den Wert abonnieren, aber nicht direkt verändern.
  modal$: Observable<boolean> = this.modalSubject.asObservable();

  // Öffnet das Modal, indem der Wert des Subjects auf 'true' gesetzt wird
  showModal() {
    this.modalSubject.next(true);
  }

  // Schließt das Modal, indem der Wert des Subjects auf 'false' gesetzt wird
  close() {
    this.modalSubject.next(false);
  }

  // Gibt den aktuellen Wert des Subjects synchron zurück
  get isOpen(): boolean {
    return this.modalSubject.value;
  }
}
```

**Detailierte Kommentare zu den Befehlen:**

- `private modalSubject = new BehaviorSubject<boolean>(false);`  
  Erstellt ein BehaviorSubject, das immer einen aktuellen Wert hält (hier:
  `false` = Modal geschlossen).  
  Das Subject ist privat, damit nur der Service den Wert direkt verändern kann.
- `modal$: Observable<boolean> = this.modalSubject.asObservable();`  
  Gibt das Subject als Observable nach außen, damit andere Komponenten den Wert
  abonnieren können, aber nicht direkt verändern.  
  Die `$`-Konvention zeigt an, dass es sich um ein Observable handelt.
- `showModal()` und `close()`  
  Methoden, um den State zentral zu ändern. Komponenten rufen diese Methoden
  auf, statt direkt den Wert zu setzen.
- `get isOpen(): boolean`  
  Ermöglicht synchronen Zugriff auf den aktuellen Wert, z.B. für Logik im
  Service.

---

### 2. Komponente abonniert den State

```typescript
// modal.component.ts
import { Component, OnDestroy } from "@angular/core"; // Basis für Angular-Komponenten
import { ModalService } from "./modal.service"; // Importiert den Service
import { Subscription } from "rxjs"; // Ermöglicht das Verwalten von Subscriptions

@Component({
  /* ... */
})
export class ModalComponent implements OnDestroy {
  show = false; // Steuert die Anzeige des Modals
  private sub: Subscription; // Speichert die Subscription

  constructor(private modalService: ModalService) {
    // Abonniert das Observable aus dem Service
    // Bei jeder Änderung wird 'show' aktualisiert
    this.sub = this.modalService.modal$.subscribe((val) => (this.show = val));
  }

  ngOnDestroy() {
    // Subscription aufräumen, um Speicherlecks zu vermeiden
    this.sub.unsubscribe();
  }

  closeModal() {
    // Ruft die Methode im Service auf, um das Modal zu schließen
    this.modalService.close();
  }
}
```

**Detailierte Kommentare zu den Befehlen:**

- `this.sub = this.modalService.modal$.subscribe(...)`  
  Die Komponente abonniert das zentrale Observable aus dem Service.  
  Bei jeder Änderung des Modal-Status wird die lokale Variable `show`
  aktualisiert.
- `ngOnDestroy()`  
  Lebenszyklus-Hook, wird beim Entfernen der Komponente aufgerufen.  
  Hier wird die Subscription beendet, um Speicherlecks zu vermeiden.
- `closeModal()`  
  Methode, die das Modal über den Service schließt.  
  Die Komponente verändert den State nie direkt, sondern immer über den Service.

---

### 3. Template: Anzeige abhängig vom State

```html
<!-- modal.component.html -->
<div *ngIf="show" class="modal">
  <p>Modal-Inhalt</p>
  <button (click)="closeModal()">Schließen</button>
</div>
```

**Detailierte Kommentare zu den Befehlen:**

- `*ngIf="show"`  
  Das Modal wird nur angezeigt, wenn `show` true ist.  
  Die Anzeige ist direkt an den State aus dem Service gekoppelt.
- `(click)="closeModal()"`  
  Beim Klick auf den Button wird die Methode `closeModal()` ausgeführt, die das
  Modal über den Service schließt.

---

### 4. Modal von überall öffnen

```typescript
// irgendeine andere Komponente
constructor(private modalService: ModalService) {}

openModal() {
  // Öffnet das Modal über den Service
  this.modalService.showModal();
}
```

**Detailierte Kommentare zu den Befehlen:**

- `modalService.showModal()`  
  Setzt den Wert im Subject auf `true`, alle Subscriber (z.B. Modal-Komponente)
  reagieren darauf.  
  So kann das Modal von jeder beliebigen Komponente aus geöffnet werden, ohne
  direkte Verbindung zur Modal-Komponente.

---

## Zusammenfassung der wichtigsten Befehle

| Befehl                        | Bedeutung                                             |
| ----------------------------- | ----------------------------------------------------- |
| `new BehaviorSubject<T>(val)` | Erstellt ein Subject mit Typ T und Startwert val      |
| `.next(val)`                  | Sendet einen neuen Wert an alle Subscriber            |
| `.asObservable()`             | Gibt das Subject als Observable aus (nur lesend)      |
| `.subscribe(fn)`              | Reagiert auf jeden neuen Wert des Observables         |
| `.unsubscribe()`              | Beendet das Abo, verhindert Speicherlecks             |
| `*ngIf="..."`                 | Zeigt/verbirgt Elemente im Template abhängig vom Wert |
| `(click)="..."`               | Führt eine Methode beim Klick aus                     |

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
> Es ist ein universelles Werkzeug für jegliches zentrales, reaktives
> State-Management in Angular.  
> Typische Einsatzgebiete sind z.B. Ladeindikatoren, Authentifizierungsstatus,
> globale Benachrichtigungen, Feature-Toggles oder das Teilen von Daten zwischen
> Komponenten.  
> Das Modal ist hier nur ein einfaches Anwendungsbeispiel.
