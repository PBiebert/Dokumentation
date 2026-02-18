[Zurück zum Inhaltsverzeichnis](../README.md)

# TypeScript: Typen in Arrays und Funktionen

## Inhaltsverzeichnis

- [Typen in Arrays](#typen-in-arrays)
  - [Arrays mit mehreren Typen (Union Types)](#arrays-mit-mehreren-typen-union-types)
- [Arrays mit ausschließlich numbers oder ausschließlich strings](#arrays-mit-ausschließlich-numbers-oder-ausschließlich-strings)
- [Typen in Funktionen](#typen-in-funktionen)
  - [Funktionen mit optionalen und Standard-Parametern](#funktionen-mit-optionalen-und-standard-parametern)
  - [Funktionen als Typ](#funktionen-als-typ)
- [Arrays von Funktionen](#arrays-von-funktionen)
- [Verschachtelte Typen in Arrays](#verschachtelte-typen-in-arrays)
- [Zusammenfassung](#zusammenfassung)

---

## Typen in Arrays

Du kannst angeben, welchen Typ die Elemente eines Arrays haben sollen:

```typescript
let zahlen: number[] = [1, 2, 3, 4];
let namen: string[] = ["Anna", "Ben", "Clara"];
```

Alternativ mit Generics:

```typescript
let flags: Array<boolean> = [true, false, true];
```

### Arrays mit mehreren Typen (Union Types)

```typescript
let gemischt: (string | number)[] = ["Anna", 42, "Ben"];
```

---

## Arrays mit ausschließlich numbers oder ausschließlich strings

Wenn du möchtest, dass ein Array **nur** Zahlen oder **nur** Strings enthalten
darf (aber nicht gemischt), kannst du eine Union auf Array-Ebene verwenden:

```typescript
let nurZahlenOderNurStrings: number[] | string[];

nurZahlenOderNurStrings = [1, 2, 3]; // erlaubt
nurZahlenOderNurStrings = ["a", "b", "c"]; // erlaubt
nurZahlenOderNurStrings = [1, "b", 3]; // Fehler! Gemischt nicht erlaubt
```

**Hinweis:**  
`number[] | string[]` bedeutet: Das Array ist entweder ein reines Zahlen-Array
**oder** ein reines String-Array.  
Im Gegensatz dazu erlaubt `(number | string)[]` gemischte Werte im selben Array.

---

## Typen in Funktionen

Du kannst für Parameter und Rückgabewerte Typen angeben:

```typescript
function addieren(a: number, b: number): number {
  return a + b;
}

function begruessen(name: string): void {
  console.log("Hallo, " + name);
}
```

- Der Rückgabetyp `void` bedeutet, dass die Funktion keinen Wert zurückgibt.

---

### Funktionen mit optionalen und Standard-Parametern

```typescript
// log gibt eine Nachricht aus. Der zweite Parameter user ist optional (user?: string).
// Wird user übergeben, wird er mit ausgegeben, sonst nur die Nachricht.
function log(message: string, user?: string) {
  console.log(user ? `[${user}] ${message}` : message);
}

// multiply multipliziert zwei Zahlen. Der zweite Parameter b hat einen Standardwert von 2.
// Wird b nicht übergeben, wird automatisch 2 verwendet.
function multiplizieren(a: number, b: number = 2): number {
  return a * b;
}
```

- Ein Parameter mit `?` ist optional und kann beim Funktionsaufruf weggelassen
  werden.
- Ein Parameter mit `=` bekommt einen Standardwert, falls kein Wert übergeben
  wird.

---

### Funktionen als Typ

Du kannst Funktions-Typen explizit angeben:

```typescript
let operation: (a: number, b: number) => number;
operation = (x, y) => x + y;
```

---

## Arrays von Funktionen

```typescript
let callbacks: Array<() => void> = [
  () => console.log("A"),
  () => console.log("B"),
];
```

**Erklärung:**  
Hier wird ein Array `callbacks` deklariert, das nur Funktionen ohne Parameter
und ohne Rückgabewert (`void`) enthalten darf.  
Jedes Element im Array ist also eine Funktion, die z.B. als Callback verwendet
werden kann.  
Mit `callbacks[0]()` wird die erste Funktion im Array ausgeführt und gibt `"A"`
in der Konsole aus.

Solche Arrays sind nützlich, wenn du mehrere Aktionen (z.B. Event-Handler)
gesammelt speichern und später ausführen möchtest.

---

## Verschachtelte Typen in Arrays

Verschachtelte Typen in TypeScript bedeuten, dass du Arrays und deren Inhalte
sehr präzise beschreiben kannst. Hier ein paar Tipps und Beispiele:

1. **Ein Array mit verschiedenen Typen**  
   Das Array kann Zahlen oder Strings enthalten:

   ```typescript
   let gemischt: (number | string)[] = [1, "zwei", 3, "vier"];
   ```

2. **Ein Array, das Arrays enthalten kann**  
   Das Array kann entweder Zahlen oder Arrays von Zahlen enthalten:

   ```typescript
   let verschachtelt: (number | number[])[] = [1, [2, 3], 4, [5, 6]];
   ```

3. **Ein Array, das bestimmte verschachtelte Strukturen erlaubt**  
   Das Array kann enthalten:
   - einen String
   - ein Array mit genau `[string, boolean, number[]]`
   - `null`
   ```typescript
   let komplex: (string | [string, boolean, number[]] | null)[] = [
     "Hallo",
     ["Test", true, [1, 2, 3]],
     null,
     "Welt",
   ];
   ```

**Hinweis:**  
Mit solchen verschachtelten Typen kannst du sehr genau festlegen, welche
Strukturen in Arrays erlaubt sind. Das erhöht die Typsicherheit und Lesbarkeit
deines Codes.

---

## Zusammenfassung

- Arrays und Funktionen können und sollten mit Typen versehen werden.
- Mit Union Types können Arrays mehrere Typen enthalten.
- Funktions-Typen machen den Code sicherer und verständlicher.
- `any` nur im Notfall verwenden, um Typsicherheit zu erhalten.
