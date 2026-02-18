[Zurück zum Inhaltsverzeichnis](../README.md)

# Date-Objekt in JavaScript

## Inhaltsverzeichnis

1. [Date – Grundlagen](#date--grundlagen)
2. [Timestamps](#timestamps)
3. [Datum und Uhrzeit lesen & setzen](#datum-und-uhrzeit-lesen--setzen)
   - [Getter](#getter)
   - [Setter](#setter)
4. [Datum und Uhrzeit formatieren](#datum-und-uhrzeit-formatieren)
5. [Zeit berechnen & vergleichen](#zeit-berechnen--vergleichen)
   - [Differenz berechnen](#differenz-berechnen)
   - [Vergleich](#vergleich)
6. [Nützliche Funktionen](#nützliche-funktionen)
   - [Datum formatieren (tt.mm.jjjj)](#datum-formatieren-ttmmjjjj)
   - [Tage bis zu einem Datum](#tage-bis-zu-einem-datum)
7. [Häufige Fehler & Tipps](#häufige-fehler--tipps)
8. [Alternativen: Date-Bibliotheken](#alternativen-date-bibliotheken)
9. [Zusammenfassung](#zusammenfassung)

---

## Date – Grundlagen

Das Date-Objekt speichert einen bestimmten Zeitpunkt. Es kann mit dem aktuellen
Datum oder einem eigenen Wert erstellt werden.

```js
let now = new Date(); // aktuelles Datum & Uhrzeit
let birthday = new Date("1990-05-15"); // Datum aus String
let custom = new Date(2025, 5, 30, 12, 0); // Jahr, Monat, Tag, Stunde, Minute
```

---

## Timestamps

Ein Timestamp ist die Anzahl der Millisekunden seit dem 1.1.1970 (UTC).
Timestamps sind nützlich zum Vergleichen und Berechnen von Zeitpunkten.

```js
let ts = Date.now(); // aktueller Timestamp
let ts2 = now.getTime(); // Timestamp aus Date-Objekt
```

Umwandlung zwischen Date und Timestamp:

```js
let dateFromTs = new Date(ts); // Timestamp zu Date
let tsFromDate = birthday.getTime(); // Date zu Timestamp
```

---

## Datum und Uhrzeit lesen & setzen

Getter-Methoden lesen einzelne Teile des Datums, wie Jahr, Monat oder Stunde.
Setter-Methoden ändern diese Werte.

### Getter

```js
let d = new Date();
d.getFullYear(); // Jahr, z.B. 2025
d.getMonth(); // Monat (0 = Januar)
d.getDate(); // Tag des Monats
d.getDay(); // Wochentag (0 = Sonntag)
d.getHours(); // Stunde
d.getMinutes(); // Minute
d.getSeconds(); // Sekunde
```

### Setter

Setter-Methoden ändern bestimmte Teile des Datums.

```js
d.setFullYear(2026); // Jahr setzen
d.setMonth(11); // Monat setzen (Dezember)
d.setDate(24); // Tag setzen
d.setHours(23); // Stunde setzen
d.setMinutes(59); // Minute setzen
d.setSeconds(59); // Sekunde setzen
```

---

## Datum und Uhrzeit formatieren

Das Date-Objekt bietet verschiedene Methoden, um Daten als Text darzustellen –
standardisiert oder lokalisiert.

```js
d.toString(); // "Mon Jun 30 2025 12:00:00 GMT+0200"
d.toDateString(); // "Mon Jun 30 2025"
d.toTimeString(); // "12:00:00 GMT+0200"
d.toISOString(); // "2025-06-30T10:00:00.000Z"
d.toLocaleDateString(); // "30.6.2025" (abhängig von Sprache)
d.toLocaleTimeString(); // "12:00:00"
```

Formatieren mit Optionen:

```js
d.toLocaleString("de", {
  weekday: "long",
  year: "numeric",
  month: "long",
  day: "numeric",
}); // "Montag, 30. Juni 2025"
```

---

## Zeit berechnen & vergleichen

Date-Objekte können verwendet werden, um Zeitspannen zu berechnen und Zeitpunkte
zu vergleichen.

### Differenz berechnen

```js
let start = new Date("2025-06-30T08:00:00Z");
let end = new Date("2025-06-30T10:30:00Z");
let diffMs = end - start; // Differenz in Millisekunden
let diffMin = diffMs / 1000 / 60; // Differenz in Minuten
```

### Vergleich

```js
if (start < end) {
  console.log("Start ist früher");
}
```

---

## Nützliche Funktionen

Hilfsfunktionen für typische Aufgaben mit Datum und Zeit.

### Datum formatieren (tt.mm.jjjj)

```js
function formatDate(date) {
  let day = date.getDate().toString().padStart(2, "0");
  let month = (date.getMonth() + 1).toString().padStart(2, "0");
  let year = date.getFullYear();
  return `${day}.${month}.${year}`;
}
formatDate(new Date()); // "30.06.2025"
```

### Tage bis zu einem Datum

```js
function daysUntil(date) {
  let today = new Date();
  let diff = date.getTime() - today.getTime();
  return Math.ceil(diff / (1000 * 60 * 60 * 24));
}
let vacation = new Date("2025-07-19");
console.log(`${daysUntil(vacation)} Tage bis zum Urlaub`);
```

---

## Häufige Fehler & Tipps

- Monate sind nullbasiert: Januar = 0, Dezember = 11.
- Der Vergleich von Date-Objekten mit `<`, `>`, `===` bezieht sich auf den
  Zeitpunkt, nicht auf das Objekt selbst.
- Auf Zeitzonen achten: `toISOString()` gibt UTC zurück, lokale Methoden geben
  lokale Zeit aus.
- Setter-Methoden wie `setDate()` verändern das Date-Objekt direkt.

---

## Alternativen: Date-Bibliotheken

Für komplexere Aufgaben gibt es spezialisierte Bibliotheken.

| Bibliothek | Vorteile                | Nachteile        |
| ---------- | ----------------------- | ---------------- |
| Moment.js  | Einfach, viele Features | Groß, veraltet   |
| date-fns   | Modular, modern         | Weniger Features |
| Day.js     | Klein, ähnlich Moment   | Weniger Plugins  |

---

## Zusammenfassung

- Das Date-Objekt ist die Basis für die Arbeit mit Datum und Zeit in JavaScript.
- Timestamps ermöglichen Vergleiche und Berechnungen.
- Methoden zum Lesen, Setzen und Formatieren sind verfügbar.
- Bibliotheken sind für komplexe Anforderungen nützlich.
- Zeitzonen und Monatsindex sind häufige Fehlerquellen.
