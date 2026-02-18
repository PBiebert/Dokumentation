[Zurück zum Inhaltsverzeichnis](../README.md)

# Zahlen und Strings in JavaScript

## Inhaltsverzeichnis

1. [Zahlen – Grundrechenarten](#zahlen--grundrechenarten)
2. [Strings](#strings)
3. [String-Methoden](#string-methoden)
   - [Länge und Leerzeichen](#länge-und-leerzeichen)
   - [Groß- und Kleinschreibung](#groß--und-kleinschreibung)
   - [Suchen in Strings](#suchen-in-strings)
   - [Text ersetzen](#text-ersetzen)
   - [Zahlen formatieren](#zahlen-formatieren)
   - [Strings aufteilen](#strings-aufteilen)
   - [Strings auffüllen](#strings-auffüllen)
4. [Zusammenfassung](#zusammenfassung)

## Zahlen – Grundrechenarten

```js
let myDivision = 10 / 5; // Division
console.log("Division:", myDivision); // Ausgabe: 2

let myMultiplication = 10 * 5; // Multiplikation
console.log("Multiplikation:", myMultiplication); // Ausgabe: 50

let myPower = 2 ** 3; // Potenzierung
console.log("Potenz:", myPower); // Ausgabe: 8

let mySubtraction = 10 - 5; // Subtraktion
console.log("Subtraktion:", mySubtraction); // Ausgabe: 5

let myAddition = 10 + 5; // Addition
console.log("Addition:", myAddition); // Ausgabe: 15
```

---

## Strings

```js
let myConcat = "Hallo " + "Welt"; // String-Verkettung
console.log("Verkettung:", myConcat); // Ausgabe: Hallo Welt

// Kombination von Strings und Zahlen
let myCombination1 = "5" + 5; // ergibt den String "55"
console.log("String + Zahl:", myCombination1);

let myCombination2 = 5 + "5"; // ergibt ebenfalls "55"
console.log("Zahl + String:", myCombination2);
```

---

## String-Methoden

### Länge und Leerzeichen

**.length** gibt die Länge des Strings zurück (inklusive Leerzeichen).  
**.trim()** entfernt Leerzeichen am Anfang und Ende des Strings.

```js
let myTestString = " Hallo  ";
console.log(myTestString.length); // Ausgabe: 8
console.log(myTestString.trim()); // Ausgabe: "Hallo"
```

### Groß- und Kleinschreibung

**.toUpperCase()** wandelt alle Zeichen in Großbuchstaben um.  
**.toLowerCase()** wandelt alle Zeichen in Kleinbuchstaben um.

```js
let fruit = "bAnanA";
console.log(fruit.toUpperCase()); // Ausgabe: "BANANA"
console.log(fruit.toLowerCase()); // Ausgabe: "banana"
```

### Suchen in Strings

**.includes()** prüft, ob ein Teilstring im String enthalten ist (gibt true oder
false zurück).

```js
console.log(myTestString.includes("Hall")); // Ausgabe: true
console.log(myTestString.includes("welt")); // Ausgabe: false
```

**.startsWith(suchText)** prüft, ob der String mit dem angegebenen Text beginnt.

```js
console.log("JavaScript".startsWith("Java")); // Ausgabe: true
console.log("JavaScript".startsWith("Script")); // Ausgabe: false
```

**.endsWith(suchText)** prüft, ob der String mit dem angegebenen Text endet.

```js
console.log("JavaScript".endsWith("Script")); // Ausgabe: true
console.log("JavaScript".endsWith("Java")); // Ausgabe: false
```

**.indexOf(suchText)** gibt den Index des ersten Vorkommens von suchText zurück
oder -1, wenn nicht gefunden.

```js
console.log("Banane".indexOf("a")); // Ausgabe: 1
console.log("Banane".indexOf("x")); // Ausgabe: -1
```

**.lastIndexOf(suchText)** gibt den Index des letzten Vorkommens von suchText
zurück oder -1, wenn nicht gefunden.

```js
console.log("Banane".lastIndexOf("a")); // Ausgabe: 5
console.log("Banane".lastIndexOf("x")); // Ausgabe: -1
```

### Text ersetzen

**.replace(suchWert, neuerWert)** ersetzt das erste Vorkommen von suchWert durch
neuerWert (ändert nicht das Original, gibt einen neuen String zurück).

```js
let greeting = "Hallo Welt";
console.log(greeting.replace("Welt", "JavaScript")); // Ausgabe: "Hallo JavaScript"

let sentence = "Ich liebe Äpfel";
let newSentence = sentence.replace("Äpfel", "Bananen");
console.log(newSentence); // Ausgabe: "Ich liebe Bananen"
```

### Zahlen formatieren

**.toFixed(anzahlStellen)** rundet eine Zahl und gibt sie als String mit fester
Nachkommastellenanzahl zurück.  
Mit **.replace()** kannst du z.B. den Punkt durch ein Komma ersetzen.

```js
let myNumber = 10.2560876;
console.log(myNumber.toFixed(2)); // Ausgabe: "10.26"
console.log(myNumber.toFixed(0)); // Ausgabe: "10"
console.log(myNumber.toFixed(4)); // Ausgabe: "10.2560"

let price = 10.25;
let formattedPrice = price.toString().replace(".", ",");
console.log(formattedPrice); // Ausgabe: "10,25"
```

### Strings aufteilen

**.split(trennzeichen)** teilt einen String anhand des Trennzeichens in ein
Array.

```js
let fruits = "Apfel,Banane,Kirsche";
console.log(fruits.split(",")); // Ausgabe: ["Apfel", "Banane", "Kirsche"]
```

### Strings auffüllen

**.padStart(zielLänge, auffüllString)** füllt den String am Anfang auf die
gewünschte Länge auf.

```js
let code = "7";
console.log(code.padStart(3, "0")); // Ausgabe: "007"
```

**.padEnd(zielLänge, auffüllString)** füllt den String am Ende auf die
gewünschte Länge auf.

```js
let code = "7";
console.log(code.padEnd(3, "0")); // Ausgabe: "700"
```

---

## Zusammenfassung

- Mit Grundrechenarten und String-Operationen kannst du viele Aufgaben in
  JavaScript lösen.
- String-Methoden wie `.trim()`, `.toUpperCase()`, `.includes()` oder
  `.replace()` sind sehr nützlich für die Textverarbeitung.
- Zahlen können mit `.toFixed()` formatiert werden.
