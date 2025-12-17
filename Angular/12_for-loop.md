# Der `@for`-Loop in Angular

Mit der modernen `@for`-Syntax kannst du in Angular Listen im Template
durchlaufen und anzeigen. Sie ist eine Weiterentwicklung des klassischen
`*ngFor` und bietet eine klarere Syntax, bessere Performance und neue Features
wie verschachtelte Loops und die `@empty`-Klausel.

## Praxisbeispiel: Die Fruitlist-Komponente

In der Datei `fruitlist.ts` ist eine Liste von Früchten definiert, die jeweils
Bewertungen enthalten:

```typescript
// fruitlist.ts
export class Fruitlist {
  fruitlist = [
    {
      name: "Apfel",
      img: "apple.png",
      // ...weitere Eigenschaften...
      reviews: [
        { name: "Kevin W.", text: "ist lecker" },
        { name: "Arne P.", text: "nicht so meins" },
      ],
    },
    // ...weitere Früchte...
  ];
}
```

Im Template (`fruitlist.html`) wird diese Liste mit `@for` durchlaufen:

```html
<div class="fruitlist_content">
  @for(item of fruitlist; track item.name){
  <section class="singleFruitBox">
    <h2>{{ item.name }}</h2>
    <!-- ...weitere Felder... -->
    <div>
      <div>
        @for(review of item.reviews; track review){
        <div><b>{{ review.name }}:</b><br />{{ review.text }}</div>
        } @empty {
        <b>no comments</b>
        }
      </div>
    </div>
  </section>
  }
</div>
```

## Erklärung der Syntax

- `@for(item of fruitlist; track item.name){ ... }`: Durchläuft jedes Element
  (`item`) in der Liste `fruitlist`.
- `track item.name`: Gibt Angular einen eindeutigen Schlüssel für jedes Element,
  was die Performance bei Änderungen verbessert.
- Verschachtelte Loops: Du kannst innerhalb eines Loops einen weiteren
  `@for`-Loop verwenden, z.B. um für jede Frucht die Bewertungen (`reviews`)
  anzuzeigen.
- `@empty`: Wird ausgeführt, wenn die Liste leer ist. Im Beispiel erscheint dann
  „no comments“, falls keine Bewertungen vorhanden sind.

## Verschachtelte Loops

Das Verschachteln von `@for`-Loops ist besonders nützlich, wenn du mit
verschachtelten Datenstrukturen arbeitest, wie z.B. einer Liste von Früchten,
die jeweils eine Liste von Bewertungen enthalten. So kannst du für jedes
Hauptelement (Frucht) die zugehörigen Unterelemente (Bewertungen) anzeigen.

### Erklärung: `@for(review of item.reviews; track review){ ... }`

Mit dieser Syntax durchläufst du für jedes einzelne Frucht-Objekt (`item`) die
zugehörige Liste von Bewertungen (`reviews`).

- `review of item.reviews` bedeutet: Für das aktuelle Frucht-Objekt (`item`)
  wird die Eigenschaft `reviews` (also die Liste der Bewertungen) durchlaufen.
- `review` ist dabei die Variable, die in jedem Schleifendurchlauf das aktuelle
  Bewertungselement enthält.

**Beispiel:**  
Wenn `item.name` `"Apfel"` ist, dann ist `item.reviews` die Liste aller
Bewertungen für den Apfel.  
Mit `@for(review of item.reviews; track review){ ... }` wird jede Bewertung für
den Apfel einzeln durchlaufen und angezeigt.

Das ist notwendig, weil jede Frucht ihre eigenen Bewertungen hat.  
Du schreibst also:

- `item of fruitlist` für die äußere Liste (alle Früchte)
- `review of item.reviews` für die innere Liste (alle Bewertungen der aktuellen
  Frucht)

So erreichst du, dass zu jeder Frucht die passenden Bewertungen angezeigt
werden.

## Die `@empty`-Klausel

Mit `@empty` kannst du eine Alternative anzeigen, falls die durchlaufene Liste
leer ist. Das ist besonders praktisch für User-Feedback, z.B. wenn noch keine
Bewertungen vorhanden sind.

Beispiel:

```html
@for(review of item.reviews; track review){
<div>{{ review.text }}</div>
} @empty {
<b>no comments</b>
}
```

## Weitere Möglichkeiten

- **Lokale Variablen:** Mit `let index = $index` kannst du den Index des
  aktuellen Elements nutzen.
- **Mehrere lokale Variablen:** Du kannst auch `$first`, `$last`, `$even`,
  `$odd` verwenden.
- **Tracking:** Mit `track` kannst du Angular mitteilen, wie die Elemente
  eindeutig identifiziert werden sollen.

## Vorteile des `@for`-Loops

- **Klarere Syntax** als `*ngFor`
- **Verschachtelung** und **@empty**-Klausel für bessere Kontrolle
- **Performance** durch gezieltes Tracking

## Fazit

Mit dem `@for`-Loop kannst du in Angular Listen und verschachtelte Strukturen
übersichtlich und performant im Template darstellen. Die `@empty`-Klausel sorgt
für eine elegante Behandlung leerer Listen.
