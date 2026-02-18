[Zurück zum Inhaltsverzeichnis](../README.md)

# Local Storage in JavaScript

## Inhaltsverzeichnis

1. [Was ist Local Storage?](#was-ist-local-storage)
2. [Einfaches Anwendungsbeispiel](#einfaches-anwendungsbeispiel)
3. [Daten im Local Storage speichern](#daten-im-local-storage-speichern)
4. [Daten aus Local Storage laden](#daten-aus-local-storage-laden)
5. [Neue Daten hinzufügen und speichern](#neue-daten-hinzufügen-und-speichern)
6. [Daten auf der Seite anzeigen](#daten-auf-der-seite-anzeigen)
7. [Initialisierung](#initialisierung)
8. [Zusammenfassung](#zusammenfassung)

## Was ist Local Storage?

Local Storage ist eine Browser-Funktion, mit der du Daten direkt im Browser des
Nutzers speichern kannst. Die Daten bleiben auch nach dem Schließen und erneuten
Öffnen des Browsers erhalten. Alles wird als String gespeichert.

---

## Einfaches Anwendungsbeispiel

```js
// Initiales Array mit Beispieldaten
let myData = ["Banane", "keine Banane", "Apfel"];
```

---

## Daten im Local Storage speichern

**localStorage.setItem(key, value)** speichert einen Wert unter einem Schlüssel.
Der Wert muss ein String sein, daher werden Arrays oder Objekte mit
**JSON.stringify()** umgewandelt.

```js
function saveToLocalStorage() {
  localStorage.setItem("myData", JSON.stringify(myData));
}
```

---

## Daten aus Local Storage laden

**localStorage.getItem(key)** liest einen Wert anhand des Schlüssels. Mit
**JSON.parse()** wandelst du den String wieder in ein Array oder Objekt um.

```js
function getFromLocalStorage() {
  let myArray = JSON.parse(localStorage.getItem("myData"));
  if (myArray == null) {
    myData = [];
  } else {
    myData = myArray;
  }
}
```

---

## Neue Daten hinzufügen und speichern

Du kannst neue Daten zum Array hinzufügen und dann im Local Storage speichern.

```js
function saveData() {
  let inputRef = document.getElementById("dataInput");
  if (inputRef.value != "") {
    myData.push(inputRef.value);
  }
  saveToLocalStorage();
  render();
  inputRef.value = "";
}
```

---

## Daten auf der Seite anzeigen

Du kannst die Daten aus dem Array im HTML anzeigen, z.B. als Liste oder als
Absätze.

```js
function render() {
  let contentRef = document.getElementById("content");
  contentRef.innerHTML = "";
  for (let i = 0; i < myData.length; i++) {
    contentRef.innerHTML += `<p>${myData[i]}</p>`;
  }
}
```

---

## Initialisierung

Beim Laden der Seite kannst du die Daten aus dem Local Storage laden und
anzeigen.

```js
function init() {
  getFromLocalStorage();
  render();
}
```

---

## Zusammenfassung

- Mit Local Storage kannst du Daten dauerhaft im Browser speichern.
- Mit **JSON.stringify()** und **JSON.parse()** speicherst und lädst du Arrays
  oder Objekte.
- Daten werden immer als String gespeichert.
- Nützlich, um Benutzereingaben, Einstellungen oder Listen lokal zu speichern.
