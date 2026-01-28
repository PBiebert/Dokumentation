[Zurück zur Übersicht](../README.md)

# Einzelne Felder eines Dokuments mit `updateDoc` in Firebase aktualisieren

## Zweck

Die Funktion `updateDoc` aus der Firebase Firestore-Bibliothek dient dazu,
bestehende Dokumente in einer Collection gezielt zu aktualisieren, ohne das
gesamte Dokument zu ersetzen. In deinem Projekt wird diese Methode verwendet, um
Notizen (`Note`) nachträglich zu bearbeiten und die Änderungen in der Cloud zu
speichern.

---

## Funktionsweise im Projekt

Wenn ein Nutzer eine Notiz bearbeitet (z.B. Titel, Inhalt oder Markierung
ändert), wird die Methode `updateNote` im Service `NoteListService` aufgerufen.
Diese Methode sorgt dafür, dass die Änderungen in der Firestore-Datenbank
übernommen werden.

### Beispiel: updateNote

```typescript
async updateNote(note: Note) {
  if (note.id) {
    try {
      let docRef = this.getSingleDocRef(this.getColIdFromNote(note), note.id);
      await updateDoc(docRef, this.getCleanJson(note));
    } catch (error) {
      console.log('Error during updateNote: ', error);
    }
  }
}
```

**Ablauf:**

1. Es wird geprüft, ob die Notiz eine gültige `id` besitzt.
2. Mit `getColIdFromNote` wird ermittelt, in welcher Collection (`notes` oder
   `trash`) sich die Notiz befindet.
3. Mit `getSingleDocRef` wird eine Referenz auf das konkrete Dokument erzeugt.
4. Mit `getCleanJson` wird die Notiz in ein einfaches JSON-Objekt umgewandelt.
5. `updateDoc` aktualisiert das Dokument in Firestore.
6. Fehler werden im Catch-Block geloggt.

---

## Zentrale Methoden im Detail

### getSingleDocRef

Erstellt eine gültige Referenz auf ein Dokument in einer Collection.  
**Wichtig:** Die Referenz muss exakt zwei Segmente haben: Collection-Name und
Dokument-ID.

```typescript
getSingleDocRef(colId: string, docId: string) {
  // Erstellt eine Dokument-Referenz wie z.B. "notes/abc123"
  // doc(collection(this.firestore, colId), docId) ist korrekt!
  return doc(collection(this.firestore, colId), docId);
}
```

**Hinweis:**  
Falsch wäre z.B. `doc(collection(this.firestore, colId, docId))`, da dies eine
Collection-Referenz mit zu vielen Segmenten erzeugt.

---

### getColIdFromNote

Bestimmt anhand des Typs der Notiz, in welcher Collection sie gespeichert ist.

```typescript
getColIdFromNote(note: Note) {
  if (note.type == 'note') {
    return 'notes';
  } else {
    return 'trash';
  }
}
```

- `"note"` → Collection `"notes"`
- alles andere (z.B. `"trash"`) → Collection `"trash"`

---

### getCleanJson

Wandelt das Notiz-Objekt in ein einfaches JSON um, das für Firestore geeignet
ist.  
**Warum?** Firestore erwartet ein plain JSON-Objekt ohne zusätzliche Methoden
oder Metadaten.

```typescript
getCleanJson(note: Note): {} {
  return {
    type: note.type,
    title: note.title,
    content: note.content,
    marked: note.marked,
  };
}
```

**Beispiel:**  
Eine Notiz mit weiteren Properties oder Methoden würde beim direkten Speichern
zu Fehlern führen.

---

## Typischer Ablauf im UI

1. **Bearbeiten:** Nutzer ändert eine Notiz im Frontend.
2. **Speichern:** Die Methode `saveNote()` im Component ruft `updateNote()` im
   Service auf.
3. **Service:** Der Service bereitet die Daten auf und ruft `updateDoc` auf.
4. **Firestore:** Das Dokument wird gezielt aktualisiert, ohne andere Felder zu
   beeinflussen.

---

## Fehlerquellen und Troubleshooting

- **Ungültige Referenz:**  
  Die Dokumenten-Referenz muss exakt zwei Segmente haben (`collection/docId`).  
  Fehler wie
  `FirebaseError: Invalid collection reference. Collection references must have an odd number of segments...`
  deuten auf eine falsche Referenzerstellung hin.

- **Fehlende ID:**  
  Das zu aktualisierende Objekt muss eine gültige `id` besitzen, sonst kann
  Firestore das Dokument nicht finden.

- **Nicht-Plain-JSON:**  
  Das Objekt muss ein einfaches JSON sein. Methoden, Prototypen oder Metadaten
  führen zu Fehlern.

- **Fehlende Felder:**  
  Nur die im JSON übergebenen Felder werden aktualisiert. Nicht übergebene
  Felder bleiben unverändert.

---

## Best Practices

- Immer mit `getCleanJson` arbeiten, um saubere Daten zu speichern.
- Fehlerbehandlung einbauen, um Probleme frühzeitig zu erkennen.
- Die Methodenstruktur (`getColIdFromNote`, `getSingleDocRef`) sorgt für
  Übersichtlichkeit und Wiederverwendbarkeit.

---

## Siehe auch

- [Firebase Doku zu updateDoc](https://firebase.google.com/docs/reference/js/firestore_.updatedoc)
- Eigene Methoden:
  - `getSingleDocRef`: Erzeugt Dokument-Referenz
  - `getColIdFromNote`: Bestimmt Collection
  - `getCleanJson`: Wandelt Notiz in plain JSON um
