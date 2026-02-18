[Zurück zum Inhaltsverzeichnis](../README.md)

# TypeScript: Primitive Typen

## Inhaltsverzeichnis

- [Was sind Primitive Typen?](#was-sind-primitive-typen)
- [Typzuweisung](#typzuweisung)
- [Union Types](#union-types)
- [Der Typ `any`](#der-typ-any)
- [Zusammenfassung](#zusammenfassung)

---

## Was sind Primitive Typen?

Primitive Typen sind die grundlegenden Datentypen in TypeScript. Sie speichern
einfache Werte und sind keine Objekte.

- **string** – Zeichenketten (z.B. `"Hallo"`)
- **number** – Zahlen (ganzzahlig und Kommazahlen, z.B. `42`, `3.14`)
- **boolean** – Wahrheitswerte (`true` oder `false`)
- **null** – explizit kein Wert (eigener Typ, selten direkt verwendet)
- **undefined** – Wert wurde nicht zugewiesen (eigener Typ, selten direkt
  verwendet)
- **bigint** – sehr große Ganzzahlen (z.B. `9007199254740991n`)
- **symbol** – eindeutige, unveränderliche Werte (z.B. `Symbol("id")`)

> Es gibt keine weiteren primitiven Typen in TypeScript.

---

## Typzuweisung

Du kannst Variablen explizit einen Typ zuweisen:

```typescript
let benutzername: string = "Anna";
let alter: number = 25;
let istAdmin: boolean = false;
let nichtsHier: null = null;
let nichtZugewiesen: undefined = undefined;
let grosseZahl: bigint = 9007199254740991n;
let eindeutigeId: symbol = Symbol("id");
```

Oder TypeScript erkennt den Typ automatisch (Typinferenz):

```typescript
let stadt = "Berlin"; // Typ: string
let anzahl = 10; // Typ: number
```

---

## Union Types

Du kannst angeben, dass eine Variable mehrere Typen annehmen darf:

```typescript
let wert: string | number;
wert = "Hallo";
wert = 42;
```

---

## Der Typ `any`

Mit `any` kann eine Variable jeden beliebigen Typ annehmen:

```typescript
let irgendwas: any = "Text";
irgendwas = 123;
irgendwas = false;
```

> **Hinweis:**  
> Die Verwendung von `any` sollte vermieden werden, da damit die Vorteile von
> TypeScript verloren gehen. `any` signalisiert, dass der Typ nicht bekannt ist
> – das kann zu Fehlern führen und macht den Code schwer wartbar.

---

## Zusammenfassung

- Primitive Typen: string, number, boolean, null, undefined, bigint, symbol
- Typen werden explizit zugewiesen oder automatisch erkannt
- Mit Union Types (`type1 | type2`) kann eine Variable mehrere Typen annehmen
- `any` nur im Notfall verwenden, da sonst Typsicherheit verloren geht
