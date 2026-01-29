[Zurück zur Übersicht](../README.md)

# Dokumente mit `deleteDoc` in Firebase löschen

## Zweck

Die Funktion `deleteDoc` aus der Firebase Firestore-Bibliothek dient dazu, ein
bestimmtes Dokument aus einer Collection dauerhaft zu entfernen. In deinem
Projekt wird diese Methode verwendet, um Notizen (`Note`) entweder aus der
normalen Sammlung (`notes`) oder aus dem Papierkorb (`trash`) zu löschen.

---

## Funktionsweise im Projekt

Wenn ein Nutzer eine Notiz löschen möchte (z.B. aus dem Papierkorb oder direkt
aus der Notiz-Liste), wird die Methode `deleteNote` im Service `NoteListService`
aufgerufen. Diese Methode sorgt dafür, dass das entsprechende Dokument aus
Firestore entfernt wird.

### Beispiel: deleteNote

```typescript
async deleteNote(colId: 'notes' | 'trash', docId: string) {
  try {
    await deleteDoc(this.getSingleDocRef(colId, docId));
  } catch (error) {
    console.log('Error during deleteNote: ', error);
  }
}
```

**Ablauf:**

1. Die Methode erhält die Collection (`notes` oder `trash`) und die
   Dokumenten-ID.
2. Mit `getSingleDocRef` wird eine Referenz auf das konkrete Dokument erzeugt.
3. `deleteDoc` löscht das Dokument in Firestore.
4. Fehler werden im Catch-Block geloggt.

---

## Beispiel: Notiz in den Papierkorb verschieben

Um eine Notiz in den Papierkorb zu verschieben, wird sie zunächst in die
Collection `trash` kopiert und anschließend aus der Collection `notes` gelöscht.
Das Löschen erfolgt dabei mit `deleteDoc`.

```typescript
moveToTrash() {
  if (this.note.id) {
    this.note.type = 'trash';
    let docId = this.note.id; // Speichert die alte ID zwischen
    delete this.note.id; // Entfernt die ID aus dem Objekt, damit Firestore beim Hinzufügen eine neue ID vergibt
    this.noteService.addNote(this.note, 'trash'); // Fügt die Notiz im Papierkorb hinzu (neue ID)
    this.noteService.deleteNote('notes', docId); // Löscht die Notiz aus der ursprünglichen Sammlung
  }
}
```

**Kommentar:**

- Die Notiz wird nicht direkt verschoben, sondern kopiert und dann gelöscht.
- Die ID wird entfernt, damit Firestore beim Hinzufügen im Papierkorb eine neue
  ID vergibt.
- Das Löschen aus der ursprünglichen Collection erfolgt mit `deleteDoc` über die
  Methode `deleteNote`.

---

## Zentrale Methoden im Detail

### getSingleDocRef

Erstellt eine gültige Referenz auf ein Dokument in einer Collection.  
**Wichtig:** Die Referenz muss exakt zwei Segmente haben: Collection-Name und
Dokument-ID.

```typescript
getSingleDocRef(colId: string, docId: string) {
  // Erstellt eine Dokument-Referenz wie z.B. "notes/abc123"
  return doc(collection(this.firestore, colId), docId);
}
```

---

## Typischer Ablauf im UI

1. **Löschen:** Nutzer klickt auf "Löschen" bei einer Notiz.
2. **Service:** Die Methode `deleteNote()` im Service wird mit Collection und ID
   aufgerufen.
3. **Firestore:** Das Dokument wird aus der Datenbank entfernt.

**Papierkorb-Funktion:**  
Beim Verschieben in den Papierkorb wird die Notiz zuerst in die Collection
`trash` eingefügt und danach aus `notes` gelöscht.

---

## Fehlerquellen und Troubleshooting

- **Ungültige Referenz:**  
  Die Dokumenten-Referenz muss exakt zwei Segmente haben (`collection/docId`).  
  Fehler wie  
  `FirebaseError: Invalid collection reference. Collection references must have an odd number of segments...`  
  deuten auf eine falsche Referenzerstellung hin.

- **Fehlende ID:**  
  Das zu löschende Objekt muss eine gültige `id` besitzen, sonst kann Firestore
  das Dokument nicht finden.

- **Berechtigungen:**  
  Stelle sicher, dass die Firestore-Regeln das Löschen erlauben.

---

## Best Practices

- Immer mit `getSingleDocRef` arbeiten, um saubere Referenzen zu erzeugen.
- Fehlerbehandlung einbauen, um Probleme frühzeitig zu erkennen.
- Die Methodenstruktur sorgt für Übersichtlichkeit und Wiederverwendbarkeit.
- Beim Verschieben in den Papierkorb:
  - Die ID vor dem Kopieren zwischenspeichern und aus dem Objekt entfernen,
    damit Firestore eine neue ID vergibt.
  - Erst kopieren, dann löschen, um Datenverlust zu vermeiden.

---

## Siehe auch

- [Firebase Doku zu deleteDoc](https://firebase.google.com/docs/reference/js/firestore_.deletedoc)
- Eigene Methoden:
  - `getSingleDocRef`: Erzeugt Dokument-Referenz
  - `deleteNote`: Löscht ein Dokument aus Firestore
