[Zurück zur Übersicht](../README.md)

# [ngModel] und @Output – Datenbindung und Event-Kommunikation

---

## Was ist ngModel?

Mit `ngModel` kannst du ein Eingabefeld direkt mit einer Variable in deiner
Komponente verbinden (Two-Way Data Binding). Änderungen im Input werden sofort
in der Variable gespeichert – und umgekehrt.

**Wichtig:**  
Für `ngModel` muss das `FormsModule` im Modul (bzw. bei Standalone-Komponenten
im `imports`-Array) importiert werden.

---

## Syntax: ngModel

```html
<input type="text" [(ngModel)]="inputData" />
```

```typescript
inputData = "";
```

- Die Variable `inputData` ist immer synchron mit dem Wert im Eingabefeld.

---

## Beispiel: Eingabe und Event mit @Output

Stell dir vor, du möchtest einen Kommentar zu einer Frucht eingeben und diesen
Wert an die Eltern-Komponente weitergeben.  
Dafür nutzt du `ngModel` für die Eingabe und `@Output` für die Kommunikation
nach außen.

### Kind-Komponente (`singlefruit.ts`)

```typescript
import { Component, Input, Output, EventEmitter } from "@angular/core";
import { FormsModule } from "@angular/forms";

@Component({
  selector: "app-singlefruit",
  imports: [FormsModule],
  templateUrl: "./singlefruit.html",
  styleUrl: "./singlefruit.scss",
})
export class Singlefruit {
  @Input() fruit: any;
  inputData = "";

  @Output() fruitname = new EventEmitter<string>();

  sendInputData() {
    this.fruitname.emit(this.inputData);
    // Optional: inputData zurücksetzen
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

---

## Eltern-Komponente (`fruitlist.html`)

Die Eltern-Komponente kann das Event abfangen und weiterverarbeiten:

```html
@for (singlefruit of fruitlist; track singlefruit.name; let index = $index){
<app-singlefruit
  (fruitname)="nameLog($event)"
  [fruit]="singlefruit"
></app-singlefruit>
}
```

```typescript
nameLog(name: string) {
  console.log(name); // Hier kommt der Wert aus der Kind-Komponente an
}
```

---

## Was passiert im Hintergrund?

- `ngModel` sorgt dafür, dass das Eingabefeld immer mit der Variable `inputData`
  synchron ist.
- Mit `@Output()` und `EventEmitter` kann die Kind-Komponente ein Event (mit
  Wert) an die Eltern-Komponente senden.
- Die Eltern-Komponente fängt das Event mit `(fruitname)="nameLog($event)"` ab
  und kann darauf reagieren.

---

## Typische Anwendungsfälle

- Formulareingaben an die Eltern-Komponente weitergeben (z.B. Kommentare,
  Bewertungen)
- Kommunikation von Kind- zu Eltern-Komponente (z.B. Button-Klicks, Änderungen)
- Dynamische Anzeige und Verarbeitung von Benutzereingaben

---

## Zusammenfassung

- `ngModel` verbindet Eingabefelder mit Variablen in der Komponente (Two-Way
  Binding).
- Mit `@Output` und `EventEmitter` sendest du Werte/Ereignisse von der Kind- zur
  Eltern-Komponente.
- So kannst du Benutzereingaben einfach aufnehmen und weiterverarbeiten – ohne
  komplexe Formlogik.

---
