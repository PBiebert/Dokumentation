# Asynchrone Programmierung, Promises & fetch

## Inhaltsverzeichnis

1. [Was bedeutet asynchron?](#1-was-bedeutet-asynchron)
2. [Was ist ein Promise?](#2-was-ist-ein-promise)
3. [Wie funktionieren resolve und reject bei Promises?](#3-wie-funktionieren-resolve-und-reject-bei-promises)
4. [Die fetch-API](#4-die-fetch-api)
5. [Was ist ein eigenes Promise und wofür braucht man das?](#5-was-ist-ein-eigenes-promise-und-wofür-braucht-man-das)
6. [async & await](#6-async--await)
7. [Fehlerbehandlung](#7-fehlerbehandlung)
8. [Mehrere APIs gleichzeitig abfragen (Promise.all)](#8-mehrere-apis-gleichzeitig-abfragen-promiseall)
9. [Typische Anwendungsbeispiele](#9-typische-anwendungsbeispiele)
10. [Zusammenfassung](#10-zusammenfassung)
11. [Praxisbeispiel: Daten laden, speichern und weiterverarbeiten](#11-praxisbeispiel-daten-laden-speichern-und-weiterverarbeiten)

## 1. Was bedeutet asynchron?

Asynchrone Operationen laufen im Hintergrund ab, ohne den Haupt-Thread zu
blockieren. Das ist wichtig, wenn du z.B. auf Daten aus dem Internet wartest.

---

## 2. Was ist ein Promise?

Ein **Promise** ist ein Objekt, das einen zukünftigen Wert repräsentiert. Es hat
drei Zustände:

- _pending_ (läuft noch)
- _fulfilled_ (erfolgreich)
- _rejected_ (Fehler)

---

## 3. Wie funktionieren `resolve` und `reject` bei Promises?

Wenn du ein eigenes Promise erstellst, bekommst du zwei Funktionen: `resolve`
und `reject`.

- Mit `resolve(wert)` sagst du: Das Promise ist erfolgreich abgeschlossen und
  liefert `wert` zurück.
- Mit `reject(fehlermeldung)` sagst du: Das Promise ist fehlgeschlagen und gibt
  einen Fehler zurück.

**Beispiel:**

```js
function meinPromise() {
  return new Promise((resolve, reject) => {
    if (Math.random() > 0.5) {
      resolve("Erfolg!");
    } else {
      reject("Fehler!");
    }
  });
}

meinPromise()
  .then((result) => console.log(result))
  .catch((error) => console.error(error));
```

---

## 4. Die fetch-API

`fetch` ist die Standardmethode, um HTTP-Anfragen im Browser zu machen. Sie gibt
immer ein Promise zurück.

**Beispiel:**

```js
fetch("datei.json")
  .then((response) => response.json())
  .then((data) => console.log(data));
```

---

## 5. Was ist ein eigenes Promise und wofür braucht man das?

Ein eigenes Promise erstellst du, wenn du eine asynchrone Operation hast, die
nicht schon von Haus aus ein Promise zurückgibt (wie z.B. fetch). Das ist oft
bei eigenen zeitverzögerten Abläufen, Events oder älteren Callback-Funktionen
der Fall.

**Wofür verwendet man eigene Promises?**

- Um eigene asynchrone Abläufe zu kapseln und kontrolliert auf Erfolg oder
  Fehler zu reagieren.
- Um alte Callback-APIs in moderne Promise-APIs umzuwandeln.
- Um z.B. Wartezeiten, Animationen oder komplexe Logik asynchron zu steuern.

**Beispiel:**

```js
function warte(ms) {
  return new Promise((resolve) => {
    setTimeout(() => {
      resolve("Fertig gewartet!");
    }, ms);
  });
}

// Anwendung:
warte(1000).then((text) => console.log(text)); // Gibt nach 1 Sekunde "Fertig gewartet!" aus
```

Du kannst also mit eigenen Promises alles, was asynchron ist, in eine moderne,
einfach zu verwendende Form bringen.

---

## 6. async & await

Mit `async` markierst du eine Funktion als asynchron. Mit `await` wartest du auf
die Auflösung eines Promises, ohne den Code zu blockieren.

**Beispiel:**

```js
async function ladeDaten() {
  let response = await fetch("datei.json");
  let daten = await response.json();
  console.log(daten);
}
```

---

## 7. Fehlerbehandlung

Fehler bei asynchronen Operationen solltest du immer abfangen. Das geht mit
`.catch()` oder – bei `async/await` – mit `try...catch`:

**Beispiel:**

```js
async function ladeDaten() {
  try {
    let response = await fetch("datei.json");
    if (!response.ok) throw new Error("Fehler beim Laden!");
    let daten = await response.json();
    console.log(daten);
  } catch (error) {
    console.error("Fehler:", error);
  }
}
```

---

## 8. Mehrere APIs gleichzeitig abfragen (Promise.all)

Wenn du Daten aus mehreren unabhängigen APIs gleichzeitig abrufen möchtest (z.B.
bei der Pokémon-API die Basisdaten und bei der Species-API die deutschen Namen),
kannst du `Promise.all` verwenden. Damit laufen beide Anfragen parallel und du
wartest, bis beide fertig sind.

### Clean Code Ansatz: Fetch-Logik auslagern

Im Sinne von Clean Code sollten Funktionen kurz und fokussiert sein. Daher ist
es sinnvoll, die fetch-Befehle in separate Funktionen auszulagern:

```js
async function fetchSinglePokemon(pokemon) {
  try {
    const responsePokemonData = await fetch(pokemon.url);
    const pokemonData = await responsePokemonData.json();

    const responsePokemonSpecies = await fetch(pokemonData.species.url);
    const pokemonSpecies = await responsePokemonSpecies.json();

    return { pokemonData, pokemonSpecies };
  } catch (error) {
    console.error("Fehler beim Laden:", error);
    return null;
  }
}

async function loadPokemonDetails(fetchStack, saveStack) {
  try {
    const promises = [];

    // Alle Fetch-Anfragen gleichzeitig starten
    for (let i = 0; i < fetchStack.length; i++) {
      promises.push(fetchSinglePokemon(fetchStack[i]));
    }

    // Auf alle Promises warten
    const allPokemonData = await Promise.all(promises);

    // Daten weiterverarbeiten
    for (let i = 0; i < allPokemonData.length; i++) {
      const pokemonData = allPokemonData[i].pokemonData;
      const pokemonSpecies = allPokemonData[i].pokemonSpecies;
      // Weitere Verarbeitung...
      saveStack.push({ pokemonData, pokemonSpecies });
    }
  } catch (error) {
    console.error("Fehler beim Laden der Details:", error);
  }
}
```

**Vorteile:**

- Beide API-Anfragen (Pokemon + Species) laufen parallel und sparen Zeit
- Mehrere Pokémon werden gleichzeitig geladen (alle Promises in der Schleife)
- Klare Trennung der Verantwortlichkeiten: `fetchSinglePokemon` lädt Daten,
  `loadPokemonDetails` koordiniert
- Bessere Lesbarkeit und Wartbarkeit durch kleinere, fokussierte Funktionen

---

## 9. Typische Anwendungsbeispiele

### JSON laden

```js
async function fetchDataJson() {
  let response = await fetch("db.json");
  let responseAsJson = await response.json();
  console.log(response);
  console.log(responseAsJson);
}
```

### Text laden

```js
async function fetchDataText() {
  let response = await fetch("h1.txt");
  let responseAsText = await response.text();
  document.getElementById("content").innerHTML = responseAsText;
}
```

### POST-Request

```js
async function postData(url = "", data = {}) {
  const response = await fetch(url, {
    method: "post",
    headers: {
      "Content-Type": "application/json",
    },
    redirect: "follow",
    body: JSON.stringify(data),
  });
  return response.json();
}
```

### Eigenes Promise

```js
function getPromise() {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      if (prommisError) {
        reject("hat nicht geklappt");
      } else {
        resolve("hat geklappt");
      }
    }, 300);
  });
}
```

### Promise mit async/await nutzen

```js
async function usePromise() {
  try {
    await getPromise();
    console.log("klappt");
  } catch (error) {
    console.error(error);
  }
  console.log("ende");
}
```

---

## 10. Zusammenfassung

- Asynchrone Operationen blockieren den Code nicht.
- Promises sind die Grundlage für moderne asynchrone Programmierung.
- fetch gibt ein Promise zurück.
- Mit async/await kannst du Promises einfach nutzen.
- Fehlerbehandlung ist wichtig!

---

## 11. Praxisbeispiel: Daten laden, speichern und weiterverarbeiten

Im folgenden Beispiel werden Fruchtdaten von einer API geladen, in einem Array
gespeichert und anschließend im HTML angezeigt – inklusive Fehlerbehandlung:

```js
let fruits = [];

async function init() {
  await fetchDataJson();
  renderFruits();
}

async function fetchDataJson() {
  try {
    const response = await fetch(`https://www.fruityvice.com/api/fruit/all`);
    if (!response.ok) throw new Error("Fehler beim Laden!");
    const responseAsJson = await response.json();
    fruits = responseAsJson; // Daten im Array speichern
  } catch (error) {
    console.error("Fehler beim Laden der Früchte:", error);
  }
}

function renderFruits() {
  console.log(fruits); // Array mit allen Fruchtdaten
  let container = document.querySelector(`#content`);
  container.innerHTML = "";

  for (let i = 0; i < fruits.length; i++) {
    container.innerHTML += `${fruits[i].name} <br>`;
  }
}
```

**Was passiert hier?**

- Mit `fetchDataJson()` werden die Daten asynchron geladen und als Array in
  `fruits` gespeichert.
- Mit `renderFruits()` werden die Daten weiterverarbeitet und im HTML angezeigt.
- Du kannst das Array `fruits` beliebig weiterverwenden, z.B. filtern, sortieren
  oder einzelne Früchte anzeigen.

**Tipp:**  
Du kannst die Daten auch weiterverarbeiten, z.B. nach bestimmten Kriterien
filtern:

```js
let citrusFruits = fruits.filter((fruit) => fruit.family === "Rutaceae");
console.log(citrusFruits);
```
