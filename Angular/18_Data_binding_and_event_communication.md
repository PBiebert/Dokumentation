[Zurück zur Übersicht](../README.md)

# Datenbindung mit ngModel & Event-Kommunikation mit @Output

---

## Inhaltsverzeichnis

1. [Was ist Datenbindung in Angular?](#was-ist-datenbindung-in-angular)
2. [Was ist ngModel?](#was-ist-ngmodel)
3. [Was ist @Output und EventEmitter?](#was-ist-output-und-eventemitter)
4. [ToDo-Liste / Leitfaden für das Beispiel](#todo-liste--leitfaden-für-das-beispiel)
5. [Schritt 1: Interface für Post](#schritt-1-interface-für-post)
6. [Schritt 2: Eltern-Komponente (MainPage)](#schritt-2-eltern-komponente-mainpage)
7. [Schritt 3: Kind-Komponente (Post)](#schritt-3-kind-komponente-post)
8. [Schritt 4: Datenfluss & Event-Kommunikation](#schritt-4-datenfluss--event-kommunikation)
9. [Was passiert im Hintergrund?](#was-passiert-im-hintergrund)
10. [Typische Anwendungsfälle](#typische-anwendungsfälle)
11. [Typische Fehlerquellen](#typische-fehlerquellen)
12. [Zusammenfassung](#zusammenfassung)
13. [Fazit](#fazit)

---

## Was ist Datenbindung in Angular?

Datenbindung bedeutet, dass du Werte zwischen deiner Komponente (TypeScript) und
dem Template (HTML) synchronisieren kannst.  
Es gibt verschiedene Arten der Datenbindung:

- **One-Way Binding:** Daten fließen nur in eine Richtung (z.B. von Komponente
  zu Template).
- **Two-Way Binding:** Daten fließen in beide Richtungen (z.B. mit `ngModel`).

---

## Was ist ngModel?

Mit `ngModel` kannst du ein Eingabefeld direkt mit einer Variable in deiner
Komponente verbinden.  
Das nennt man **Two-Way Data Binding**.  
Das bedeutet:

- Wenn der Nutzer etwas ins Eingabefeld schreibt, wird der Wert sofort in der
  Variable gespeichert.
- Wenn du die Variable im Code änderst, wird der Wert auch im Eingabefeld
  angezeigt.

**Wichtig:**  
Damit `ngModel` funktioniert, musst du das `FormsModule` importieren!

---

## Was ist @Output und EventEmitter?

Mit `@Output` kannst du aus einer Kind-Komponente ein Event (z.B. einen
Button-Klick oder einen neuen Kommentar) an die Eltern-Komponente senden.  
Dafür nutzt du den `EventEmitter`.  
Die Eltern-Komponente kann das Event abfangen und darauf reagieren.

---

## ToDo-Liste / Leitfaden für das Beispiel

1. **Interface für Post anlegen**  
   Definiere die Struktur eines Posts mit Name, Bild, Likes, Like-Status und
   Kommentaren.

2. **Eltern-Komponente (MainPage) erstellen**
   - Enthält eine Liste von Posts.
   - Methode zum Hinzufügen eines Kommentars.

3. **Kind-Komponente (Post) erstellen**
   - Zeigt einen einzelnen Post an.
   - Eingabefeld für Kommentare mit `ngModel`.
   - Button zum Absenden des Kommentars.
   - Like-Funktion.

4. **Event-Kommunikation einrichten**
   - Kind-Komponente sendet Kommentar per `@Output` an Eltern-Komponente.
   - Eltern-Komponente fügt Kommentar zur richtigen Post-Liste hinzu.

5. **Template-Struktur**
   - Eltern-Komponente rendert alle Posts mit `@for`.
   - Kind-Komponente zeigt Post-Daten und Kommentare an.

---

## Schritt 1: Interface für Post

Das Interface legt fest, wie ein Post aufgebaut ist.  
So weiß TypeScript, welche Eigenschaften ein Post haben muss.

```typescript
// post.interface.ts
export interface PostInterface {
  name: string;
  imgPath: string;
  likes: number;
  isLiked: boolean;
  commits: string[];
}
```

---

## Schritt 2: Eltern-Komponente (MainPage)

Die Eltern-Komponente hält die Liste aller Posts.  
Sie ist dafür zuständig, Kommentare zu speichern und die Kind-Komponenten zu
rendern.

```typescript
// main-page.ts
export class MainPage {
  posts: PostInterface[] = [
    // ...Post-Daten...
  ];

  addComent(comment: string, index: number) {
    this.posts[index].commits.push(comment);
  }
}
```

**Template:**

```html
<!-- main-page.html -->
<section>
  @for (post of posts; track $index) {
  <app-post [post]="post" (newCommit)="addComent($event, $index)"></app-post>
  }
  <a routerLink="contact">Contact</a>
</section>
```

**Erklärung:**

- Mit `@for` wird für jeden Post eine Kind-Komponente erzeugt.
- `[post]="post"` übergibt die Post-Daten an die Kind-Komponente.
- `(newCommit)="addComent($event, $index)"` fängt das Event aus der
  Kind-Komponente ab und ruft die Methode zum Hinzufügen eines Kommentars auf.

---

## Schritt 3: Kind-Komponente (Post)

Die Kind-Komponente zeigt die Daten eines einzelnen Posts an und ermöglicht das
Liken und Kommentieren.

```typescript
// post.ts
export class Post {
  @Input() post!: PostInterface;
  inputField: string = "";
  @Output() newCommit = new EventEmitter<string>();

  like(post: PostInterface) {
    // Like/Unlike Logik
  }

  addNewComment() {
    if (this.inputField != "") {
      this.newCommit.emit(this.inputField);
      this.inputField = "";
    }
  }
}
```

**Template:**

```html
<!-- post.html -->
<div class="post-container">
  <p>{{ post.name }}</p>
  <div class="img-container">
    <img src="{{ post.imgPath }}" alt="" />
  </div>
  <img
    class="like"
    [src]="post.isLiked ? 'assets/img/heart-red.png' : 'assets/img/heart-black.png'"
    (click)="like(post)"
  />
  <p>{{ post.likes }}</p>
  <p>add a Comment</p>
  <input type="text" [(ngModel)]="inputField" />
  <button (click)="addNewComment()">Add a coment to parrent List</button>
  <ul>
    @for (commit of post.commits; track $index) {
    <li>{{ commit }}</li>
    }
  </ul>
</div>
```

**Erklärung:**

- `@Input() post` empfängt die Post-Daten von der Eltern-Komponente.
- `[(ngModel)]="inputField"` verbindet das Eingabefeld mit der Variable
  `inputField`.
- Beim Klick auf den Button wird `addNewComment()` ausgeführt und der Kommentar
  an die Eltern-Komponente geschickt.
- Die Kommentare werden mit `@for` als Liste angezeigt.

---

## Schritt 4: Datenfluss & Event-Kommunikation

**Wie funktioniert das Zusammenspiel?**

1. **Der Nutzer gibt einen Kommentar ein.**
2. **ngModel** sorgt dafür, dass der Wert direkt in `inputField` gespeichert
   wird.
3. **Button-Klick:** Die Methode `addNewComment()` wird ausgeführt.
4. **EventEmitter:** Der Kommentar wird mit
   `this.newCommit.emit(this.inputField)` an die Eltern-Komponente geschickt.
5. **Eltern-Komponente:** Fängt das Event ab und fügt den Kommentar zur
   richtigen Post-Liste hinzu.

---

## Was passiert im Hintergrund?

- **ngModel:**
  - Synchronisiert das Eingabefeld und die Variable.
  - Änderungen im Feld oder in der Variable sind immer aktuell.

- **@Output & EventEmitter:**
  - Die Kind-Komponente kann ein Event auslösen.
  - Die Eltern-Komponente kann darauf reagieren und z.B. einen Kommentar
    speichern.

- **@Input:**
  - Übergibt Daten von der Eltern- zur Kind-Komponente.

- **@for:**
  - Rendert dynamisch alle Posts und deren Kommentare.

---

## Typische Anwendungsfälle

- Formulareingaben an die Eltern-Komponente weitergeben (z.B. Kommentare,
  Bewertungen)
- Kommunikation von Kind- zu Eltern-Komponente (z.B. Button-Klicks, Änderungen)
- Dynamische Anzeige und Verarbeitung von Benutzereingaben

---

## Typische Fehlerquellen

- `FormsModule` muss in der Komponente importiert sein, sonst funktioniert
  `ngModel` nicht.
- Der Event-Name im Template muss mit dem `@Output`-Property übereinstimmen.
- Die Eltern-Komponente muss die Methode für das Event bereitstellen.
- Die Übergabe von Daten mit `@Input` und die Event-Kommunikation mit `@Output`
  müssen korrekt sein.

---

## Zusammenfassung

- **ngModel** verbindet Eingabefelder mit Variablen in der Komponente (Two-Way
  Binding).
- Mit **@Output** und **EventEmitter** sendest du Werte/Ereignisse von der Kind-
  zur Eltern-Komponente.
- Mit **@Input** empfängt die Kind-Komponente Daten von der Eltern-Komponente.
- Mit **@for** kannst du Listen dynamisch anzeigen.
- So kannst du Benutzereingaben einfach aufnehmen und weiterverarbeiten – ohne
  komplexe Formlogik.

---

## Fazit

Mit dieser Struktur kannst du Benutzereingaben einfach aufnehmen und an die
Eltern-Komponente weitergeben.  
Nutze die ToDo-Liste als Leitfaden, um ähnliche Features in eigenen Projekten
umzusetzen.

---
