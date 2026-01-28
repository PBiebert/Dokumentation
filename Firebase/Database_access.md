[Zurück zur Übersicht](../README.md)

# Datenbankzugriff mit Angular und Firebase

## 1. Firestore-Service anlegen

1. Erstelle einen neuen Service für den Datenbankzugriff:
   ```bash
   ng generate service firebase-services/note-list
   ```
2. Öffne die Datei `note-list.service.ts` und importiere die benötigten
   Firebase-Module:
   ```typescript
   import { inject, Injectable } from "@angular/core";
   import { collection, doc, Firestore } from "@angular/fire/firestore";
   ```
3. Integriere den Firestore-Service in die Klasse:
   ```typescript
   @Injectable({
     providedIn: "root",
   })
   export class NoteListService {
     firestore: Firestore = inject(Firestore);
     // ...
   }
   ```

## 2. Referenzen auf Collections und Dokumente erstellen

1. Um auf eine Collection (z.B. `notes`) zuzugreifen:
   ```typescript
   getNotesRef() {
     return collection(this.firestore, 'notes');
   }
   ```
2. Um auf eine weitere Collection (z.B. `trash`) zuzugreifen:
   ```typescript
   getTrashRef() {
     return collection(this.firestore, 'trash');
   }
   ```
3. Um auf ein einzelnes Dokument innerhalb einer Collection zuzugreifen:
   ```typescript
   getSingleDocRef(collId: string, docID: string) {
     return doc(collection(this.firestore, collId), docID);
   }
   ```

## 2.1 Erklärung: `collection()` und `doc()`

### `collection(firestore, collectionPath)`

- Erstellt eine Referenz auf eine Collection in Firestore.
- **Parameter:**
  - `firestore`: Die Firestore-Instanz (z.B. `this.firestore`).
  - `collectionPath`: Der Name oder Pfad der Collection als String (z.B. `'notes'` oder `'users/123/notes'`).
- **Beispiel:**
  ```typescript
  collection(this.firestore, 'notes');
  ```

### `doc(collectionRef, documentId)`

- Erstellt eine Referenz auf ein einzelnes Dokument innerhalb einer Collection.
- **Parameter:**
  - `collectionRef`: Die Collection-Referenz, z.B. von `collection()`.
  - `documentId`: Die ID des Dokuments als String.
- **Beispiel:**
  ```typescript
  doc(collection(this.firestore, 'notes'), 'abc123');
  ```

## 3. Hinweise zur Nutzung

- Die Methoden geben Referenzen zurück, die für Lese- und Schreiboperationen mit
  AngularFire genutzt werden können.
- Weitere Informationen zur Verwendung von AngularFire findest du in der
  [offiziellen Dokumentation](https://github.com/angular/angularfire).
