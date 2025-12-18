````markdown
[Zurück zur Übersicht](../README.md)

# @Output – Events und Daten von Kind- zu Eltern-Komponente senden

## Was ist @Output?

Mit dem Angular Decorator `@Output` kannst du aus einer Kind-Komponente ein Event (mit oder ohne Wert) an die Eltern-Komponente senden.  
Das ist die Standardmethode, um auf Aktionen oder Änderungen in einer Kind-Komponente in der Eltern-Komponente zu reagieren.

## Vorteile von @Output

- Ermöglicht Kommunikation von Kind- zu Eltern-Komponente
- Eltern-Komponente kann auf Aktionen in der Kind-Komponente reagieren
- Sauberer und klarer Datenfluss nach oben

## Funktionsweise

- Die Kind-Komponente deklariert ein Event mit `@Output` und `EventEmitter`.
- Die Eltern-Komponente fängt das Event im Template ab und kann darauf reagieren.

## Syntax

### In der Kind-Komponente

```typescript
import { Component, Output, EventEmitter } from '@angular/core';

@Component({
  selector: 'app-singlefruit',
  // ...existing code...
})
export class Singlefruit {
  @Output() fruitname = new EventEmitter<string>();
  inputData = '';

  sendInputData() {
    this.fruitname.emit(this.inputData);
    this.inputData = '';
  }
}
```

### In der Eltern-Komponente (Template)

```html
<app-singlefruit
  (fruitname)="addComment($event, index)"
  [fruit]="singlefruit"
></app-singlefruit>
```

- `(fruitname)="addComment($event, index)"`: Das Event wird abgefangen, `$event` enthält den Wert aus der Kind-Komponente.

### In der Eltern-Komponente (Klasse)

```typescript
addComment(comment: string, index: number) {
  this.fruitlist[index].reviews.push({
    name: 'Philipp B.',
    text: comment,
  });
}
```

## Beispiel

**Kind-Komponente (`singlefruit.ts`):**

```typescript
@Output() fruitname = new EventEmitter<string>();
inputData = '';

sendInputData() {
  this.fruitname.emit(this.inputData);
  this.inputData = '';
}
```

**Kind-Template (`singlefruit.html`):**

```html
<input type="text" [(ngModel)]="inputData" />
<button>
  <img (click)="sendInputData()" src="./assets/img/arrow-35-32.png" alt="" />
</button>
```

**Eltern-Komponente (`fruitlist.html`):**

```html
@for (singlefruit of fruitlist; track singlefruit.name; let index = $index){
  <app-singlefruit
    (fruitname)="addComment($event, index)"
    [fruit]="singlefruit"
  ></app-singlefruit>
}
```

**Eltern-Komponente (`fruitlist.ts`):**

```typescript
addComment(comment: string, index: number) {
  this.fruitlist[index].reviews.push({
    name: 'Philipp B.',
    text: comment,
  });
}
```

## Fazit

Mit `@Output` und `EventEmitter` kannst du Events und Daten gezielt von Kind- zu Eltern-Komponenten senden. Das sorgt für einen klaren, nachvollziehbaren Datenfluss in Angular-Anwendungen.
````
