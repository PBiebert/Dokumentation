[Back to Table of Contents](../README.md)

# Objekte in JavaScript

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

````js
let objectKeys = Object.keys(myObject);
let ourArray = [];

for (let i = 0; i < objectKeys.length; i++) {
  # Objects in JavaScript

  ## Creating Objects

  Objects in JavaScript are collections of key-value pairs. Keys are strings (or symbols), values can be any data type, including functions (methods) or other objects (nested objects).

  ```js
  // Create an object with key-value pairs
  let myObject = {
    name: "Philipp",
    age: 32,
    job: function () {
      console.log("Trainee");
    },
    good_guy: true,
  };
````

---

## Accessing Properties

You can access properties using dot or bracket notation.

```js
// Dot notation
console.log(myObject.name); // Output: Philipp

// Bracket notation
console.log(myObject["name"]); // Output: Philipp
```

---

## Displaying Objects

`console.table()` displays objects and arrays as a table in the console. This is
useful for a quick overview, but not ideal for nested objects.

```js
console.table(myObject);
```

---

## Calling Methods

A function inside an object is called a method. You can call it using dot
notation.

```js
myObject.job(); // Output: Trainee
```

---

## Getting Keys and Values

- `Object.keys(obj)` returns an array of all keys of the object.
- With a loop you can read all values.

```js
let objectKeys = Object.keys(myObject);
let ourArray = [];

for (let i = 0; i < objectKeys.length; i++) {
  ourArray.push(myObject[objectKeys[i]]);
}

console.log(ourArray); // Output: ["Philipp", 32, function, true]
```

---

## Getting Entries

`Object.entries(obj)` returns an array of key-value pairs as nested arrays.

```js
console.log(Object.entries(myObject));
/* Output:
  [ 'name', 'Philipp' ],
  [ 'age', 32 ],
  [ 'job', [Function: job] ],
  [ 'good_guy', true ]
*/
```

---

## Summary

- Objects store data as key-value pairs.
- Access properties with dot or bracket notation.
- Methods are functions inside objects.
- Use `Object.keys()` and `Object.entries()` to work with object data.
- `console.table()` is useful for displaying objects in the console.
