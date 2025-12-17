````markdown
[Zurück zur Übersicht](../README.md)

# @Output – Events und Daten von Kind- zu Eltern-Komponente senden

---

## Was ist @Output?

Mit dem Angular Decorator `@Output` kannst du aus einer Kind-Komponente ein
Event (mit oder ohne Wert) an die Eltern-Komponente senden.  
Das ist die Standardmethode, um auf Aktionen oder Änderungen in einer
Kind-Komponente in der Eltern-Komponente zu reagieren.

**Wichtig:**  
`@Output` wird in der Kind-Komponente verwendet, um ein Event zu deklarieren.  
Die Eltern-Komponente kann dieses Event im Template abfangen und darauf
reagieren.

---

## Wie funktioniert @Output?

Du hast eine Kind-Komponente, in der z.B. ein Button geklickt oder ein Wert
eingegeben wird.  
Mit `@Output` und einem `EventEmitter` kannst du ein Event auslösen und einen
Wert an die Eltern-Komponente schicken.

---

## Begriffe und Datenfluss

- **Kind-Komponente**: Sendet ein Event nach außen (z.B. bei Klick, Eingabe,
  Änderung).
- **Eltern-Komponente**: Fängt das Event im Template ab und kann darauf
  reagieren.
- **EventEmitter**: Ein spezielles Angular-Objekt, das Events auslöst.

---

## Syntax

### In der Kind-Komponente

```typescript
import { Component, Output, EventEmitter } from "@angular/core";

@Component({
  selector: "app-singlefruit",
  // ...existing code...
})
export class Singlefruit {
  @Output() fruitname = new EventEmitter<string>();

  sendInputData() {
    this.fruitname.emit("Beispielwert");
  }
}
```
````

- Mit `@Output()` wird die Eigenschaft `fruitname` als Event deklariert.
- Mit `this.fruitname.emit(wert)` wird das Event ausgelöst und ein Wert
  gesendet.

### In der Eltern-Komponente

```html
<app-singlefruit (fruitname)="nameLog($event)"></app-singlefruit>
```

- Mit `(fruitname)="nameLog($event)"` wird das Event abgefangen.
- `$event` enthält den Wert, der von der Kind-Komponente gesendet wurde.

---

## Beispiel aus dem Projekt

### Kind-Komponente (`singlefruit.ts`)

```typescript
import { Component, Output, EventEmitter } from "@angular/core";

@Component({
  selector: "app-singlefruit",
  // ...existing code...
})
export class Singlefruit {
  @Output() fruitname = new EventEmitter<string>();
  inputData = "";

  sendInputData() {
    this.fruitname.emit(this.inputData);
    this.inputData = "";
  }
}
```

### Kind-Template (`singlefruit.html`)

```html
<input type="text" [(ngModel)]="inputData" />
<button>
  <img (click)="sendInputData()" src="./assets/img/arrow-35-32.png" alt="" />
</button>
```

### Eltern-Komponente (`fruitlist.html`)

```html
@for (singlefruit of fruitlist; track singlefruit.name; let index = $index){
<app-singlefruit
  (fruitname)="nameLog($event)"
  [fruit]="singlefruit"
></app-singlefruit>
}
```

### Eltern-Komponente (`fruitlist.ts`)

```typescript
nameLog(name: string) {
  console.log(name); // Wert aus der Kind-Komponente
}
```

---

## Was passiert im Hintergrund?

- Die Kind-Komponente löst mit `this.fruitname.emit(wert)` ein Event aus.
- Die Eltern-Komponente fängt das Event mit `(fruitname)="nameLog($event)"` ab.
- Der Wert wird als `$event` an die Methode der Eltern-Komponente übergeben.

---

## Eigene Namen und Typen für Events

Du kannst den Namen des Events und den Typ des Wertes beliebig wählen, z.B.  
`@Output() myEvent = new EventEmitter<number>();`  
und dann `(myEvent)="handleNumber($event)"`.

---

## Typische Anwendungsfälle

- Benutzereingaben oder Aktionen an die Eltern-Komponente melden
- Kommunikation von Kind- zu Eltern-Komponente (z.B. Button-Klicks, Änderungen)
- Dynamische Verarbeitung von Daten und Events

---

## Zusammenfassung

- Mit `@Output` und `EventEmitter` sendest du Events und Werte von der Kind- zur
  Eltern-Komponente.
- Die Eltern-Komponente kann das Event im Template abonnieren und darauf
  reagieren.
- Ideal für die Kommunikation von unten nach oben in der Komponenten-Hierarchie.
