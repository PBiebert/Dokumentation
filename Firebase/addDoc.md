[Zurück zur Übersicht](../README.md)

# Dokumente zu Firestore mit `addDoc` hinzufügen in Angular

Diese Anleitung zeigt dir, wie du mit Angular und Firebase Firestore neue
Dokumente in eine Collection einfügst.

## Inhaltsverzeichnis

1. [Firestore-Service vorbereiten](#1-firestore-service-vorbereiten)
2. [Methode zum Hinzufügen (`addDoc`) implementieren](#2-methode-zum-hinzufügen-adddoc-implementieren)
3. [Dokumente aus Angular-Komponenten hinzufügen](#3-dokumente-aus-angular-komponenten-hinzufügen)
4. [Hinweise und Fehlerquellen](#4-hinweise-und-fehlerquellen)
5. [Tipp](#tipp)

---

## 1. Firestore-Service vorbereiten

1. Erstelle einen Service (falls noch nicht vorhanden):
   ```bash
   ng generate service firebase-services/note-list
   ```
2. Importiere die benötigten Firebase-Module:
   ```typescript
   import { inject, Injectable } from "@angular/core";
   import { addDoc, collection, Firestore } from "@angular/fire/firestore";
   ```

---

## 2. Methode zum Hinzufügen (`addDoc`) implementieren

Im Service wird eine Methode definiert, die ein neues Dokument in die gewünschte
Collection einfügt.

```typescript
@Injectable({
  providedIn: "root",
})
export class NoteListService {
  firestore: Firestore = inject(Firestore);

  // Methode zum Hinzufügen eines Dokuments
  async addNote(note: Note) {
    try {
      // Referenz auf die Collection 'notes'
      const docRef = await addDoc(this.getNotesRef(), note);
      console.log("Dokument hinzugefügt mit ID:", docRef.id);
    } catch (error) {
      console.error("Fehler beim Hinzufügen:", error);
    }
  }

  getNotesRef() {
    return collection(this.firestore, "notes");
  }
}
```

**Erklärung:**

- `addDoc(collectionRef, data)` fügt ein neues Dokument mit den angegebenen
  Daten in die Collection ein.
- Die Methode ist asynchron (`async`) und gibt eine Referenz auf das neue
  Dokument zurück.
- Fehler werden im `catch`-Block behandelt.

---

## 3. Dokumente aus Angular-Komponenten hinzufügen

In der Komponente wird die Methode des Services aufgerufen, z.B. beim Klick auf
einen Button.

```typescript
import { Component } from "@angular/core";
import { NoteListService } from "../firebase-services/note-list.service";
import { Note } from "../interfaces/note.interface";

@Component({
  selector: "app-add-note-dialog",
  // ...templateUrl, styleUrl...
})
export class AddNoteDialogComponent {
  title = "";
  description = "";
  constructor(private noteService: NoteListService) {}

  addNote() {
    const note: Note = {
      type: "note",
      title: this.title,
      content: this.description,
      marked: false,
    };
    this.noteService.addNote(note);
  }
}
```

**Erklärung:**

- Die Komponente sammelt die Eingabedaten und ruft die Service-Methode auf.
- Das neue Dokument wird in Firestore gespeichert.

---

## 4. Hinweise und Fehlerquellen

- **Collection-Referenz:** Stelle sicher, dass die Collection existiert oder
  automatisch angelegt wird.
- **Datenstruktur:** Die Daten sollten dem erwarteten Interface entsprechen.
- **Fehlerbehandlung:** Nutze `try/catch`, um Fehler beim Hinzufügen abzufangen.
- **Asynchronität:** Die Methode ist asynchron – ggf. auf Rückgabe/Status
  achten.

---

## 5. Tipp

Mit `addDoc` kannst du beliebige Datenstrukturen speichern. Die ID des neuen
Dokuments wird automatisch generiert und kann über `docRef.id` abgerufen werden.

---

Weitere Infos:  
[AngularFire Dokumentation zu addDoc](https://github.com/angular/angularfire)
