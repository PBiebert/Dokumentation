[Zurück zum Inhaltsverzeichnis](../README.md)

# Funktionen in JavaScript

## Inhaltsverzeichnis

1. [Was ist eine Funktion?](#was-ist-eine-funktion)
2. [Beispiel: Funktion mit Parametern und Rückgabewert](#beispiel-funktion-mit-parametern-und-rückgabewert)
3. [Arrow Functions](#arrow-functions)
4. [Zusammenfassung](#zusammenfassung)

## Was ist eine Funktion?

Eine Funktion ist ein wiederverwendbarer Codeblock. Du kannst Eingaben
(Parameter) an die Funktion übergeben. Mit "return" kann die Funktion ein
Ergebnis zurückgeben.

---

## Beispiel: Funktion mit Parametern und Rückgabewert

```js
let tax = 1.19; // Mehrwertsteuer-Faktor

// Funktion aufrufen und Ergebnis speichern
let result = calculatePrice(50, 400, tax);

console.log(result); // Ausgabe: 416.5

// Funktionsdefinition
function calculatePrice(discount, price, tax) {
  // discount, price, tax sind Eingaben (Parameter)
  // return gibt das Ergebnis zurück
  return (price - discount) * tax;
}
```

---

## Arrow Functions

Arrow Functions sind eine kürzere Schreibweise für Funktionen. Sie verwenden die
`=>`-Syntax statt des Schlüsselworts `function`. Besonders praktisch für kurze,
einfache Funktionen.

**Syntax:** `(parameter) => { Codeblock }` oder `(parameter) => Ausdruck`

```js
// Traditionelle Funktion
function addTraditional(a, b) {
  return a + b;
}

// Arrow Function (lange Form)
let addArrow = (a, b) => {
  return a + b;
};

// Arrow Function (kurze Form) – gibt automatisch den Ausdruck zurück
let addShort = (a, b) => a + b;

// Arrow Function mit nur einem Parameter (Klammern optional)
let double = (x) => x * 2;

// Arrow Function ohne Parameter
let getRandomNumber = () => Math.random();

// Arrow Functions testen
console.log(addArrow(5, 3)); // Ausgabe: 8
console.log(addShort(10, 20)); // Ausgabe: 30
console.log(double(7)); // Ausgabe: 14
console.log(getRandomNumber()); // Ausgabe: Zufallszahl zwischen 0 und 1
```

---

## Zusammenfassung

- Funktionen helfen, Code wiederzuverwenden und zu strukturieren.
- Parameter machen Funktionen flexibel.
- Arrow Functions sind eine moderne, kompakte Alternative zur klassischen
  Funktionssyntax.
- Mit `return` gibst du Werte aus einer Funktion zurück.
