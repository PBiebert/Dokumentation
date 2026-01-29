[Zurück zur Übersicht](../README.md)

# Filtern und Begrenzen von Firestore-Abfragen mit `query`, `limit`, `where`, `orderBy` und `onSnapshot`

## Zweck

Mit der Kombination aus `query`, `limit` und weiteren Filtern wie `where` und
Sortierungen mit `orderBy` kannst du gezielt Daten aus einer
Firestore-Collection abrufen. Die Methode `onSnapshot` ermöglicht es, diese
gefilterten und sortierten Daten in Echtzeit zu beobachten und auf Änderungen zu
reagieren.

---

## Funktionsweise im Projekt

Im Projekt werden Abfragen auf die Collection `notes` mit bestimmten Filtern
(`where`), Sortierungen (`orderBy`) und einer Begrenzung der Ergebnismenge
(`limit`) durchgeführt. Das ist besonders nützlich, um z.B. nur markierte
Notizen (`marked == true`), sortiert nach Titel, oder maximal 300 Einträge zu
laden.

### Beispiel: Abfrage mit `query`, `where`, `orderBy`, `limit` und `onSnapshot`

```typescript
const q = query(
  this.getNotesRef(),
  where("marked", "==", true), // Filter: Nur markierte Notizen
  orderBy("title"), // Sortierung: Nach Titel alphabetisch
  limit(300), // Maximal 300 Einträge
);

return onSnapshot(
  q,
  (list) => {
    this.normalMarkedNotes = [];
    list.forEach((element) => {
      this.normalMarkedNotes.push(
        this.setNoteObject(element.data(), element.id),
      );
    });
    list.docChanges().forEach((change) => {
      if (change.type === "added") {
        console.log("New note: ", change.doc.data());
      }
      if (change.type === "modified") {
        console.log("Modified note: ", change.doc.data());
      }
      if (change.type === "removed") {
        console.log("Removed note: ", change.doc.data());
      }
    });
  },
  (error) => {
    console.log("Error during onSnapshot:", error);
  },
);
```

---

## Erklärung der wichtigsten Query-Operatoren

### `where`

Mit `where` kannst du gezielt nach bestimmten Feldwerten filtern.  
Beispiel:

- `where('marked', '==', true)` gibt nur Notizen zurück, die als Favorit
  markiert sind.
- `where('title', '>=', 'A')` gibt alle Notizen zurück, deren Titel mit A oder
  später im Alphabet beginnen.

**Hinweis:**  
Mehrere `where`-Bedingungen sind möglich, aber nicht alle Kombinationen sind
erlaubt.  
Komplexe Filter benötigen manchmal zusätzliche Firestore-Indizes.

### `orderBy`

Mit `orderBy` kannst du die Reihenfolge der zurückgegebenen Dokumente
bestimmen.  
Beispiel:

- `orderBy('title')` sortiert alphabetisch nach Titel.
- `orderBy('createdAt', 'desc')` sortiert nach Erstellungsdatum absteigend.

**Wichtig:**  
Wenn du `where`-Filter auf ein Feld verwendest, das du auch mit `orderBy`
sortierst, muss dieses Feld in beiden Operatoren genannt werden.  
Beispiel:

- `where('status', '==', 'active'), orderBy('status')` ist erlaubt.

### `limit`

Mit `limit` kannst du die maximale Anzahl der zurückgegebenen Dokumente
festlegen.  
Das ist wichtig, um große Datenmengen und Performance-Probleme zu vermeiden.

---

## Parameter und Ablauf

- **`query`**: Baut eine Abfrage mit Filtern (`where`), Sortierungen (`orderBy`)
  und Begrenzungen (`limit`).
- **`where`**: Filtert die Dokumente nach bestimmten Kriterien.
- **`orderBy`**: Sortiert die Ergebnisse nach einem Feld.
- **`limit`**: Gibt die maximale Anzahl der zurückgegebenen Dokumente an.
- **`onSnapshot`**: Beobachtet die Abfrage in Echtzeit und liefert bei jeder
  Änderung die aktuelle Liste.

**Ablauf:**

1. Die Abfrage wird mit gewünschten Filtern, Sortierung und Limit erstellt.
2. `onSnapshot` registriert einen Listener auf diese Abfrage.
3. Bei jeder Änderung (Hinzufügen, Ändern, Löschen) wird die Callback-Funktion
   ausgeführt.
4. Optional: Mit `docChanges()` können Änderungen nach Typ unterschieden werden.

---

## Typische Anwendungsfälle

- **Favoriten-Filter:** Nur markierte Notizen anzeigen
  (`where('marked', '==', true)`).
- **Sortierung:** Notizen alphabetisch oder nach Datum sortieren
  (`orderBy('title')`).
- **Begrenzung:** Lade nur die ersten 300 Einträge (`limit(300)`), um
  Performance zu sichern.
- **Live-Updates:** Mit `onSnapshot` werden Änderungen sofort im UI sichtbar.

---

## Fehlerquellen und Troubleshooting

- **Kombination von `where` und `orderBy`:** Nicht alle Kombinationen sind
  erlaubt. Prüfe die
  [Firestore-Dokumentation](https://firebase.google.com/docs/firestore/query-data/queries).
  Oft ist ein zusätzlicher Index nötig, wenn du mehrere Filter und Sortierungen
  kombinierst.
- **Limit zu niedrig:** Es werden weniger Einträge angezeigt als erwartet.
- **Fehlende Indexe:** Bei komplexen Abfragen kann Firestore einen Index
  verlangen. Die Fehlermeldung enthält meist einen Link zum Erstellen des Index.
- **Sortierung und Filter auf unterschiedlichen Feldern:**  
  Wenn du z.B. nach `marked` filterst, aber nach `title` sortierst, kann ein
  Index nötig sein.

---

## Best Practices

- Nutze `limit`, um große Datenmengen zu vermeiden und die Performance zu
  verbessern.
- Kombiniere Filter (`where`) gezielt, um nur relevante Daten zu laden.
- Sortiere mit `orderBy`, wenn die Reihenfolge der Ergebnisse wichtig ist.
- Behandle Fehler im Callback von `onSnapshot`, um Probleme frühzeitig zu
  erkennen.
- Nutze `docChanges()`, um gezielt auf einzelne Änderungen zu reagieren (z.B.
  für Animationen im UI).

---

## Siehe auch

- [Firestore Doku zu Queries](https://firebase.google.com/docs/firestore/query-data/queries)
- [Firestore Doku zu onSnapshot](https://firebase.google.com/docs/firestore/query-data/listen)
- Eigene Methoden:
  - `subMarkedNotesList`: Beispiel für Filter und Limit mit Live-Updates
