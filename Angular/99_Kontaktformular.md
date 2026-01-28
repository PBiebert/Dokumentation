[Zurück zur Übersicht](../README.md)

# Kontaktformular in Angular – Schritt für Schritt

---

## Was ist ein Kontaktformular in Angular?

Ein Kontaktformular ermöglicht es Nutzern, direkt über die Website Kontakt
aufzunehmen.  
Mit Angular kannst du Formulare einfach erstellen, validieren und die Daten an
einen Server senden.

---

## Vorteile eines Angular-Kontaktformulars

- **Reaktive Validierung** und sofortiges Feedback für Nutzer
- **Einfache Datenbindung** mit `ngModel`
- **Flexible Fehlerbehandlung** und Benutzerführung
- **Integration mit Backend** (z.B. PHP, Node.js)
- **Saubere Trennung** von Template und Logik

---

## Typische Anwendungsfälle

- Kontaktaufnahme durch Kunden oder Interessenten
- Anfragen, Feedback oder Support direkt über die Website
- Automatisierte Weiterleitung von Nachrichten per E-Mail

---

## Beispiel: Kontaktformular-Template

```html
<!-- contact-me.html -->
<form (ngSubmit)="onSubmit(contactForm)" #contactForm="ngForm">
  <div class="input-container">
    <label
      for="input-name"
      [class.highlight-label-If-Invalid]="name.touched && name.invalid"
    >
      {{ name.touched && name.invalid && name.dirty ? 'Gib bitte min. 5 Zeichen
      ein' : 'Wie ist dein Name?' }}
    </label>
    <input
      [(ngModel)]="contactData.name"
      [class.invalid]="name.touched"
      name="input-name"
      id="input-name"
      type="text"
      minlength="5"
      [placeholder]="
        !name.touched ? 'Gib bitte deinen Namen ein ' : 'Hoppla! dein Name fehlt.'
      "
      required
      #name="ngModel"
    />
  </div>
  <!-- ...weitere Felder wie E-Mail, Nachricht, Datenschutz... -->
  <button
    type="submit"
    [disabled]="name.invalid || email.invalid || massage.invalid || privacyCheck.invalid"
  >
    Senden
  </button>
</form>
```

> **Hinweis:**  
> Die Validierung erfolgt direkt im Template mit Angular-Formular-Features
> (`ngModel`, `required`, `minlength` etc.).

---

## Beispiel: Component-Logik

```typescript
// contact-me.ts
import { Component, inject } from "@angular/core";
import { HttpClient } from "@angular/common/http";
import { NgForm } from "@angular/forms";

@Component({
  // ...Selector, Imports, Template, Style...
})
export class ContactMe {
  http = inject(HttpClient);

  contactData = {
    name: "",
    email: "",
    message: "",
    privacyCheck: false,
  };

  post = {
    endPoint: "https://deineDomain.de/sendMail.php",
    body: (payload: any) => JSON.stringify(payload),
    options: {
      headers: {
        "Content-Type": "text/plain",
        responseType: "text",
      },
    },
  };

  onSubmit(ngForm: NgForm) {
    if (ngForm.submitted && ngForm.form.valid) {
      this.http
        .post(this.post.endPoint, this.post.body(this.contactData))
        .subscribe({
          next: (response) => ngForm.resetForm(),
          error: (error) => console.error(error),
          complete: () => console.info("send post complete"),
        });
    }
  }
}
```

> **Tipp:**  
> Die Formularwerte werden über `contactData` gebunden und beim Absenden an das
> Backend gesendet.

---

## Erklärung der wichtigsten Template-Bindings am Beispiel des Namensfeldes

```html
<input
  [(ngModel)]="contactData.name"
  [class.invalid]="name.touched"
  name="input-name"
  id="input-name"
  type="text"
  minlength="5"
  [placeholder]="
    !name.touched ? 'Gib bitte deinen Namen ein ' : 'Hoppla! dein Name fehlt.'
  "
  required
  #name="ngModel"
/>
```

**Was passiert hier im Detail?**

- `[(ngModel)]="contactData.name"`  
  → Zwei-Wege-Datenbindung: Der Wert des Eingabefeldes wird automatisch mit der
  Eigenschaft `contactData.name` im Component-Code synchronisiert. Änderungen im
  Feld werden direkt im Objekt gespeichert und umgekehrt.

- `[class.invalid]="name.touched"`  
  → Dynamisches Hinzufügen der CSS-Klasse `invalid`, sobald das Feld vom Nutzer
  berührt wurde (`touched`). Damit kann man z.B. eine rote Umrandung anzeigen,
  wenn das Feld leer bleibt.

- `name="input-name"`  
  → Name des Feldes für Angulars Formular-API. Wichtig für die Validierung und
  das Referenzieren im Template.

- `id="input-name"`  
  → Eindeutige ID für das Feld, z.B. für das zugehörige `<label>`.

- `type="text"`  
  → Definiert das Eingabefeld als Textfeld.

- `minlength="5"`  
  → Das Feld ist erst gültig, wenn mindestens 5 Zeichen eingegeben wurden.

- `[placeholder]="!name.touched ? 'Gib bitte deinen Namen ein ' : 'Hoppla! dein Name fehlt.'"`  
  →
  Dynamischer Platzhalter: Vor dem ersten Klick steht ein neutraler Hinweis,
  danach ein Warnhinweis, falls das Feld leer bleibt.

- `required`  
  → Das Feld ist ein Pflichtfeld.

- `#name="ngModel"`  
  → Template-Referenzvariable: Damit kann im Template auf den Status und die
  Validierung des Feldes zugegriffen werden (z.B. `name.invalid`,
  `name.touched`, `name.dirty`).

**Zusammengefasst:**  
Mit diesen Bindings und Attributen erreichst du eine enge Verzahnung von
Benutzerinteraktion, Validierung und visuellem Feedback – alles direkt im
Template, ohne zusätzliche Logik im TypeScript-Code.

---

## Erklärung: Formular-Tag und Angular-Formularbindung

```html
<form (ngSubmit)="onSubmit(contactForm)" #contactForm="ngForm">
  <!-- ... -->
</form>
```

**Was passiert hier im Detail?**

- `(ngSubmit)="onSubmit(contactForm)"`  
  → Das Formular löst beim Absenden (z.B. Klick auf den Senden-Button oder
  Enter-Taste) das Angular-Event `ngSubmit` aus.  
  → Die Methode `onSubmit()` der Komponente wird aufgerufen und erhält das
  Formular-Objekt `contactForm` als Argument.  
  → So kann die Komponente auf alle Werte und den Status des Formulars zugreifen
  und z.B. eine Validierung oder einen HTTP-Request auslösen.

- `#contactForm="ngForm"`  
  → Erstellt eine Template-Referenzvariable namens `contactForm`, die auf das
  Angular-Formularobjekt (`NgForm`) verweist.  
  → Über diese Variable kann im Template auf den Status, die Gültigkeit und die
  Werte des gesamten Formulars zugegriffen werden (z.B. `contactForm.valid`,
  `contactForm.submitted`).

**Zusammengefasst:**  
Mit diesen beiden Bindings wird das Angular-Formular aktiviert, die Validierung
gesteuert und das Absenden sauber an die Komponenten-Logik übergeben.

---

## Schritt-für-Schritt: Kontaktformular in Angular

1. **Formular im Template anlegen**  
   Verwende `<form>` mit `ngForm` und binde die Felder mit `ngModel`.
2. **Validierung hinzufügen**  
   Nutze Angular-Validatoren wie `required`, `minlength`, `pattern` etc.
3. **Fehlermeldungen anzeigen**  
   Zeige Hinweise abhängig vom Status der Felder (`touched`, `invalid`,
   `dirty`).
4. **Absenden-Button deaktivieren**  
   Button bleibt deaktiviert, solange das Formular ungültig ist.
5. **Komponenten-Logik implementieren**  
   Sammle die Daten und sende sie per HTTP-Request an das Backend.
6. **Backend anbinden**  
   Beispiel: PHP-Skript, das die Daten per E-Mail weiterleitet.

---

## Methodenübersicht

| Feature                 | Beschreibung                               |
| ----------------------- | ------------------------------------------ |
| `ngModel`               | Zwei-Wege-Datenbindung für Formularfelder  |
| `required`, `minlength` | Validierung direkt im Template             |
| `ngForm`                | Formularstatus und Zugriff auf Validierung |
| `ngSubmit`              | Event beim Absenden des Formulars          |
| `HttpClient`            | Senden der Daten an das Backend            |

---

## Hinweise

- **Datenschutz:**  
  Immer eine Checkbox für die Zustimmung zur Datenverarbeitung einbauen.
- **Fehlermeldungen:**  
  Nutzerfreundliche Hinweise bei ungültigen Eingaben anzeigen.
- **Backend:**  
  Das Backend (z.B. PHP) muss CORS erlauben und die Daten korrekt verarbeiten.

---

## Beispiel: PHP-Backend für das Kontaktformular

```php
// sendMail.php
switch ($_SERVER['REQUEST_METHOD']) {
    case ("OPTIONS"):
        header("Access-Control-Allow-Origin: *");
        header("Access-Control-Allow-Methods: POST");
        header("Access-Control-Allow-Headers: content-type");
        exit;
    case("POST"):
        header("Access-Control-Allow-Origin: *");
        $json = file_get_contents('php://input');
        $params = json_decode($json);

        $email = $params->email;
        $name = $params->name;
        $message = $params->message;

        $recipient = 'deine@email.de';
        $subject = "Contact From <$email>";
        $message = "From:" . $name . "<br>" . $message ;

        $headers   = array();
        $headers[] = 'MIME-Version: 1.0';
        $headers[] = 'Content-type: text/html; charset=utf-8';
        $headers[] = "From: noreply@mywebsite.com";

        mail($recipient, $subject, $message, implode("\r\n", $headers));
        break;
    default:
        header("Allow: POST", true, 405);
        exit;
}
```

---

## Fazit

Mit Angular kannst du ein Kontaktformular schnell, flexibel und
benutzerfreundlich umsetzen.  
Durch die Kombination aus Template-Validierung, sauberer Komponenten-Logik und
Anbindung an ein Backend erhältst du ein professionelles und sicheres
Kontaktformular.

---
