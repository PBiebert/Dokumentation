[Zurück zum Inhaltsverzeichnis](../README.md)

# TypeScript: Literaltypen und Tupel

## Inhaltsverzeichnis

- [Literaltypen](#literaltypen)
  - [Beispiele für Literaltypen](#beispiele-für-literaltypen)
  - [Anwendung: Enums ersetzen](#anwendung-enums-ersetzen)
- [Arrays mit bestimmten erlaubten Werten (Literaltypen)](#arrays-mit-bestimmten-erlaubten-werten-literaltypen)
- [Tupel (Tuples)](#tupel-tuples)
  - [Beispiel für ein Tupel](#beispiel-für-ein-tupel)
  - [Zugriff auf Tupel](#zugriff-auf-tupel)
  - [Optionale und Rest-Elemente](#optionale-und-rest-elemente)
- [Zusammenfassung](#zusammenfassung)

---

## Literaltypen

Mit Literaltypen kannst du Variablen auf ganz bestimmte Werte beschränken,
anstatt nur auf einen allgemeinen Typ wie `string` oder `number`.

### Beispiele für Literaltypen

```typescript
let richtung: "links" | "rechts";
richtung = "links"; // erlaubt
richtung = "rechts"; // erlaubt
richtung = "oben"; // Fehler!
```

Du kannst Literaltypen auch mit Zahlen oder Booleans verwenden:

```typescript
let antwort: 42 | 7;
antwort = 42; // erlaubt
antwort = 7; // erlaubt
antwort = 10; // Fehler!
```

### Anwendung: Enums ersetzen

Literaltypen werden oft genutzt, um feste Werte zu definieren, ähnlich wie
Enums:

```typescript
type Farbe = "rot" | "grün" | "blau";
let meineFarbe: Farbe = "rot";
```

---

## Arrays mit bestimmten erlaubten Werten (Literaltypen)

Du kannst Arrays so typisieren, dass sie nur bestimmte Werte enthalten dürfen,
z.B. nur `"rot"`, `"grün"` oder `"blau"`:

```typescript
let farben: ("rot" | "grün" | "blau")[];

farben = ["rot", "blau"]; // erlaubt
farben = ["grün", "rot", "blau"]; // erlaubt
farben = ["gelb"]; // Fehler! "gelb" ist nicht erlaubt
```

**Hinweis:**  
Mit `"wert1" | "wert2" | "wert3"` als Typ kannst du die erlaubten Werte explizit
festlegen.

---

## Tupel (Tuples)

Ein Tupel ist ein Array mit einer festen Anzahl und Reihenfolge von Typen. Damit
kannst du z.B. Rückgabewerte von Funktionen strukturieren.

### Beispiel für ein Tupel

```typescript
let person: [string, number];
person = ["Anna", 25]; // erlaubt
person = [25, "Anna"]; // Fehler! Reihenfolge stimmt nicht
person = ["Anna", 25, 1]; // Fehler! Zu viele Werte
```

### Zugriff auf Tupel

Du kannst wie bei Arrays auf die Werte zugreifen:

```typescript
console.log(person[0]); // "Anna"
console.log(person[1]); // 25
```

### Optionale und Rest-Elemente

Tupel können optionale oder beliebig viele Elemente am Ende enthalten:

```typescript
let punkt: [number, number?, number?];
punkt = [1];
punkt = [1, 2];
punkt = [1, 2, 3];

let rgb: [number, ...number[]];
rgb = [255];
rgb = [255, 128, 64];
```

---

## Zusammenfassung

- **Literaltypen** beschränken Variablen auf ganz bestimmte Werte.
- **Tupel** sind Arrays mit fester Länge und Typenreihenfolge.
- Beide Features helfen, den Code sicherer und klarer zu machen.
