[Zurück zum Inhaltsverzeichnis](../README.md)

# Arrays und Schleifen in JavaScript

## Inhaltsverzeichnis

1. [Arrays – Grundlagen](#arrays--grundlagen)
2. [Array-Methoden](#array-methoden)
   - [.push()](#push)
   - [.pop()](#pop)
   - [.shift()](#shift)
   - [.unshift()](#unshift)
   - [.splice()](#splice)
3. [Suchen und Prüfen](#suchen-und-prüfen)
   - [.includes()](#includes)
   - [.indexOf()](#indexof)
   - [.find()](#find)
   - [.filter()](#filter)
   - [.findIndex()](#findindex)
4. [Kopieren und Transformieren von Arrays](#kopieren-und-transformieren-von-arrays)
   - [.slice()](#slice)
   - [.join()](#join)
   - [Spread Operator (...)](#spread-operator-)
5. [Schleifen – Grundlagen](#schleifen--grundlagen)
   - [for-Schleife](#for-schleife)
   - [for...of-Schleife](#forof-schleife)
   - [while-Schleife](#while-schleife)
   - [forEach()](#foreach)
6. [Schleifen – Beispiele](#schleifen--beispiele)
   - [Summe aller Elemente](#summe-aller-elemente)
   - [Zahlenreihe erzeugen](#zahlenreihe-erzeugen)
   - [Zahlenreihe rückwärts erzeugen](#zahlenreihe-rückwärts-erzeugen)
   - [Jedes dritte Element ausgeben](#jedes-dritte-element-ausgeben)
   - [Primzahl-Prüfung](#primzahl-prüfung)
7. [Schleifensteuerung](#schleifensteuerung)
8. [Zusammenfassung](#zusammenfassung)

## Arrays – Grundlagen

Arrays sind Listen von Werten. Die Indizierung beginnt bei 0.

```js
let myList = [12, "Banane", 1];
let fruits = ["Banane", "Orange", "Apfel", "Mango"];

console.log(myList[1]); // Zugriff auf ein Element
myList[1] = "keine Banane"; // Wert überschreiben
```

---

## Array-Methoden

### .push()

**.push()** fügt ein oder mehrere Elemente am Ende des Arrays hinzu und gibt die
neue Länge zurück.

```js
fruits.push("Kiwi");
```

### .pop()

**.pop()** entfernt das letzte Element aus dem Array und gibt es zurück.

```js
fruits.pop();
```

### .shift()

**.shift()** entfernt das erste Element aus dem Array und gibt es zurück.

```js
function removeFirstElement(array) {
  array.shift();
  return array;
}
```

### .unshift()

**.unshift()** fügt ein oder mehrere Elemente am Anfang des Arrays hinzu und
gibt die neue Länge zurück.

```js
function addElementToStart(array, element) {
  array.unshift(element);
  return array;
}
```

### .splice()

**.splice()** verändert das Array, indem Elemente entfernt, ersetzt oder
hinzugefügt werden.

```js
function modifyArray(array, start, deleteCount, ...items) {
  array.splice(start, deleteCount, ...items);
  return array;
}
```

---

## Suchen und Prüfen

### .includes()

**.includes()** prüft, ob ein Wert im Array enthalten ist (gibt true oder false
zurück).

```js
let arr = ["Anna", "Ben", "Clara"];
arr.includes("Ben"); // true
```

### .indexOf()

**.indexOf()** gibt den Index eines Wertes zurück (oder -1, wenn nicht
gefunden).

```js
arr.indexOf("Clara"); // 2
```

### .find()

**.find()** gibt das erste Element zurück, das eine Bedingung erfüllt (ansonsten
undefined).

```js
let people = [
  { name: "Anna", age: 25 },
  { name: "Ben", age: 30 },
  { name: "Clara", age: 28 },
];
let foundPerson = people.find((person) => person.age > 26);
```

### .filter()

**.filter()** gibt alle Elemente zurück, die eine Bedingung erfüllen.

```js
let onlyGoodGuys = people.filter((person) => person.age > 26);
```

### .findIndex()

**.findIndex()** gibt den Index des ersten passenden Elements zurück (oder -1).

```js
let index = people.findIndex((person) => person.name === "Ben");
```

---

## Kopieren und Transformieren von Arrays

### .slice()

**.slice()** erstellt eine Kopie eines Abschnitts des Arrays (von Start- bis
End-Index, End-Index exklusiv).

```js
let arr = [1, 2, 3, 4, 5];
let subArr = arr.slice(1, 4); // [2, 3, 4]
```

### .join()

**.join()** verbindet alle Elemente zu einem String, getrennt durch das
angegebene Zeichen.

```js
let joined = arr.join(" - "); // "1 - 2 - 3 - 4 - 5"
```

### Spread Operator (...)

**...** kopiert oder erweitert Arrays.

```js
let copy = [...arr];
```

---

## Schleifen – Grundlagen

### for-Schleife

**for-Schleife**: Wiederholt Code, solange eine Bedingung wahr ist.

```js
for (let i = 0; i < fruits.length; i++) {
  console.log(fruits[i]);
}
```

### for...of-Schleife

**for...of-Schleife**: Iteriert direkt über die Werte eines Arrays.

```js
for (const fruit of fruits) {
  console.log(fruit);
}
```

### while-Schleife

**while-Schleife**: Führt Code aus, solange eine Bedingung wahr ist.

```js
function whileLoop() {
  let i = 0;
  while (i < 5) {
    i++;
    console.log("while " + i);
  }
}
whileLoop();
```

### forEach()

**forEach()**: Führt eine Funktion für jedes Element im Array aus.

```js
fruits.forEach((element) => {
  console.log(element);
});
```

---

## Schleifen – Beispiele

### Summe aller Elemente

```js
function sumArray(array) {
  let sum = 0;
  for (let i = 0; i < array.length; i++) {
    sum += array[i];
  }
  return `${array.join(" + ")} = ${sum}`;
}
```

### Zahlenreihe erzeugen

```js
function printNumbers(num) {
  let numb = 0;
  let numbArray = [];
  for (let i = 0; i < num; i++) {
    numb += 1;
    numbArray.push(numb);
  }
  return numbArray.join(", ");
}
```

### Zahlenreihe rückwärts erzeugen

```js
function printNumbersReverse(num) {
  let numb = num + 1;
  let numbArray = [];
  for (let i = 0; i < num; i++) {
    numb -= 1;
    numbArray.push(numb);
  }
  return numbArray.join(", ");
}
```

### Jedes dritte Element ausgeben

```js
function printEveryThirdElement(array) {
  let counter = 3;
  let thirdElementArray = [];
  for (let i = 0; i < array.length; i++) {
    if (counter >= 3) {
      thirdElementArray.push(array[i]);
      counter = 1;
    } else {
      counter++;
    }
  }
  return thirdElementArray.join(", ");
}
```

### Primzahl-Prüfung

```js
function isPrime(num) {
  if (num < 2) return `${num} ist keine Primzahl`;
  for (let i = 2; i < num; i++) {
    if (num % i === 0) {
      return `${num} ist keine Primzahl`;
    }
  }
  return `${num} ist eine Primzahl`;
}
```

---

## Schleifensteuerung

**break** beendet die aktuelle Schleife sofort.  
**continue** überspringt die aktuelle Iteration und macht mit der nächsten
weiter.

```js
function sumArrayBreak(array) {
  let sum = 0;
  for (let i = 0; i < array.length; i++) {
    sum += array[i];
    if (array[i] == "error") {
      console.error(`error`);
      break;
    }
  }
  return sum;
}

function sumArrayContinue(array) {
  let sum = 0;
  for (let i = 0; i < array.length; i++) {
    if (array[i] == "error") {
      continue;
    }
    sum += array[i];
  }
  return sum;
}
```

---

## Zusammenfassung

- Arrays speichern Listen von Werten.
- Mit Methoden wie push, pop, shift, unshift, splice, slice, join, includes usw.
  kannst du Arrays flexibel bearbeiten.
- Schleifen helfen, wiederkehrende Aufgaben zu automatisieren.
- Mit break und continue steuerst du den Ablauf von Schleifen.
