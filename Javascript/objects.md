[Zurück zum Inhaltsverzeichnis](../README.md)

# Objekte in JavaScript

## Inhaltsverzeichnis

1. [Objekte erstellen](#objekte-erstellen)
2. [Zugriff auf Eigenschaften](#zugriff-auf-eigenschaften)
3. [Objekte anzeigen](#objekte-anzeigen)
4. [Methoden aufrufen](#methoden-aufrufen)
5. [Schlüssel und Werte erhalten](#schlüssel-und-werte-erhalten)
6. [Einträge erhalten](#einträge-erhalten)
7. [Zusammenfassung](#zusammenfassung)

## Objekte erstellen

Objekte in JavaScript sind Sammlungen von Schlüssel-Wert-Paaren. Schlüssel sind
Strings (oder Symbole), Werte können beliebige Datentypen sein, auch Funktionen
(Methoden) oder weitere Objekte (verschachtelte Objekte).

```js
// Erstelle ein Objekt mit Schlüssel-Wert-Paaren
let myObject = {
  name: "Philipp",
  age: 32,
  job: function () {
    console.log("Azubi");
  },
  good_guy: true,
};
```

---

## Zugriff auf Eigenschaften

Du kannst auf Eigenschaften mit Punkt- oder Klammer-Notation zugreifen.

```js
// Punkt-Notation
console.log(myObject.name); // Ausgabe: Philipp

// Klammer-Notation
console.log(myObject["name"]); // Ausgabe: Philipp
```

---

## Objekte anzeigen

`console.table()` zeigt Objekte und Arrays als Tabelle in der Konsole an. Das
ist praktisch für einen schnellen Überblick, aber nicht ideal für verschachtelte
Objekte.

```js
console.table(myObject);
```

---

## Methoden aufrufen

Eine Funktion im Objekt nennt man Methode. Du kannst sie mit der Punkt-Notation
aufrufen.

```js
myObject.job(); // Ausgabe: Azubi
```

---

## Schlüssel und Werte erhalten

- `Object.keys(obj)` gibt ein Array aller Schlüssel des Objekts zurück.
- Mit einer Schleife kannst du alle Werte auslesen.

```js
let objectKeys = Object.keys(myObject);
let ourArray = [];

for (let i = 0; i < objectKeys.length; i++) {
  ourArray.push(myObject[objectKeys[i]]);
}

console.log(ourArray); // Ausgabe: ["Philipp", 32, function, true]
```

---

## Einträge erhalten

`Object.entries(obj)` gibt ein Array von Schlüssel-Wert-Paaren als
verschachtelte Arrays zurück.

```js
console.log(Object.entries(myObject));
/* Ausgabe:
  [ 'name', 'Philipp' ],
  [ 'age', 32 ],
  [ 'job', [Function: job] ],
  [ 'good_guy', true ]
*/
```

---

## Zusammenfassung

- Objekte speichern Daten als Schlüssel-Wert-Paare.
- Greife mit Punkt- oder Klammer-Notation auf Eigenschaften zu.
- Methoden sind Funktionen innerhalb von Objekten.
- Mit `Object.keys()` und `Object.entries()` kannst du mit Objektdaten arbeiten.
- `console.table()` ist praktisch zum Anzeigen von Objekten in der Konsole.
