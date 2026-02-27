[Zurück zum Inhaltsverzeichnis](../README.md)

# Fehlerbehandlung mit try...catch in JavaScript

## Inhaltsverzeichnis

- [Fehlerbehandlung mit try...catch in JavaScript](#fehlerbehandlung-mit-trycatch-in-javascript)
  - [Inhaltsverzeichnis](#inhaltsverzeichnis)
  - [Was ist try...catch?](#was-ist-trycatch)
  - [Grundlegende Syntax](#grundlegende-syntax)
  - [Fehlerobjekt nutzen](#fehlerobjekt-nutzen)
  - [finally-Block](#finally-block)
  - [Eigene Fehler werfen](#eigene-fehler-werfen)
  - [Typische Anwendungsfälle](#typische-anwendungsfälle)
  - [Zusammenfassung](#zusammenfassung)

---

## Was ist try...catch?

Mit `try...catch` kannst du Fehler (Exceptions) im Code abfangen, ohne dass das
ganze Programm abstürzt.  
Das ist besonders nützlich, wenn du riskante Operationen durchführst (z.B.
Zugriff auf APIs, Parsen von JSON).

---

## Grundlegende Syntax

```js
try {
  // Code, der einen Fehler verursachen könnte
} catch (error) {
  // Dieser Block wird ausgeführt, wenn im try-Block ein Fehler auftritt
}
```

**Beispiel:**

```js
try {
  let result = 10 / x; // x ist nicht definiert
} catch (err) {
  console.log("Fehler aufgetreten:", err.message);
}
```

---

## Fehlerobjekt nutzen

Im `catch`-Block erhältst du ein Fehlerobjekt (meist `error` oder `err`
genannt), das Informationen über den Fehler enthält.

```js
try {
  JSON.parse("ungültiges JSON");
} catch (error) {
  console.log(error.name); // z.B. SyntaxError
  console.log(error.message); // z.B. Unexpected token u in JSON at position 0
}
```

---

## finally-Block

Mit `finally` kannst du Code ausführen, der immer läuft – egal ob ein Fehler
auftritt oder nicht.

```js
try {
  // ...
} catch (error) {
  // ...
} finally {
  console.log("Dieser Block läuft immer!");
}
```

---

## Eigene Fehler werfen

Mit `throw` kannst du eigene Fehler erzeugen.

```js
function divide(a, b) {
  if (b === 0) {
    throw new Error("Division durch 0 ist nicht erlaubt!");
  }
  return a / b;
}

try {
  divide(5, 0);
} catch (error) {
  console.log(error.message); // Ausgabe: Division durch 0 ist nicht erlaubt!
}
```

---

## Typische Anwendungsfälle

- Fehler beim Parsen von JSON abfangen
- Fehler bei Netzwerk-Anfragen behandeln
- Validierung von Benutzereingaben
- Eigene Fehler werfen und gezielt behandeln

---

## Was macht man mit Fehlern im echten Projekt?

Im echten Projekt solltest du Fehler nicht einfach nur in der Konsole anzeigen –
das sieht der Nutzer nicht und ist oft nicht hilfreich. Typische Strategien:

- **Nutzerfreundliche Fehlermeldung anzeigen:**  
  Zeige dem Nutzer eine verständliche Info, z.B. "Etwas ist schiefgelaufen,
  bitte versuche es später erneut."
- **Fehler protokollieren (Logging):**  
  Schreibe Fehler in ein zentrales Log (z.B. für Entwickler, Monitoring oder
  Support).
- **Fehler an einen Server senden:**  
  Sende Fehlerdaten an einen Backend-Service (z.B. für Fehleranalyse mit Sentry,
  LogRocket, eigene API).
- **Fallback-Logik:**  
  Führe eine alternative Aktion aus, z.B. Standardwerte verwenden oder einen
  anderen Service probieren.
- **Keine sensiblen Daten anzeigen:**  
  Zeige dem Nutzer niemals technische Details oder Stacktraces.

**Beispiel für Nutzerfeedback:**

```js
try {
  // riskanter Code
} catch (error) {
  showErrorMessage(
    "Leider ist ein Fehler aufgetreten. Bitte versuche es erneut.",
  );
  // Optional: Fehler an Server senden
  // sendErrorToServer(error);
}
```

**Merke:**  
Fehler sollten immer so behandelt werden, dass der Nutzer nicht verwirrt wird
und Entwickler trotzdem genug Infos zur Analyse bekommen.

---

## Zusammenfassung

- Mit `try...catch` kannst du Fehler im Code sicher abfangen.
- Das Fehlerobjekt im `catch`-Block liefert Infos zum Fehler.
- Mit `finally` führst du immer Code aus – egal ob ein Fehler auftritt.
- Mit `throw` kannst du eigene Fehler erzeugen.
