
# Angular NgForm – Template Formulare & Validierung

Angulars `NgForm`-Direktive macht das Arbeiten mit Formularen einfach und mächtig: Sie verwaltet den Status eines Formulars und ermöglicht direkte Bindung im Template sowie Zugriff auf Validierungen im TypeScript-Code.

---

## Inhaltsverzeichnis

- [Import & Setup](#import--setup)
- [Verwendung – NgForm im Template](#verwendung--ngform-im-template)
- [Eigenschaften der Validierung](#eigenschaften-der-validierung)
- [Validierung im Template](#validierung-im-template)
- [Validierung in TypeScript nutzen](#validierung-in-typescript-nutzen)
- [Beispiel: Ein einfaches Login-Formular](#beispiel-ein-einfaches-login-formular)
- [Weitere Hinweise](#weitere-hinweise)
- [Quellen](#quellen)

---

## Import & Setup

Um Template Formulare in Angular zu verwenden, muss das Modul importiert werden:

```typescript
import { FormsModule } from '@angular/forms';
```

In deiner AppModule:

```typescript
@NgModule({
  imports: [FormsModule]
})
```

---

## Verwendung – NgForm im Template

Das Template Driven Form wird mit `<form>` und `ngForm` gebaut. Einzelne Felder werden mit `ngModel` gebunden:

```html
<form #loginForm="ngForm" (ngSubmit)="onSubmit(loginForm)">
  <input name="username" ngModel required minlength="3" #username="ngModel" />
  <input type="password" name="password" ngModel required #password="ngModel" />
  <button [disabled]="loginForm.invalid">Login</button>
</form>
```

---

## Eigenschaften der Validierung

Angular bietet zahlreiche Validierungsattribute – die meisten sind direkt im Template nutzbar und über die Template-Referenz im Code abfragbar:

- **required**: Muss ausgefüllt werden.
- **minlength, maxlength**: Mindest-/Maximallänge.
- **pattern**: Regulärer Ausdruck für Eingabe.
- **email**: Email-Format.
- **Validators**: Eigene Validatoren möglich (z.B. im TS).

Status-Properties für Felder (über die Template-Referenz, z.B. `#username="ngModel"`):

| Property    | Bedeutung                      |
|-------------|-------------------------------|
| valid       | Feld ist valide                |
| invalid     | Feld ist nicht valide          |
| touched     | Feld wurde berührt             |
| untouched   | Feld wurde nicht berührt       |
| dirty       | Wert wurde verändert           |
| pristine    | Wert ist unverändert           |
| errors      | Enthält Fehlerobjekt, falls Feld ungültig |

---

## Validierung im Template

Fehlermeldungen und Status werden direkt im Template angezeigt:

```html
<input name="mail" ngModel required email #mail="ngModel" />
<div *ngIf="mail.errors?.required && mail.touched">
  E-Mail ist erforderlich!
</div>
<div *ngIf="mail.errors?.email && mail.touched">
  Ungültige E-Mail-Adresse!
</div>
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

## Beispiel: Ein einfaches Login-Formular

```typescript
@Component({
  selector: "app-login",
  template: `
    <form #loginForm="ngForm" (ngSubmit)="onSubmit(loginForm)">
      <input name="username" ngModel required minlength="3" #username="ngModel" />
      <div *ngIf="username.errors?.required && username.touched">
        Benutzername erforderlich!
      </div>
      <div *ngIf="username.errors?.minlength && username.dirty">
        Mindestens 3 Zeichen!
      </div>
      <input type="password" name="password" ngModel required #password="ngModel" />
      <div *ngIf="password.errors?.required && password.touched">
        Passwort erforderlich!
      </div>
      <button [disabled]="loginForm.invalid">Login</button>
    </form>
  `
})
export class LoginComponent {
  onSubmit(form: NgForm) {
    if (form.valid) {
      // Verarbeitung z.B. Backend-Login
    }
  }
}
```

---

## Weitere Hinweise

- Mit Angular kann man Custom Validators für komplexe Anforderungen im TypeScript schreiben.
- Template Driven Forms eignen sich für kleine bis mittlere Formulare. Für komplexe Validierung und dynamische Felder empfiehlt Angular Reactive Forms.
- Der Status von Formularen und Feldern (valid, touched, errors etc.) kann sowohl im Template als auch im Code abgefragt und verarbeitet werden.

---

## Quellen

- [Angular Dokumentation: Template-driven forms](https://angular.io/guide/forms)
- [API Docs: NgForm](https://angular.io/api/forms/NgForm)
- [Angular Form Validation](https://angular.io/guide/form-validation)
```

Füge diesen Block 1:1 in deine gewünschte Datei ein!