[Zurück zum Inhaltsverzeichnis](../README.md)

# Bedingungen und Vergleichsoperatoren

## Inhaltsverzeichnis

1. [Was sind Bedingungen?](#was-sind-bedingungen)
2. [Logische Operatoren](#logische-operatoren)
   - [NOT Operator (!)](#not-operator-)
   - [ODER Operator (||)](#oder-operator-)
   - [UND Operator (&&)](#und-operator-)
3. [Vergleichsoperatoren](#vergleichsoperatoren)
   - [Gleichheit (== und ===)](#gleichheit--und-)
   - [Größer/Kleiner als](#größerkleiner-als)
   - [Ungleich (!= und !==)](#ungleich--und-)
4. [Ternary Operator (Kurz-If)](#ternary-operator-kurz-if)
5. [if-else Bedingung](#if-else-bedingung)
6. [switch/case](#switchcase)
7. [Zusammenfassung](#zusammenfassung)

## Was sind Bedingungen?

Bedingungen steuern, ob ein bestimmter Codeblock ausgeführt wird oder nicht.
Dafür nutzt man Vergleichs- und logische Operatoren.

---

## Logische Operatoren

### NOT Operator (!)

**!** kehrt den booleschen Wert um (true wird false, false wird true).

```js
let myCondition = true;
myCondition = !myCondition; // Ergebnis: false
```

### ODER Operator (||)

**||** ergibt true, wenn mindestens ein Wert true ist.

```js
myCondition = true || false || true; // Ergebnis: true
```

### UND Operator (&&)

**&&** ergibt nur dann true, wenn beide Werte true sind.

```js
myCondition = true && false; // Ergebnis: false
```

---

## Vergleichsoperatoren

### Gleichheit (== und ===)

**==** prüft, ob zwei Werte gleich sind (nicht strikt, Typen können
unterschiedlich sein).  
**===** prüft, ob zwei Werte gleich und vom gleichen Typ sind (strikt).

```js
myCondition = 45 == 45; // true
myCondition = 45 == "45"; // true
myCondition = 45 === 45; // true
myCondition = 45 === "45"; // false
```

### Größer/Kleiner als

**>** prüft, ob der linke Wert größer ist.  
**<** prüft, ob der linke Wert kleiner ist.  
**>=** prüft, ob der linke Wert größer oder gleich ist.  
**<=** prüft, ob der linke Wert kleiner oder gleich ist.

```js
myCondition = 45 > 123; // false
myCondition = 45 < 123; // true
myCondition = 45 >= 123; // false
myCondition = 45 <= 123; // true
```

### Ungleich (!= und !==)

**!=** prüft, ob zwei Werte ungleich sind (nicht strikt).  
**!==** prüft, ob zwei Werte ungleich oder vom unterschiedlichen Typ sind
(strikt).

```js
myCondition = 45 != "47"; // true
myCondition = 45 !== 47; // true
myCondition = 45 !== "45"; // true
```

---

## Ternary Operator (Kurz-If)

Mit dem ternären Operator kann man eine Bedingung in einer Zeile schreiben:

```js
let lang = "de";
let myTitle = lang == "de" ? "Webseite" : "Website"; // Ergebnis: "Webseite"
```

---

## if-else Bedingung

Mit if-else kann man verschiedene Fälle prüfen:

```js
let myIfCondition = false;
let mySecondIfCondition = false;

if (myIfCondition) {
  console.log("Erste Bedingung ist wahr");
} else if (mySecondIfCondition) {
  console.log("Zweite Bedingung ist wahr");
} else {
  console.log("Keine Bedingung ist wahr -> else-Block wird ausgeführt");
}
```

---

## switch/case

Mit switch/case kann man einen Wert gegen mehrere mögliche Fälle prüfen:

```js
function colorCheck(color) {
  let colorCode;
  switch (color) {
    case "red":
      colorCode = "#ba190eff";
      break;
    case "green":
      colorCode = "#0eba1eff";
      break;
    case "blue":
      colorCode = "#1e0eba";
      break;
    default:
      colorCode = "#210eba";
      break;
  }
  return colorCode;
}
```

---

## Zusammenfassung

- Bedingungen steuern den Programmfluss.
- Logische Operatoren (&&, ||, !) und Vergleichsoperatoren (==, ===, >, <, ...)
  sind die Basis für Bedingungen.
- Mit if-else und switch/case prüfst du verschiedene Fälle.
- Der ternäre Operator ist eine Kurzform für einfache Bedingungen.
