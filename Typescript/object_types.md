[Back to Table of Contents](../README.md)

# TypeScript: Typen bei Objekten

## Objekt-Typen definieren

Mit TypeScript kannst du die Struktur von Objekten exakt festlegen. So wird
sichergestellt, dass nur erlaubte Eigenschaften mit den richtigen Typen
verwendet werden.

### Beispiel: Person-Objekt

```typescript
type Person = {
  readonly name: string;
  age: number;
  position?: string;
};

const person: Person = { name: "Florian", age: 50 };
```

- **`readonly`**: Die Eigenschaft kann nach der Initialisierung nicht mehr
  verändert werden.
- **`?` (optional)**: Die Eigenschaft ist optional, das heißt, sie muss beim
  Erstellen des Objekts nicht gesetzt werden.

### Zugriff und Verhalten

```typescript
person.age = 51; // erlaubt
person.name = "Anna"; // Fehler! name ist readonly

console.log(person.position); // undefined, wenn nicht gesetzt
```

---

## Das Fragezeichen (`?`) bei Eigenschaften

Das Fragezeichen macht eine Eigenschaft optional. Das bedeutet, sie kann fehlen
– der Wert ist dann `undefined`.

**Achtung:**  
Verwende das Fragezeichen nicht leichtfertig!  
Wenn eine Eigenschaft für die Logik deines Programms zwingend benötigt wird,
sollte sie **nicht** optional sein. Zu viele optionale Eigenschaften können zu
Fehlern führen, weil du immer prüfen musst, ob sie vorhanden sind.

---

## Objekt-Typen mit `interface`

Statt `type` kannst du für Objekt-Typen auch ein `interface` verwenden:

```typescript
interface Person {
  readonly name: string;
  age: number;
  position?: string;
}

const person: Person = { name: "Florian", age: 50 };
```

**Wann macht ein `interface` Sinn?**

- Wenn du möchtest, dass dein Typ von anderen erweitert oder implementiert
  werden kann (z.B. bei Klassen oder für Vererbung).
- Wenn du große Projekte hast, in denen mehrere Strukturen aufeinander aufbauen.
- Interfaces sind speziell für die Beschreibung von Objektstrukturen gedacht und
  werden von TypeScript bevorzugt, wenn es um reine Objekttypen geht.

**Kurz:**  
Verwende `interface` für Objektstrukturen, die erweitert werden sollen oder von
Klassen implementiert werden. Für einfache, einmalige Typdefinitionen reicht
meist auch `type`.

---

## Auslagern von Interfaces

Gerade in größeren Projekten ist es sinnvoll, Interfaces in eigene Dateien und
Ordner auszulagern. So bleibt der Code übersichtlich und wiederverwendbar.

**Beispielstruktur:**

```
src/
└── app/
    └── interfaces/
        └── person.interface.ts
```

**Inhalt von `person.interface.ts`:**

```typescript
// src/app/interfaces/person.interface.ts
export interface Person {
  name: string;
  age: number;
  position?: string;
}
```

**Verwendung im Code:**

```typescript
import { Person } from "./interfaces/person.interface";

const person: Person = { name: "Florian", age: 50 };
```

**Vorteile:**

- Interfaces sind zentral an einer Stelle gepflegt.
- Wiederverwendbarkeit und bessere Wartbarkeit.
- Klare Projektstruktur.

---

## Zusammenfassung

- Mit Objekt-Typen legst du die erlaubten Eigenschaften und deren Typen fest.
- `readonly` schützt vor versehentlichen Änderungen.
- Das Fragezeichen `?` macht Eigenschaften optional – nutze es mit Bedacht!
- Für erweiterbare Objektstrukturen empfiehlt sich die Verwendung von
  `interface`.
- Interfaces sollten in größeren Projekten ausgelagert und zentral abgelegt
  werden.
