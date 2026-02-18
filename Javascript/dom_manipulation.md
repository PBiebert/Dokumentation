[Zurück zum Inhaltsverzeichnis](../README.md)

# DOM-Manipulation in JavaScript

## Inhaltsverzeichnis

1. [Was ist das DOM?](#was-ist-das-dom)
2. [Selektoren](#selektoren)
3. [Element-Inhalt ändern](#element-inhalt-ändern)
4. [Klassen bearbeiten](#klassen-bearbeiten)
5. [Attribute bearbeiten](#attribute-bearbeiten)
6. [Werte von Form-Elementen](#werte-von-form-elementen)
7. [Events und EventListener](#events-und-eventlistener)
   - [Click-Event](#click-event)
   - [Maus-Events](#maus-events)
   - [Tastatur-Events](#tastatur-events)
   - [Focus-Events bei Inputs](#focus-events-bei-inputs)
   - [Form-Event](#form-event)
8. [Eigene Hilfsfunktionen](#eigene-hilfsfunktionen)
9. [Zusammenfassung](#zusammenfassung)

## Was ist das DOM?

Das DOM (Document Object Model) ist die Schnittstelle, mit der du HTML-Elemente
mit JavaScript auslesen, verändern und auf sie reagieren kannst.

---

## Selektoren

Mit Selektoren kannst du Elemente im HTML auswählen:

```js
// ID-Selektor
let title = document.getElementById("websiteTitel");

// Klassen-Selektor
let boxes = document.getElementsByClassName("box");

// Tag-Selektor
let paragraphs = document.getElementsByTagName("p");

// CSS-Selektoren
let firstDiv = document.querySelector("#testDiv"); // Erstes Element mit passendem Selektor
let allDivs = document.querySelectorAll("div"); // Alle <div>-Elemente
let firstBox = document.querySelector(".box"); // Erstes Element mit Klasse 'box'
```

---

## Element-Inhalt ändern

**.innerHTML** liest oder setzt den HTML-Inhalt eines Elements.

**.innerText** liest oder setzt den sichtbaren Text (ohne HTML-Tags).

```js
console.log(title.innerHTML); // Aktueller HTML-Inhalt
title.innerHTML = "<p>test</p>"; // Neuen HTML-Inhalt setzen
title.innerHTML = "neuer Text"; // Nur Text setzen
document.getElementById("testDiv").innerText = "test"; // Sichtbaren Text setzen
```

---

## Klassen bearbeiten

Mit **.classList** kannst du Klassen hinzufügen, entfernen, toggeln oder prüfen:

```js
document.getElementById("testDiv").classList.add("green_bg"); // Hinzufügen
document.getElementById("testDiv").classList.remove("green_bg"); // Entfernen
document.getElementById("testDiv").classList.toggle("green_bg"); // Umschalten
console.log(document.getElementById("testDiv").classList.contains("green_bg")); // Prüfen

document.getElementById("testDiv").classList.replace("green_bg", "red_bg"); // Ersetzen
```

---

## Attribute bearbeiten

Mit **.setAttribute()**, **.getAttribute()** und **.removeAttribute()** kannst
du Attribute setzen, auslesen oder entfernen:

```js
document.getElementById("testInput").setAttribute("value", 15);
console.log(document.getElementById("testInput").getAttribute("value"));
document.getElementById("testInput").removeAttribute("value");
```

---

## Werte von Form-Elementen

Mit **.value** liest oder setzt du den Wert eines Input-Felds:

```js
document.getElementById("testInput").value = 20;
console.log(document.getElementById("testInput").value);
```

---

## Events und EventListener

Mit **addEventListener** kannst du auf verschiedene Ereignisse im DOM reagieren.
Hier einige wichtige Events:

### Click-Event

**"click"** wird ausgelöst, wenn ein Element angeklickt wird.

```js
let button = document.getElementById("myButton");
button.addEventListener("click", () => {
  alert("Button geklickt!");
});
```

### Maus-Events

**"mouseover"** wird ausgelöst, wenn die Maus über ein Element fährt.  
**"mouseout"** wird ausgelöst, wenn die Maus das Element verlässt.  
**"dblclick"** wird beim Doppelklick ausgelöst.

```js
button.addEventListener("mouseover", () => {
  console.log("Maus über dem Button");
});
button.addEventListener("mouseout", () => {
  console.log("Maus hat den Button verlassen");
});
button.addEventListener("dblclick", () => {
  console.log("Button doppelt geklickt");
});
```

### Tastatur-Events

**"keydown"** wird ausgelöst, wenn eine Taste gedrückt wird.  
**"keyup"** wird ausgelöst, wenn eine Taste losgelassen wird.

```js
document.addEventListener("keydown", (event) => {
  console.log("Taste gedrückt:", event.key);
});
document.addEventListener("keyup", (event) => {
  console.log("Taste losgelassen:", event.key);
});
```

### Focus-Events bei Inputs

**"focus"** wird ausgelöst, wenn ein Input-Feld den Fokus bekommt.  
**"blur"** wird ausgelöst, wenn das Input-Feld den Fokus verliert.  
**"input"** wird ausgelöst, wenn sich der Wert eines Input-Felds ändert.

```js
let input = document.getElementById("testInput");
input.addEventListener("focus", () => {
  console.log("Input im Fokus");
});
input.addEventListener("blur", () => {
  console.log("Input nicht mehr im Fokus");
});
input.addEventListener("input", (event) => {
  console.log("Aktueller Wert:", event.target.value);
});
```

### Form-Event

**"submit"** wird ausgelöst, wenn ein Formular abgeschickt wird.

```js
let form = document.getElementById("myForm");
form.addEventListener("submit", (event) => {
  event.preventDefault(); // Verhindert das Neuladen der Seite
  console.log("Formular abgeschickt!");
});
```

---

## Eigene Hilfsfunktionen

Du kannst eigene Funktionen schreiben, um DOM-Elemente zu manipulieren:

```js
// Sichtbarkeit eines Elements toggeln durch Umschalten der Klasse "dNone" (display:none)
function toggleDNone(id) {
  document.getElementById(id).classList.toggle("dNone");
}
```

---

## Zusammenfassung

- Mit DOM-Methoden kannst du gezielt HTML-Elemente auswählen und verändern.
- Mit Event-Listenern reagierst du auf Benutzeraktionen.
- Mit eigenen Funktionen kannst du wiederkehrende Aufgaben im DOM einfach lösen.
