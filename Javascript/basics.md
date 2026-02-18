[Zurück zum Inhaltsverzeichnis](../README.md)

# JavaScript Grundlagen

## Inhaltsverzeichnis

1. [Unterschied zwischen let und const](#unterschied-zwischen-let-und-const)
2. [Datentypen](#datentypen)
   - [String](#string)
   - [Number](#number)
   - [Boolean](#boolean)
   - [Array](#array)
   - [Object](#object)
3. [Zusammenfassung](#zusammenfassung)

## Unterschied zwischen let und const

Mit `let` deklarierst du eine Variable, die verändert werden kann. Mit `const`
deklarierst du eine Konstante, die nicht verändert werden kann.

```js
let age = 27;
console.log("Alter: ", age); // Ausgabe: 27

age = 32;
console.log("Neues Alter: ", age); // Ausgabe: 32

const birthYear = 1996;
console.log("Geburtsjahr: ", birthYear); // Ausgabe: 1996
```

---

## Datentypen

JavaScript kennt verschiedene Datentypen:

### String

**String** steht für Text.

```js
let myString = "Das ist ein Text";
```

### Number

**Number** steht für Zahlen (Ganzzahl oder Kommazahl).

```js
let myInt = 32; // Ganzzahl
let myFloat = 32.5466; // Kommazahl
```

### Boolean

**Boolean** steht für wahr oder falsch.

```js
let myBoolean = true;
```

### Array

**Array** ist eine Liste von Werten.

```js
let myArray = [2, 5, "Hallo"];
```

### Object

**Object** ist eine Sammlung von Schlüssel-Wert-Paaren.

```js
let myObject = { age: 32, height: 177 };
```

---

## Zusammenfassung

- Mit `let` kannst du Variablen ändern, mit `const` nicht.
- Die wichtigsten Datentypen sind: String, Number, Boolean, Array und Object.
- Arrays speichern Listen, Objekte speichern strukturierte Daten.
