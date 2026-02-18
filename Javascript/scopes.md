[Zurück zum Inhaltsverzeichnis](../README.md)

# Scopes in JavaScript

## Inhaltsverzeichnis

1. [Was ist ein Scope?](#was-ist-ein-scope)
2. [Block-Scope mit let und const](#block-scope-mit-let-und-const)
3. [Funktions-Scope mit var](#funktions-scope-mit-var)
4. [Globaler Scope](#globaler-scope)
5. [Zusammenfassung](#zusammenfassung)

## Was ist ein Scope?

Ein Scope bestimmt, wo im Code du auf Variablen zugreifen kannst. In JavaScript
gibt es verschiedene Arten von Scopes:

- **Globaler Scope**: Variablen, die außerhalb von Funktionen deklariert werden,
  sind überall im Code sichtbar.
- **Funktions-Scope**: Variablen, die mit `var`, `let` oder `const` innerhalb
  einer Funktion deklariert werden, sind nur innerhalb dieser Funktion sichtbar.
- **Block-Scope**: Variablen, die mit `let` oder `const` innerhalb eines Blocks
  (z.B. if, for) deklariert werden, sind nur innerhalb dieses Blocks sichtbar.

---

## Block-Scope mit let und const

**let** und **const** sind blockbasiert, das heißt sie sind nur innerhalb des
umgebenden Blocks sichtbar.

```js
function scopeTest() {
  if (true) {
    let testScopeVar = "hello world"; // Block-Scope
  }
  console.log(testScopeVar); // ReferenceError: testScopeVar is not defined
}
```

- `testScopeVar` ist nur innerhalb des if-Blocks sichtbar.
- Außerhalb des Blocks (aber noch in der Funktion) führt der Zugriff zu einem
  Fehler.

---

## Funktions-Scope mit var

**var** ist nur funktionsbasiert, nicht blockbasiert. Das bedeutet, eine mit var
deklarierte Variable ist innerhalb der gesamten Funktion sichtbar, auch
außerhalb von Blöcken.

```js
function varScopeTest() {
  if (true) {
    var testVar = "sichtbar";
  }
  console.log(testVar); // Ausgabe: sichtbar
}
```

- `testVar` ist in der gesamten Funktion sichtbar, auch außerhalb des if-Blocks.

---

## Globaler Scope

Variablen, die außerhalb von Funktionen deklariert werden, sind global und
überall im Code sichtbar.

```js
let globalVar = "Ich bin global";

function showGlobal() {
  console.log(globalVar); // Ausgabe: Ich bin global
}

showGlobal();
```

---

## Zusammenfassung

- Verwende `let` und `const` für blockbasierte Sichtbarkeit.
- Mit `var` deklarierte Variablen sind nur innerhalb der Funktion sichtbar.
- Vermeide globale Variablen, um Fehler und unerwartetes Verhalten zu
  verhindern.
- Scopes helfen, Variablen sauber und sicher zu halten.
