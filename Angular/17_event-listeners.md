[Zurück zur Übersicht](../README.md)

# Event Listener – Benutzerinteraktionen abfangen

---

## Inhaltsverzeichnis

1. [Syntax](#syntax)
2. [Häufige Events](#häufige-events)
3. [Beispiel: Klick-Event](#beispiel-klick-event)
4. [Beispiel: Übergabe von Werten](#beispiel-übergabe-von-werten)
5. [Beispiel aus dem Projekt](#beispiel-aus-dem-projekt)
6. [Event-Objekt verwenden](#event-objekt-verwenden)
7. [Zusammenfassung](#zusammenfassung)

---

## Syntax

```html
<button (click)="onClickHandler()">Klick mich!</button>
```

- `(click)` ist das Event, das abgefangen wird.
- `"onClickHandler()"` ist die Methode, die bei diesem Event in der Komponente
  aufgerufen wird.

---

## Häufige Events

- `(click)` – Mausklick
- `(dblclick)` – Doppelklick
- `(input)` – Eingabe in ein Feld
- `(change)` – Änderung eines Wertes (z.B. Checkbox)
- `(keyup)` / `(keydown)` – Tastendruck
- `(keypress)` – Taste wird gedrückt (während gedrückt gehalten)
- `(focus)` / `(blur)` – Fokus auf ein Eingabefeld / Fokusverlust
- `(mouseenter)` / `(mouseleave)` – Maus betritt/verlässt ein Element
- `(mouseover)` / `(mouseout)` – Maus bewegt sich über/weg von einem Element
- `(submit)` – Formular absenden
- `(scroll)` – Scrollen eines Elements
- `(contextmenu)` – Rechtsklick (Kontextmenü)
- `(drag)` / `(drop)` – Drag & Drop Aktionen

---

## Beispiel: Klick-Event

### Komponente (TypeScript)

```typescript
export class MyComponent {
  onButtonClick() {
    console.log("Button wurde geklickt!");
  }
}
```

### Template (HTML)

```html
<button (click)="onButtonClick()">Klick mich!</button>
```

---

## Beispiel: Übergabe von Werten

Du kannst dem Event-Handler auch Werte übergeben:

```html
<button (click)="onFruitSelect(fruit)">Auswählen</button>
```

```typescript
onFruitSelect(fruit: any) {
  console.log('Ausgewählte Frucht:', fruit);
}
```

---

## Beispiel aus dem Projekt

```html
@for (singlefruit of fruitlist; track singlefruit.name; let index = $index){
<app-singlefruit
  (click)="numLog(index)"
  [fruit]="singlefruit"
></app-singlefruit>
}
```

```typescript
numLog(index: number) {
  console.log(index);
}
```

---

## Event-Objekt verwenden

Du kannst das native Event-Objekt abgreifen:

```html
<input (input)="onInputChange($event)" />
```

```typescript
onInputChange(event: Event) {
  const value = (event.target as HTMLInputElement).value;
  console.log('Eingegebener Wert:', value);
}
```

---

## Zusammenfassung

- Mit `(eventName)="handler()"` reagierst du auf Benutzeraktionen im Template.
- Die Methode wird in der zugehörigen Komponente definiert.
- Du kannst Werte und das Event-Objekt übergeben.
- Typische Events sind Klick, Eingabe, Änderung, Tastendruck usw.

---
