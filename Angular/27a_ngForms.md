# NgForm – Template Formulare & Validierung

Angulars `NgForm`-Direktive macht das Arbeiten mit Formularen einfach und
mächtig: Sie verwaltet den Status eines Formulars und ermöglicht direkte Bindung
im Template sowie Zugriff auf Validierungen im TypeScript-Code.

---

## Inhaltsverzeichnis

- [NgForm – Template Formulare \& Validierung](#ngform--template-formulare--validierung)
  - [Inhaltsverzeichnis](#inhaltsverzeichnis)
  - [Import \& Setup](#import--setup)
  - [Verwendung – NgForm im Template](#verwendung--ngform-im-template)
    - [Erklärung: ngModel, Template-Referenz und Two-Way-Binding](#erklärung-ngmodel-template-referenz-und-two-way-binding)
      - [Warum `ngModel` und `#username="ngModel"`?](#warum-ngmodel-und-usernamengmodel)
      - [Warum wird hier kein Two-Way-Binding (`[(ngModel)]`) verwendet?](#warum-wird-hier-kein-two-way-binding-ngmodel-verwendet)
      - [Beispiel mit Two-Way-Binding](#beispiel-mit-two-way-binding)
      - [Wann welches Muster?](#wann-welches-muster)
  - [Eigenschaften der Validierung](#eigenschaften-der-validierung)
  - [Validierung im Template](#validierung-im-template)
  - [Validierung in TypeScript nutzen](#validierung-in-typescript-nutzen)
  - [Validierung in beliebigen TypeScript-Funktionen verwenden](#validierung-in-beliebigen-typescript-funktionen-verwenden)
    - [Woher kommt der Parameter `loginForm`?](#woher-kommt-der-parameter-loginform)
  - [Beispiel: Ein einfaches Login-Formular](#beispiel-ein-einfaches-login-formular)
  - [Weitere Hinweise](#weitere-hinweise)
  - [Quellen](#quellen)

---

## Import & Setup

Um Template Formulare in Angular 17+ zu verwenden, muss das Modul importiert
werden:

```typescript
import { FormsModule } from "@angular/forms";
```

**Hinweis:**  
Ab Angular 17+ werden Standalone-Komponenten bevorzugt. Der Import von
`FormsModule` erfolgt direkt im `imports`-Array der Komponente oder im
`bootstrapApplication`-Setup. Ein klassisches `NgModule` ist nicht mehr zwingend
erforderlich.

**Beispiel für Standalone-Komponenten:**

```typescript
import { Component } from "@angular/core";
import { FormsModule } from "@angular/forms";

@Component({
  selector: "app-login",
  standalone: true,
  imports: [FormsModule],
  template: ` <!-- Dein Template --> `,
})
export class LoginComponent {}
```

---

## Verwendung – NgForm im Template

Das Template Driven Form wird mit `<form>` und `ngForm` gebaut. Einzelne Felder
werden mit `ngModel` gebunden:

```html
<form #loginForm="ngForm" (ngSubmit)="onSubmit(loginForm)">
  <input name="username" ngModel required minlength="3" #username="ngModel" />
  <input type="password" name="password" ngModel required #password="ngModel" />
  <button [disabled]="loginForm.invalid">Login</button>
</form>
```

### Erklärung: ngModel, Template-Referenz und Two-Way-Binding

#### Warum `ngModel` und `#username="ngModel"`?

- **`ngModel`**: Bindet das Eingabefeld an das Formularmodell von Angular. Es
  sorgt dafür, dass Angular den Wert und Status des Feldes verwaltet und
  validiert.
- **`#username="ngModel"`**: Erstellt eine Template-Referenzvariable namens
  `username`, die auf die Instanz des `NgModel`-Direktivs verweist. Damit kann
  man im Template auf Status und Fehler des Feldes zugreifen, z.B.
  `username.errors`, `username.valid`, `username.touched` usw.

**Ohne `#username="ngModel"`** könntest du im Template nicht direkt auf den
Status des Feldes zugreifen, sondern nur auf das gesamte Formular.

#### Warum wird hier kein Two-Way-Binding (`[(ngModel)]`) verwendet?

- In diesem Beispiel wird kein explizites Datenmodell im TypeScript benötigt.
  Die Werte werden erst beim Absenden (`onSubmit`) gesammelt und verarbeitet.
- Das einfache `ngModel` reicht aus, wenn du nur Validierung und Formularstatus
  im Template brauchst.

#### Beispiel mit Two-Way-Binding

Wenn du die Werte der Felder direkt an Variablen im TypeScript binden möchtest
(z.B. für Live-Feedback oder sofortige Verarbeitung), verwendest du das
Two-Way-Binding:

```html
<form #loginForm="ngForm" (ngSubmit)="onSubmit(loginForm)">
  <input
    name="username"
    [(ngModel)]="username"
    required
    minlength="3"
    #usernameCtrl="ngModel"
  />
  <input
    type="password"
    name="password"
    [(ngModel)]="password"
    required
    #passwordCtrl="ngModel"
  />
  <button [disabled]="loginForm.invalid">Login</button>
</form>
```

```typescript
export class LoginComponent {
  username: string = "";
  password: string = "";

  onSubmit(form: NgForm) {
    // Zugriff auf this.username und this.password direkt möglich
  }
}
```

#### Wann welches Muster?

| Muster                        | Vorteil                                                                  | Wann verwenden?                              |
| ----------------------------- | ------------------------------------------------------------------------ | -------------------------------------------- |
| `ngModel` + Template-Referenz | Einfach, wenn nur Validierung/Status im Template benötigt werden         | Wenn du kein explizites Datenmodell brauchst |
| `[(ngModel)]` (Two-Way)       | Direkter Sync zwischen Feld und Komponenten-Property (Live-Updates etc.) | Wenn du Werte sofort im TypeScript benötigst |

**Fazit:**

- Nutze `ngModel` und Template-Referenzen, wenn du nur Validierung und Status im
  Template brauchst.
- Nutze `[(ngModel)]`, wenn du die Werte der Felder direkt im TypeScript-Code
  weiterverarbeiten oder beobachten möchtest (z.B. für Live-Feedback, dynamische
  UI, etc.).

---

## Eigenschaften der Validierung

Angular bietet zahlreiche Validierungsattribute – die meisten sind direkt im
Template nutzbar und über die Template-Referenz im Code abfragbar:

- **required**: Muss ausgefüllt werden.
- **minlength, maxlength**: Mindest-/Maximallänge.
- **pattern**: Regulärer Ausdruck für Eingabe.
- **email**: Email-Format.
- **Validators**: Eigene Validatoren möglich (z.B. im TS).

Status-Properties für Felder (über die Template-Referenz, z.B.
`#username="ngModel"`):

| Property  | Bedeutung                                 |
| --------- | ----------------------------------------- |
| valid     | Feld ist valide                           |
| invalid   | Feld ist nicht valide                     |
| touched   | Feld wurde berührt                        |
| untouched | Feld wurde nicht berührt                  |
| dirty     | Wert wurde verändert                      |
| pristine  | Wert ist unverändert                      |
| errors    | Enthält Fehlerobjekt, falls Feld ungültig |

---

## Validierung im Template

Fehlermeldungen und Status werden direkt im Template angezeigt.  
Ab Angular 17+ wird die neue Block-Syntax mit `@if` statt `*ngIf` empfohlen:

```html
<input name="mail" ngModel required email #mail="ngModel" />
@if (mail.errors?.required && mail.touched) {
<div>E-Mail ist erforderlich!</div>
} @if (mail.errors?.email && mail.touched) {
<div>Ungültige E-Mail-Adresse!</div>
}
```

Der Button bleibt deaktiviert, bis das Formular gültig ist:

```html
<button [disabled]="loginForm.invalid">Absenden</button>
```

---

## Validierung in TypeScript nutzen

Die Validierung kann auch im TypeScript (z.B. im Submit-Handler) geprüft werden:

```typescript
onSubmit(form: NgForm) {
  // Zugriff auf den gesamten Formstatus
  if (form.valid) {
    // Alle Felder sind valide
    console.log("Formularwerte:", form.value);
  } else {
    // Zugriff auf Detail-Status, z.B. einzelnes Feld:
    const usernameField = form.controls['username'];
    if (usernameField?.errors?.minlength) {
      // Reagiere auf Fehler, z.B. spezielle Logik oder Logging
    }
  }
}
```

Man kann jedes Feld und dessen Fehler prüfen:

- `form.controls['username'].valid`
- `form.controls['username'].errors`
- `form.controls['username'].touched`

---

## Validierung in beliebigen TypeScript-Funktionen verwenden

Du kannst den Status und die Fehler des Formulars oder einzelner Felder
jederzeit in beliebigen Funktionen im TypeScript abfragen – nicht nur im
Submit-Handler. Dazu übergibst du das `NgForm`-Objekt oder einzelne Controls an
deine Funktion.

**Beispiel:**

```typescript
import { NgForm } from '@angular/forms';

checkUsername(form: NgForm) {
  const usernameField = form.controls['username'];
  if (usernameField?.invalid && usernameField?.errors?.minlength) {
    // Hier kannst du beliebige Logik ausführen, z.B. Logging oder UI-Feedback
    console.log('Benutzername zu kurz!');
  }
}

// Funktion kann z.B. im Template aufgerufen werden:
<button (click)="checkUsername(loginForm)">Prüfe Benutzername</button>
```

### Woher kommt der Parameter `loginForm`?

Im Template wird das Formular mit einer Template-Referenz versehen:

```html
<form #loginForm="ngForm" ...></form>
```

Dadurch steht die Variable `loginForm` im Template zur Verfügung und
referenziert das aktuelle `NgForm`-Objekt.  
Diese Referenz kann dann z.B. an Methoden im TypeScript übergeben werden:

```html
<button (click)="checkUsername(loginForm)">Prüfe Benutzername</button>
```

Im TypeScript erhältst du so das komplette Formularobjekt und kannst dessen
Status und Werte prüfen.

---

## Beispiel: Ein einfaches Login-Formular

```typescript
import { Component } from "@angular/core";
import { FormsModule, NgForm } from "@angular/forms";

@Component({
  selector: "app-login",
  standalone: true,
  imports: [FormsModule],
  template: `
    <form #loginForm="ngForm" (ngSubmit)="onSubmit(loginForm)">
      <input
        name="username"
        ngModel
        required
        minlength="3"
        #username="ngModel"
      />
      @if (username.errors?.required && username.touched) {
        <div>Benutzername erforderlich!</div>
      }
      @if (username.errors?.minlength && username.dirty) {
        <div>Mindestens 3 Zeichen!</div>
      }
      <input
        type="password"
        name="password"
        ngModel
        required
        #password="ngModel"
      />
      @if (password.errors?.required && password.touched) {
        <div>Passwort erforderlich!</div>
      }
      <button [disabled]="loginForm.invalid">Login</button>
    </form>
  `,
})
export class LoginComponent {
  onSubmit(form: NgForm) {
    if (form.valid) {
      // Verarbeitung z.B. Backend-Login
    }
  }

  checkUsername(form: NgForm) {
    const usernameField = form.controls["username"];
    if (usernameField?.invalid && usernameField?.errors?.minlength) {
      // Hier kannst du beliebige Logik ausführen, z.B. Logging oder UI-Feedback
      console.log("Benutzername zu kurz!");
    }
  }
}
```

---

## Weitere Hinweise

- Mit Angular kann man Custom Validators für komplexe Anforderungen im
  TypeScript schreiben.
- Template Driven Forms eignen sich für kleine bis mittlere Formulare. Für
  komplexe Validierung und dynamische Felder empfiehlt Angular Reactive Forms.
- Der Status von Formularen und Feldern (valid, touched, errors etc.) kann
  sowohl im Template als auch im Code abgefragt und verarbeitet werden.
- Die neue Block-Syntax (`@if`, `@for`) ist ab Angular 17 Standard und ersetzt
  die Stern-Syntax (`*ngIf`, `*ngFor`) in neuen Projekten.

---

## Quellen

- [Angular Dokumentation: Template-driven forms](https://angular.io/guide/forms)
- [API Docs: NgForm](https://angular.io/api/forms/NgForm)
- [Angular Form Validation](https://angular.io/guide/form-validation)
