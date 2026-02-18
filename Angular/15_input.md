[Zurück zur Übersicht](../README.md)

# @Input – Daten von außen an Komponenten übergeben

---

## Inhaltsverzeichnis

1. [Was ist @Input?](#was-ist-input)
2. [Wie funktioniert @Input?](#wie-funktioniert-input)
3. [Begriffe und Datenfluss](#begriffe-und-datenfluss)
4. [Syntax](#syntax)
5. [Beispiel aus dem Projekt](#beispiel-aus-dem-projekt)
6. [Was passiert im Hintergrund?](#was-passiert-im-hintergrund)
7. [Kann ich andere Namen verwenden?](#kann-ich-andere-namen-verwenden)
8. [Typische Anwendungsfälle](#typische-anwendungsfälle)
9. [Zusammenfassung](#zusammenfassung)

---

## Was ist @Input?

Mit dem Angular Decorator `@Input` kannst du Daten von einer Eltern-Komponente
an eine Kind-Komponente übergeben. Das ist die Standardmethode, um Komponenten
wiederverwendbar und flexibel zu machen.

**Wichtig:**  
`@Input` wird in der Kind-Komponente verwendet, um eine Eigenschaft für externe
Daten zu markieren. Die Eltern-Komponente kann dann einen Wert an diese
Eigenschaft binden.

---

## Wie funktioniert @Input?

Du hast eine Listen-Komponente, die viele Einträge anzeigt. Jeder Eintrag soll
als eigene Komponente (`Singlefruit`) dargestellt werden. Die Daten für jeden
Eintrag kommen aus der Eltern-Komponente (`Fruitlist`).  
Mit `@Input` kannst du jedem Eintrag (Kind-Komponente) die passenden Daten aus
der Eltern-Komponente übergeben.

---

## Begriffe und Datenfluss

- **Eltern-Komponente**: Die Komponente, die die Daten hält (z.B. ein Array mit
  allen Früchten) und die Kind-Komponente(n) im Template verwendet.
- **Kind-Komponente**: Die Komponente, die einzelne Datenobjekte (z.B. eine
  Frucht) als Input erhält und darstellt.
- **Property-Binding**: `[fruit]="singlefruit"` bedeutet: Die Eltern-Komponente
  übergibt der Kind-Komponente einen Wert (hier das Objekt `singlefruit`) und
  weist ihn der Eigenschaft `fruit` der Kind-Komponente zu.

**Wichtig:**  
Der Name links vom Gleichheitszeichen (`fruit`) ist der Name der Input-Property
in der Kind-Komponente.  
Der Wert rechts (`singlefruit`) ist eine Variable oder ein Ausdruck aus der
Eltern-Komponente.

Du kannst auch andere Namen verwenden, z.B. `[data]="item"` wenn die
Kind-Komponente `@Input() data` hat.

---

## Syntax

### In der Kind-Komponente

```typescript
import { Component, Input } from "@angular/core";

@Component({
  selector: "app-singlefruit",
  // ...existing code...
})
export class Singlefruit {
  @Input() fruit: any;
}
```

- Mit `@Input()` wird die Eigenschaft `fruit` für Daten von außen freigegeben.
- Die Kind-Komponente erwartet, dass die Eltern-Komponente einen Wert für
  `fruit` liefert.
- Die Property wird hier einfach als `fruit: any;` deklariert, ohne
  Initialisierung oder Ausrufezeichen. Du kannst den Typ nach Bedarf anpassen,
  z.B. `fruit: Fruit;` wenn du ein Interface verwendest.
- **Hinweis:**  
  Wenn du TypeScript mit strikter Typisierung verwendest (z.B.
  `"strictPropertyInitialization"` in `tsconfig.json` aktiviert), kann ein
  Fehler auftreten, weil die Property nicht direkt im Konstruktor initialisiert
  wird.  
  Um diesen Fehler zu vermeiden, kannst du ein Ausrufezeichen (`!`) verwenden:  
  `@Input() fruit!: Fruit;`  
  Das Ausrufezeichen sagt TypeScript, dass die Property später (durch Angular)
  gesetzt wird.

### In der Eltern-Komponente

```html
<app-singlefruit [fruit]="singlefruit"></app-singlefruit>
```

- `[fruit]` ist die Input-Property der Kind-Komponente.
- `"singlefruit"` ist eine Variable aus der Eltern-Komponente, die ein
  Frucht-Objekt enthält.

Du könntest auch `[fruit]="banana"` oder `[fruit]="apfel"` schreiben, wenn diese
Variablen im Eltern-Komponenten-Code existieren.

---

## Beispiel aus dem Projekt

### Eltern-Komponente (`fruitlist.html`)

Die Eltern-Komponente iteriert über ein Array von Früchten und gibt jede Frucht
an die Kind-Komponente weiter:

```html
@for(singlefruit of fruitlist; track singlefruit.name; let index = $index){
<app-singlefruit [fruit]="singlefruit"></app-singlefruit>
}
```

- `fruitlist` ist das Array mit allen Früchten.
- `singlefruit` ist die aktuelle Frucht im Schleifendurchlauf.
- `[fruit]="singlefruit"` übergibt das aktuelle Frucht-Objekt an die
  Kind-Komponente.

### Kind-Komponente (`singlefruit.ts`)

Die Kind-Komponente erhält das Frucht-Objekt als Input:

```typescript
import { Component, Input } from "@angular/core";

@Component({
  selector: "app-singlefruit",
  // ...existing code...
})
export class Singlefruit {
  @Input() fruit: any;
}
```

Jetzt kann die Kind-Komponente `fruit` im Template verwenden:

```html
<img src="./assets/img/fruits/{{ fruit.img }}" alt="" />
<h2>{{ fruit.name }}</h2>
<!-- usw. -->
```

---

## Was passiert im Hintergrund?

- Die Eltern-Komponente erstellt für jedes Element im Array eine Instanz der
  Kind-Komponente.
- Über das Property-Binding `[fruit]="singlefruit"` wird das jeweilige Objekt an
  die Kind-Komponente übergeben.
- Die Kind-Komponente kann mit `@Input()` auf diese Daten zugreifen und sie im
  Template anzeigen oder weiterverarbeiten.

---

## Kann ich andere Namen verwenden?

Ja!  
Du kannst die Input-Property in der Kind-Komponente beliebig nennen, z.B.
`@Input() data`.  
Dann musst du im Template der Eltern-Komponente `[data]="singlefruit"`
schreiben.

Beispiel:

**Kind-Komponente:**

```typescript
@Input() data: any;
```

**Eltern-Komponente:**

```html
<app-singlefruit [data]="singlefruit"></app-singlefruit>
```

Wichtig ist nur, dass der Name im Template (`[data]`) mit dem Namen der
Input-Property (`@Input() data`) übereinstimmt.

---

## Typische Anwendungsfälle

- Wiederverwendbare Komponenten (z.B. Karten, Listen-Elemente)
- Übergabe von Objekten, Strings, Zahlen, Arrays oder sogar Funktionen
- Dynamische Anzeige von Daten in Kind-Komponenten
- Kommunikation von Eltern- zu Kind-Komponente (aber nicht umgekehrt!)

---

## Zusammenfassung

- Mit `@Input` werden Daten von der Eltern- zur Kind-Komponente übergeben.
- Die Kind-Komponente markiert die Eigenschaft mit `@Input`.
- Die Eltern-Komponente übergibt den Wert per Property-Binding
  `[property]="wert"`.
- Die Namen können beliebig gewählt werden, müssen aber zusammenpassen.
- Änderungen am Wert in der Eltern-Komponente werden automatisch an die
  Kind-Komponente weitergegeben.
- Ideal für wiederverwendbare und flexible Komponenten.

---
