[Zurück zur Übersicht](../README.md)

# Firestore-Datenbankzugriff und -Anzeige in Angular

Diese Anleitung zeigt Schritt für Schritt, wie du mit Angular und Firebase Firestore arbeitest:

- **1. Firestore-Service anlegen**
- **2. Referenzen auf Collections und Dokumente erstellen**
- **3. Firestore-Daten in Angular anzeigen** (mit `collectionData()` oder `onSnapshot`)

---

## Was ist Firestore?

Firestore ist eine NoSQL-Cloud-Datenbank von Google, die speziell für die Entwicklung moderner Web- und Mobile-Apps entwickelt wurde.  
Sie speichert Daten in **Collections** (Sammlungen) und **Documents** (Dokumenten).  
Jedes Dokument ist ein JSON-ähnliches Objekt mit Feldern und Werten.

**Vorteile:**
- Echtzeit-Synchronisation: Änderungen werden sofort an alle Clients übertragen.
- Skalierbarkeit: Für kleine und große Projekte geeignet.
- Flexible Datenstruktur: Keine festen Schemata.

---

## Was ist AngularFire?

[AngularFire](https://github.com/angular/angularfire) ist die offizielle Angular-Bibliothek für Firebase.  
Sie bietet eine einfache Integration von Firestore, Auth, Storage und weiteren Firebase-Diensten in Angular-Projekte.

**Wichtige Features:**
- RxJS-Observables für reaktive Datenströme
- Automatisches Unsubscribe im Template mit der `async`-Pipe
- Einfache API für CRUD-Operationen

---

## 1. Firestore-Service anlegen

**Warum ein Service?**  
In Angular werden Datenzugriffe und Logik in Services ausgelagert, um Komponenten schlank und testbar zu halten.

1. Erstelle einen neuen Service für den Datenbankzugriff:
   ```bash
   ng generate service firebase-services/note-list
   ```
2. Öffne die Datei `note-list.service.ts` und importiere die benötigten Firebase-Module:
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

> **Tipp:**  
> Mit `providedIn: "root"` ist der Service im gesamten Projekt verfügbar (Singleton).

---

## 2. Referenzen auf Collections und Dokumente erstellen

### 2.1 Referenz auf eine Collection

Um auf eine Collection (z.B. `notes`) zuzugreifen:

```typescript
getNotesRef() {
  return collection(this.firestore, 'notes');
}
```

Um auf eine weitere Collection (z.B. `trash`) zuzugreifen:

```typescript
getTrashRef() {
  return collection(this.firestore, 'trash');
}
```

**Hintergrund:**  
Eine Collection ist eine Sammlung von Dokumenten.  
Mit `collection(firestore, 'notes')` erzeugst du eine Referenz, die du für Lese- und Schreiboperationen verwenden kannst.

### 2.2 Referenz auf ein einzelnes Dokument

Um auf ein einzelnes Dokument innerhalb einer Collection zuzugreifen:

```typescript
getSingleDocRef(collId: string, docID: string) {
  return doc(collection(this.firestore, collId), docID);
}
```

**Hintergrund:**  
Jedes Dokument hat eine eindeutige ID.  
Mit `doc(collectionRef, documentId)` kannst du gezielt ein Dokument ansprechen, z.B. für Detailansichten oder Updates.

#### Erklärung: `collection()` und `doc()`

- **`collection(firestore, collectionPath)`**  
  Erstellt eine Referenz auf eine Collection in Firestore.  
  Beispiel:
  ```typescript
  collection(this.firestore, "notes");
  ```

- **`doc(collectionRef, documentId)`**  
  Erstellt eine Referenz auf ein einzelnes Dokument innerhalb einer Collection.  
  Beispiel:
  ```typescript
  doc(collection(this.firestore, "notes"), "abc123");
  ```

> **Best Practice:**  
> Halte alle Referenz-Methoden im Service, damit du sie zentral verwalten und wiederverwenden kannst.

---

## 3. Firestore-Daten in Angular anzeigen

In Angular kannst du Firestore-Daten auf zwei grundlegende Arten anzeigen:

- Mit Observables und der `collectionData()`-Methode (empfohlen für Templates mit `async`-Pipe)
- Mit Firestore-Listenern über `onSnapshot` und lokalen Arrays (mehr Kontrolle, z.B. für komplexe Logik)

Im Folgenden werden beide Wege ausführlich erklärt.

---

### Variante 1: Daten mit `collectionData()` und Observable anzeigen

> **Was ist ein Observable?**  
> Ein **Observable** ist ein Datenstrom, der Werte liefert, sobald sie verfügbar sind.  
> In Angular werden Observables häufig für asynchrone Daten (z.B. HTTP, Firestore) verwendet.  
> Die `async`-Pipe im Template übernimmt das Abonnieren und Abmelden automatisch.

#### Was passiert hier?

- Du nutzt ein **Observable**, das automatisch alle Änderungen aus Firestore empfängt.
- Das Observable wird im Service bereitgestellt und im Template mit der `async`-Pipe abonniert.
- Angular kümmert sich um das automatische Abmelden vom Datenstrom.

#### Service

```typescript
import { inject, Injectable } from "@angular/core";
import { collection, collectionData, Firestore } from "@angular/fire/firestore";

@Injectable({
  providedIn: "root",
})
export class NoteListService {
  firestore: Firestore = inject(Firestore);
  // Das Observable liefert alle Notizen aus der Collection 'notes'
  items$ = collectionData(this.getNotesRef(), { idField: "id" });

  getNotesRef() {
    return collection(this.firestore, "notes");
  }
}
```

**Details:**  
- `collectionData(ref, { idField })` gibt ein Observable zurück, das die Daten der Collection als Array liefert.
- Mit `{ idField: "id" }` wird die Dokument-ID als Feld `id` im Objekt ergänzt.

#### Komponente

```typescript
import { Component } from "@angular/core";
import { NoteListService } from "../firebase-services/note-list.service";

@Component({
  selector: "app-note-list",
  // ...imports, templateUrl, styleUrl...
})
export class NoteListComponent {
  // Der Service wird als public deklariert, damit das Template darauf zugreifen kann
  constructor(public noteService: NoteListService) {}
}
```

#### Template

```html
<ul>
  @for (item of noteService.items$ | async; track item) {
  <li>{{ item.title }}</li>
  }
</ul>
```

**Erklärung:**
- `items$` ist ein Observable, das alle Notizen enthält.
- Mit `| async` wird das Observable im Template automatisch abonniert und aktualisiert.
- Änderungen in Firestore erscheinen sofort in der Liste.
- Kein manuelles Abmelden nötig – Angular übernimmt das.

> **Tiefere Einblicke:**  
> - Die `async`-Pipe sorgt dafür, dass das Observable korrekt abonniert und bei Bedarf wieder abgemeldet wird (z.B. beim Verlassen der Komponente).
> - Das Pattern ist besonders für einfache Listen und Standardfälle geeignet.

---

### Variante 2: Daten mit `onSnapshot` und lokalen Arrays anzeigen

> **Was ist ein Firestore-Listener?**  
> Mit `onSnapshot` kannst du einen Listener auf eine Collection oder ein Dokument setzen.  
> Bei jeder Änderung werden die neuen Daten sofort an deinen Callback geliefert.

#### Was passiert hier?

- Du nutzt Firestore-Listener (`onSnapshot`), um Datenänderungen direkt zu empfangen.
- Die Daten werden in lokale Arrays geschrieben.
- Du hast volle Kontrolle über das Datenhandling und kannst eigene Logik im Callback ausführen.
- Du musst die Listener selbst wieder entfernen, z.B. im `ngOnDestroy`.

#### Service

```typescript
import { inject, Injectable, OnDestroy } from "@angular/core";
import { collection, Firestore, onSnapshot } from "@angular/fire/firestore";
import { Note } from "../interfaces/note.interface";

@Injectable({
  providedIn: "root",
})
export class NoteListService implements OnDestroy {
  normalNotes: Note[] = [];
  trashNotes: Note[] = [];
  firestore: Firestore = inject(Firestore);

  unsubNotes;
  unsubTrash;

  constructor() {
    // Listener für Notizen und Papierkorb setzen
    this.unsubNotes = this.subNotesList();
    this.unsubTrash = this.subTrashList();
  }

  // Hilfsfunktion, um ein Firestore-Objekt in ein Note-Objekt zu verwandeln
  setNoteObject(obj: any, id: string): Note {
    return {
      id: id,
      type: obj.type || "note",
      title: obj.title || "",
      content: obj.content || "",
      marked: obj.marked || false,
    };
  }

  // Listener für normale Notizen
  subNotesList() {
    return onSnapshot(this.getNotesRef(), (list) => {
      this.normalNotes = [];
      list.forEach((element) => {
        this.normalNotes.push(this.setNoteObject(element.data(), element.id));
      });
    });
  }

  // Listener für Papierkorb-Notizen
  subTrashList() {
    return onSnapshot(this.getTrashRef(), (list) => {
      this.trashNotes = [];
      list.forEach((element) => {
        this.trashNotes.push(this.setNoteObject(element.data(), element.id));
      });
    });
  }

  getNotesRef() {
    return collection(this.firestore, "notes");
  }
  getTrashRef() {
    return collection(this.firestore, "trash");
  }

  // Wichtig: Listener beim Zerstören der Komponente entfernen!
  ngOnDestroy() {
    if (this.unsubNotes) this.unsubNotes();
    if (this.unsubTrash) this.unsubTrash();
  }
}
```

**Details:**  
- `onSnapshot(ref, callback)` setzt einen Listener auf die Collection.
- Der Rückgabewert ist eine Funktion, mit der du den Listener wieder entfernen kannst (unsubscriben).
- Die Daten werden in lokale Arrays geschrieben, die du beliebig weiterverarbeiten kannst (z.B. sortieren, filtern).

#### Komponente

```typescript
import { Component } from "@angular/core";
import { NoteListService } from "../firebase-services/note-list.service";

@Component({
  selector: "app-note-list",
  // ...imports, templateUrl, styleUrl...
})
export class NoteListComponent {
  constructor(public noteService: NoteListService) {}

  // Gibt das aktuelle Array der Notizen zurück
  getList() {
    return this.noteService.normalNotes;
  }
}
```

#### Template

```html
<ul>
  @for (note of getList(); track note) {
  <li>{{ note.title }}</li>
  }
</ul>
```

**Erklärung:**
- Die Daten werden in den Arrays `normalNotes` und `trashNotes` gehalten.
- Änderungen in Firestore werden durch die Listener sofort übernommen.
- Du kannst im Callback beliebige Logik ausführen (z.B. filtern, sortieren).
- Die Listener müssen im `ngOnDestroy` entfernt werden, um Speicherlecks zu vermeiden.

> **Tiefere Einblicke:**  
> - Mit `onSnapshot` kannst du auch einzelne Dokumente überwachen, nicht nur Collections.
> - Du bist für das Lifecycle-Management der Listener selbst verantwortlich.

---

## Unterschiede, Vorteile und Hinweise

| Methode              | Vorteile                                                                                             | Nachteile / Besonderheiten                                      |
| -------------------- | ---------------------------------------------------------------------------------------------------- | --------------------------------------------------------------- |
| **collectionData()** | - Sehr einfach für Templates<br>- Automatisches Unsubscribe<br>- Perfekt für Standardfälle           | - Weniger flexibel bei komplexer Logik<br>- Nur für Collections |
| **onSnapshot()**     | - Volle Kontrolle<br>- Eigene Logik im Callback<br>- Für Collections und einzelne Dokumente geeignet | - Manuelles Unsubscribe nötig<br>- Mehr Code/Aufwand            |

**Wann solltest du was verwenden?**

- Nutze `collectionData()`, wenn du einfach Daten im Template anzeigen willst und keine spezielle Logik brauchst.
- Nutze `onSnapshot()`, wenn du mehr Kontrolle, eigene Logik oder spezielle Firestore-Features benötigst.

---

## Best Practices und Tipps

- **Service-Struktur:** Halte alle Firestore-Operationen im Service, nicht in der Komponente.
- **Unsubscribe:** Bei Listenern (`onSnapshot`) immer im `ngOnDestroy` abmelden!
- **Fehlerbehandlung:** Fange Fehler ab, z.B. mit `.catch()` oder Error-Handlern.
- **Datenmodell:** Definiere Interfaces für deine Daten (z.B. `Note`), um Typsicherheit zu gewährleisten.
- **Performance:** Für große Datenmengen nutze Paginierung oder Abfragen mit Filtern.

---

**Tipp:**  
Beide Varianten können auch kombiniert werden, z.B. für einfache Listen mit `collectionData()` und für Spezialfälle mit `onSnapshot()`.

Weitere Infos:  
[AngularFire Dokumentation](https://github.com/angular/angularfire)
