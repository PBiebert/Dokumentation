[Zurück zur Übersicht](../README.md)

# State-Management mit BehaviorSubject im Service (am Beispiel Dialog-Steuerung)

---

## Was ist ein BehaviorSubject?

Ein `BehaviorSubject` ist ein **RxJS-Subject**, das **immer den letzten Wert
speichert**  
und ihn **sofort an neue Subscriber** ausliefert.  
Damit eignet es sich ideal für UI-State wie „Dialog offen/geschlossen“.

---

## Vorteile von BehaviorSubject im Service

- **Zentraler State** (Single Source of Truth)
- **Lose Kopplung** zwischen Komponenten
- **Reaktiv** (RxJS verteilt Änderungen automatisch)
- **Sofortiger Startwert** für neue Subscriber

---

## Typische Anwendungsfälle

- Modals / Dialoge
- Ladeindikatoren
- Auth-Status
- Globale Benachrichtigungen

---

## Wichtigste RxJS-Begriffe

### `new BehaviorSubject<T>(val)`

Erstellt ein Subject mit Startwert.  
`T` = Typ, `val` = initialer State.  
**Warum wichtig?** Neue Subscriber bekommen sofort den aktuellen Wert.

### `.next(val)`

Schreibt einen neuen Wert in den Stream.  
**Nur der Service** soll den State ändern → klare Verantwortung.

### `.asObservable()`

Gibt das Subject als **read-only Observable** aus.  
Komponenten dürfen **nur lesen/abonnieren**.  
**Warum wichtig?** Schutz vor unkontrollierten State-Änderungen.

### `.subscribe(fn)`

Reagiert **imperativ** auf Änderungen.  
Nötig, wenn du Code ausführen musst (z.B. `showModal()`).

### `.unsubscribe()`

Beendet das Abo, wichtig bei **manuellen Subscriptions**.  
**Warum wichtig?** Verhindert Memory-Leaks.

---

## Zentrale Grundlage: Service steuert den State

Der Service ist die **zentrale Quelle** für den Dialog-State.  
Er stellt das Observable bereit und steuert Öffnen/Schließen über `next(...)`.  
Erst danach macht es Sinn, in Komponenten zu abonnieren oder `async` zu nutzen.

```typescript
// dialog.service.ts
import { Injectable } from "@angular/core";
import { BehaviorSubject, Observable } from "rxjs";

@Injectable({ providedIn: "root" })
export class DialogService {
  // Zentrale Quelle für den Dialog-State (Startwert: geschlossen)
  private dialogOpenSubject = new BehaviorSubject<boolean>(false);

  // Read-only Observable für Komponenten (nur lesen/abonnieren)
  dialogOpen$: Observable<boolean> = this.dialogOpenSubject.asObservable();

  // Öffnet den Dialog: sendet true an alle Subscriber
  openDialog() {
    this.dialogOpenSubject.next(true);
  }

  // Schließt den Dialog: sendet false an alle Subscriber
  closeDialog() {
    this.dialogOpenSubject.next(false);
  }
}
```

> **Hinweis:**  
> `asObservable()` kommt aus **RxJS** und verhindert direkte Änderungen von
> außen.

---

## Beispiel: Service mit BehaviorSubject (RxJS)

```typescript
// dialog.service.ts
import { Injectable } from "@angular/core";
import { BehaviorSubject, Observable } from "rxjs";

@Injectable({ providedIn: "root" })
export class DialogService {
  // Zentrale Quelle für den Dialog-State (Startwert: geschlossen)
  private dialogOpenSubject = new BehaviorSubject<boolean>(false);

  // Read-only Observable für Komponenten (nur lesen/abonnieren)
  dialogOpen$: Observable<boolean> = this.dialogOpenSubject.asObservable();

  // Öffnet den Dialog: sendet true an alle Subscriber
  openDialog() {
    this.dialogOpenSubject.next(true);
  }

  // Schließt den Dialog: sendet false an alle Subscriber
  closeDialog() {
    this.dialogOpenSubject.next(false);
  }
}
```

> **Hinweis:**  
> `asObservable()` kommt aus **RxJS** und sorgt dafür, dass Komponenten den
> State **nicht direkt verändern** können.

---

## Beispiel: Manuelles `subscribe` (imperative Logik)

```typescript
// dialog.component.ts
import { Component, OnDestroy, OnInit, inject } from "@angular/core";
import { Subscription } from "rxjs";
import { DialogService } from "./dialog.service";

@Component({
  /* ... */
})
export class DialogComponent implements OnInit, OnDestroy {
  private sub!: Subscription;
  private dialogService = inject(DialogService);

  ngOnInit() {
    this.sub = this.dialogService.dialogOpen$.subscribe((open) => {
      // Reaktion auf State-Änderung (imperative Logik)
      if (open) {
        // showModal()
      } else {
        // close()
      }
    });
  }

  ngOnDestroy() {
    this.sub?.unsubscribe();
  }
}
```

> **Wichtig:**  
> `unsubscribe()` verhindert Memory-Leaks bei manuellen Subscriptions.

---

## Variante B: Kein `subscribe` – Template mit `async` Pipe (Angular v17+ / v20)

**Wichtig:** In Angular v17+ (und auch v20) kannst du Observables im Template
direkt mit `async` nutzen, ohne manuelles `subscribe`.

```typescript
// dialog.component.ts
import { inject } from "@angular/core";
import { DialogService } from "./dialog.service";

export class DialogComponent {
  private dialogService = inject(DialogService);
  dialogOpen$ = this.dialogService.dialogOpen$;

  closeDialog() {
    this.dialogService.closeDialog();
  }
}
```

```html
<!-- dialog.component.html -->
<div *ngIf="dialogOpen$ | async" class="dialog">
  <p>Dialog-Inhalt</p>
  <button (click)="closeDialog()">Schließen</button>
</div>
```

> **Hinweis:**  
> Die `async` Pipe übernimmt **subscribe + unsubscribe automatisch**.  
> Empfohlen ab Angular v17+ (und in v20 Standard).

---

## Hinweise

- **Manuelles subscribe:** Nur nötig, wenn du imperative Logik im Code brauchst.
- **async Pipe:** Übernimmt subscribe + unsubscribe automatisch.

---

## Schritt-für-Schritt: So funktioniert der Flow

1. Service erstellt `BehaviorSubject` mit Startwert
2. Komponente **abonniert** oder Template nutzt `async`
3. Service ändert State via `next(true/false)`
4. UI reagiert automatisch

---

## Methodenübersicht

| Methode/Property  | Beschreibung                                    |
| ----------------- | ----------------------------------------------- |
| `BehaviorSubject` | RxJS-Quelle mit Startwert + letztem Wert        |
| `next(val)`       | Neuer Wert wird an alle Subscriber gesendet     |
| `asObservable()`  | Read-only Observable für Komponenten            |
| `subscribe()`     | Imperative Reaktion auf State-Änderungen        |
| `unsubscribe()`   | Abo beenden (nur bei manuellem subscribe nötig) |

---

## Fazit

- **RxJS BehaviorSubject** ist ideal für zentralen UI-State
- **Service steuert** den State über `next(...)`
- **Komponenten reagieren** via `subscribe` oder `async` Pipe
- Ab Angular v17+ (auch v20) ist `async` die sauberste Lösung
