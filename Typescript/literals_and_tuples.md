[Back to Table of Contents](../README.md)

# TypeScript: Literal Types und Tupel

## Literal Types

Mit Literal Types kannst du Variablen auf ganz bestimmte Werte beschränken,
anstatt nur auf einen allgemeinen Typ wie `string` oder `number`.

### Beispiele für Literal Types

```typescript
let direction: "left" | "right";
direction = "left"; // erlaubt
direction = "right"; // erlaubt
direction = "up"; // Fehler!
```

Du kannst Literal Types auch mit Zahlen oder Booleans verwenden:

```typescript
let answer: 42 | 7;
answer = 42; // erlaubt
answer = 7; // erlaubt
answer = 10; // Fehler!
```

### Anwendung: Enums ersetzen

Literal Types werden oft genutzt, um feste Werte zu definieren, ähnlich wie
Enums:

```typescript
type Color = "red" | "green" | "blue";
let myColor: Color = "red";
```

---

## Arrays mit bestimmten erlaubten Werten (Literal Types)

Du kannst Arrays so typisieren, dass sie nur bestimmte Werte enthalten dürfen,
z.B. nur `"rot"`, `"grün"` oder `"blau"`:

```typescript
let colors: ("rot" | "grün" | "blau")[];

colors = ["rot", "blau"]; // erlaubt
colors = ["grün", "rot", "blau"]; // erlaubt
colors = ["gelb"]; // Fehler! "gelb" ist nicht erlaubt
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

### Optional und Rest-Elemente

Tupel können optionale oder beliebig viele Elemente am Ende enthalten:

```typescript
let point: [number, number?, number?];
point = [1];
point = [1, 2];
point = [1, 2, 3];

let rgb: [number, ...number[]];
rgb = [255];
rgb = [255, 128, 64];
```

---

## Zusammenfassung

- **Literal Types** beschränken Variablen auf ganz bestimmte Werte.
- **Tupel** sind Arrays mit fester Länge und Typenreihenfolge.
- Beide Features helfen, den Code sicherer und klarer zu machen.
